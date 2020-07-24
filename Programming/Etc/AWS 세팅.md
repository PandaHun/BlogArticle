프로젝트를 진행하면서 서버를 구축했을 때 생긴 에러들과 해결한 방법들을 적어놓았습니다.

# 필요 환경 구축하기

우선 필요 환경은 `mariadb`, `java`, `node`, `nignx`입니다.

## Java 설치

우선 설치 되어 있는 지 확인을 위해

```bash
$ javac -version
The program 'javac' can be found in the following packages:
```

Java가 설치되어 있다면 아래와 같이 나타납니다.

```bash
$ javac -version
javac 11.0.7
```

설치를 위해 아래와 같이 입력하면 됩니다. 이후 맨 처음처럼 확인 하시면 됩니다.

```bash
$ sudo apt-get install openjdk-11-jdk
```

## Node 설치

PPA(Personal Package Archive)를 이용해 설치합니다.

```bash
$ curl -sL <https://deb.nodesource.com/setup_12.x> | sudo -E bash -
```

후에 우분투에 NodeJS를 설치해줍니다.

```bash
$ sudo apt-get install -y nodejs
```

NodeJs와 NPM이 잘 깔렸는지 확인합니다.

```bash
$ node -v
$ npm -v
```

NPM이 제 기능을 하게 하기 위해 다음 명령어를 실행합니다. (요거 없으면 npm install 시 에러 날 확률이 높습니다.)

```bash
$ sudo apt-get install build-essential
```

## Mariadb 설치

아래와 같은 명령어를 사용해 설치합니다.

```bash
$ sudo apt-get install mariadb-server
```

정상적으로 설치 되었는지 확인을 위해서 아래와 같이 입력해, 정보가 뜨는지 확인합니다.

```bash
$ sudo mysql
```

## Nginx 설치

아래와 같은 명령어를 사용해 설치합니다.

```bash
$ sudo apt-get install nginx
```

정상적으로 설치 되었는지 확인을 위해서 `nginx`를 실행하고 확인해봅니다.

```bash
$ sudo service nginx start
$ ps -ef | grep nginx
```

이 때 `nginx`가 잡힌다면 정상적으로 동작하는 것 임을 알 수 있습니다.

# 외부 접속 허용하기

해당 서비스를 위해 `nginx`로 `reverse proxy`를 설정해야 하고, 로컬에서 개발하면서 들어가는 데이터를 위해 `mariadb`도 외부 접속을 허용해야 합니다.

우선 db부터 세팅하겠습니다.

## Mariadb 외부 접속 세팅하기

우선 현재 db의 상태를 확인해봅니다.

```bash
$ netstat -atnp | grep mysql
tcp    0    0   127.0.0.1:3306   0.0.0.0:*      LISTEN      22370/mysqld
```

정보를 보면 127.0.0.1:3306 로 내부에서만 접속이 허용 가능하게 되어있다.

이러한 정보를 저장하고 있는 IP 설정을 바꿔야 한다.

IP 설정을 확인해보겠습니다.

```bash
$ grep ^bind-address /etc/mysql/mariadb.conf.d/50-server.cnf
bind-address		= 127.0.0.1
```

이러한 설정을 없애거나 열어줘야 합니다.

vi 편집기를 열어 127.0.0.1에서 0.0.0.0으로 변경합니다

```bash
$ sudo vi /etc/mysql/mariadb.conf.d/50-server.cnf
bind-address		= 0.0.0.0
```

이후 `mariadb` 를 재시작하고 정보를 확인해봅니다.

```bash
$ sudo service mysql restart
$ netstat -atnp | grep mysql
tcp    0    0   0.0.0.0:3306   0.0.0.0:*      LISTEN      22370/mysqld
```

이후 원격에서 접속해 확인해봅니다.

```bash
$ mysql -u root -p -h [ip주소]
```

db 정보가 뜨면 접속이 정상적으로 진행 된겁니다!

## Nginx Reverse Proxy 설정

**리버스 프록시**

리버스 프록시란?

가짜 서버에 request가 들어왔을때, 프록시 서버가 배후 서버(reverse server)로 데이터를 요청해서 가져오는 것.

기본적으로 nginx는 비동기 처리방식을 채택하고 있다.

**장점**

Reverse 서버에 request에 대한 응답대기 프로세스를 생성하지 않아도 되는 장점이 생긴다.

보안상의 이점 : 또한 외부 사용자로부터 실제 내부망에 있는 서버의 존재와 정보를 숨길 수 있다.

로드밸런싱 : proxy서버가 내부 서버의 정보를 알고 관리하므로, 부하 여부에 따라 요청을 분배 할 수 있다.

### Nginx 설정

```bash
$ sudo vi /etc/nginx/nginx.conf
```

`nginx`설정 파일에서 호스팅 설정을 해준다.

```bash
http { 
	...

	server {
		listen 80;
		server_name      localhost;
		root /home/users/project  # document root 설정

		location / { # proxy_pass 설정. 특정 확장자 요청을 넘기는 설정
			proxy_pass <http://localhost:8080>;
			proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
			proxy_set_header Host $http_host;
		}
	}
	...
}
```

Proxy_pass : 요청이 올 경우 http://localhost:8080로 요청을 전달합니다.

proxy_set_header XXX : 실제 요청 데이터를 header의 각 항목에 할당하는 역할

이상으로 전체 필요 세팅을 마쳤습니다!