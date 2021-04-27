# 1\. JVM은 무엇이며 자바 코드는 어떻게 실행하는 것인가.

# JVM이란 무엇인가?

-   Java Virtual Machine: 자바 가상 머신
-   자바 프로그램을 실행하기 위해 물리적 머신과 유사한 머신을 소프트웨어로 구현 한 것
    -   이를 통해 `Write Once Run Anywhere`를 만들어줌
-   자바 애플리케이션을 클래스 로더를 통해 읽어 들여 자바 API와 함께 실행하는 것
-   JAVA와 OS의 중개자 역할을 수행
    -   JAVA가 OS에 구애받지 않고 재사용 가능하게 한다.
-   메모리 관리와 Garbage Collector기능을 수행한다.
-   스택 기반의 가상 머신

## 왜 알아야 하는가?

-   한정된 메모리에서 최고의 성능을 내기 위해서
    -   메모리의 효율을 위해 메모리 구조를 알아야 한다.
-   알아두면 좋다를 넘어 알아야 함

# 컴파일 하는 방법

컴파일 방법을 알아보기 이전에, Java 프로그램이 실행되는 과정을 먼저 알아 본다.

## Java 프로그램 실행 과정

1.  프로그램이 실행되면 JVM은 OS로 부터 메모리를 할당 받는다.
    -   JVM은 이 메모리를 용도에 따라 여러 영역으로 관리한다.
2.  **자바 컴파일러(javac)가 자바 코드(.java)를 읽어들여 자바 바이트 코드(.class)로 변환시킨다.**
3.  Class Loader를 통해 class파일들을 JVM에 로딩한다.
4.  로딩 된 class 파일들은 Execution Engine을 통해 해석된다.
5.  해석된 바이트 코드는 Runtime Data Areas에 배치되어 실질적인 수행이 이뤄진다.

![https://t1.daumcdn.net/cfile/tistory/25616D45576B854C3F](https://t1.daumcdn.net/cfile/tistory/25616D45576B854C3F)

## 컴파일 하는 방법

-   위 과정 중 2번에 해당되는 과정을 컴파일이라고 한다.
-   즉 JDK를 이용해 Java 파일을 Class 파일로 변환시키는 일련의 작업
-   우선 `Hello PandaHun`이라는 출력을 하는 Java 파일을 한개 만들고 시작한다.

```
$ javac Compile.java
```

-   `javac`명령어를 통해 java 파일을 컴파일 할 수 있다.

![https://user-images.githubusercontent.com/13096845/101272799-f64f8b00-37d2-11eb-8dc3-12c0a8e815fa.png](https://user-images.githubusercontent.com/13096845/101272799-f64f8b00-37d2-11eb-8dc3-12c0a8e815fa.png)

-   이렇게 생긴 .class 파일을 **바이트 코드**라고 한다.

## 실행하는 방법

-   위에서 만들어진 class 파일을 실행하는 방법을 알아보자
-   컴파일(compile)은 `javac` 명령어를 사용했고 실행은 `java` 명령어를 사용한다

```
$ java Compile
```

-   이때 확장자는 표시하지 않는다

![https://user-images.githubusercontent.com/13096845/101272815-17b07700-37d3-11eb-869d-73798f2e4e71.png](https://user-images.githubusercontent.com/13096845/101272815-17b07700-37d3-11eb-869d-73798f2e4e71.png)

# 바이트코드란 무엇인가

-   Java가 아닌 다른 언어에서는 컴파일은 프로그래밍 언어(고급언어)를 기계어로 바꾸는 역할을 수행한다.
-   하지만 Java에서 컴파일은 Java 코드를 Byte Code로 바꾸고 JVM이 Byte 코드를 기계어로 바꿔준다.
-   [위키백과](https://ko.wikipedia.org/wiki/%EB%B0%94%EC%9D%B4%ED%8A%B8%EC%BD%94%EB%93%9C)에서는 아래처럼 정의한다.
    -   특정 하드웨어가 아닌 가상 컴퓨터에서 돌아가는 실행 프로그램을 위한 이진 표현법이다. 하드웨어가 아닌 소프트웨어에 의해 처리되기 때문에, 보통 기계어보다 더 추상적이다.
-   JVM이 이해할 수 있고 JRE(Java Runtime Environment)에서 실행하기 위한 최적화된 코드의 집합이다.
-   각 OS에 맞는 JVM이 존재해 개발자는 Java Code를 작성하고, 바이트 코드로 바꿔줄 수 있다면, 그 이후는 JVM이 담당해 OS에 독립적이게 도와준다.

# JIT 컴파일러란 무엇이며 어떻게 동작하는지

-   JIT: Just In Time ⇒ 실시간
-   프로그램이 실제로 실행하는 시점에 기계어로 번역해주는 컴파일러
-   위의 그림에서 보듯 JRE 내에 존재하고 있다.
-   일반적으로 인터프리터 방식과 정적 컴파일 방식으로 나뉘는 기계어 번역
    -   인터프리터: 바이트 코드를 한줄씩 번역하면서 코드를 실행함.
        -   동일한 메소드도 계속해서 번역하는 비효율 발생
-   JIT는 이를 방지하고자 번역한 내용을 캐싱해두고 다시 사용될 때 번역하지 않고 캐싱된 결과를 사용한다

# JVM 구성 요소

### Class Loader

-   JVM 내로 class 파일을 로딩하고, 링크를 통해 배치 작업을 수행하는 모듈
-   Runtime시에 동적으로 class를 로드한다.
-   jar파일 내의 저장된 클래스를 JVM 위에 탑재하고, 사용하지 않는 클래스를 삭제한다.
-   컴파일러의 역할 수행
    -   동적코드를 컴파일 시가 아닌 런타임 시에 참조한다.

### Execution Engine

-   클래스를 실행시키는 역할
-   자바 바이트 코드는 기계보다는 사람을 위한 코드
-   실행 엔진은 이 코드를 JVM 내부에서 기계가 실행할 수 있는 형태로 변경한다.
    -   Interpreter
        -   자바 바이트 코드를 명령어 단위로 읽어서 실행한다.
        -   한 줄 씩 수행하기 때문에 느리다
    -   JIT(Just In Time)
        -   인터프리터의 단점을 보완하기 위해
        -   인터프리터 방식으로 실행하다가 적절한 시점에 바이트 코드 전체를 컴파일 해 네이티브 코드로 변경
            -   이후 인터프리팅하지 않고 네이티브 코드로 직접 실행한다.
        -   만약 한번만 실행되는 코드라면 JIT보다는 인터프리터 방식이 더 유용
        -   따라서 JVM은 메서드가 얼마나 자주 실행되는 지를 체크해 일정 수준 이상일 때만 수행한다.
    -   Garbage Collector

### Runtime Data Area

![](https://t1.daumcdn.net/cfile/tistory/275A103F576B85550D)

1.  PC Registrer
    -   Thread가 실행될 때 생성, 생성할 때마다 생성되는 공간으로 Thread마다 하나씩 존재
    -   현재 수행 중인 JVM 명령의 주소를 갖는다.
2.  JVM 스택 영역
    -   프로그램 실행 과정에서 임시로 할당 되었다가, 메서드가 종료되면 바로 소멸되는 특성의 데이터를 저장하는 영역
    -   변수, 임시 데이터, 메서드 등의 정보를 저장
    -   메서드 호출 시 마다 각각의 스택 프레임을 생성하고, 수행이 끝나면 프레임 별로 삭제한다.
3.  Native Method Stack
    -   바이트 코드가 아닌 실제로 실행 할 수 있는 기계어로 작성된 프로그램을 실행시키는 영역
    -   Java가 아닌 다른 언어로 작성된 코드를 위한 공간
    -   Java Native Interface를 통해 바이트 코드로 전환해 저장하게 된다.
    -   커널이 스택을 잡아 독자적으로 프로그램을 실행시키는 영역
4.  Method Area(=Class Area = static Class)
    -   클래스 정보를 처음 메모리 공간에 올릴 때 초기화 되는 대상을 저장하기 위한 메모리 공간
    -   올라가는 메서드의 바이트 코드는 프로그램의 흐름을 구성하는 바이트 코드
    -   Runtime Constant Pool이라는 별도의 관리 영역
        -   상수 자료형을 저장하고, 참조하고, 중복을 막는 역할 수행
5.  Heap 영역
    -   객체를 저장하는 가상 메모리 공간
    -   `new` 연산으로 생성된 객체와 배열을 저장한다.
    -   class area에 올라온 클래스들만 객체로 생성할 수 있다.

![https://t1.daumcdn.net/cfile/tistory/266E283B576B8E060B](https://t1.daumcdn.net/cfile/tistory/266E283B576B8E060B)

-   Permanent Genration
    -   생성된 객체들의 주소가 저장된 공간
    -   Class loader에 의해 로드되는 클래스, 메서드 등의 Meta 정보가 저장되는 영역이고, JVM에 의해 사용된다
-   New/Young
    -   Eden: 객체가 최초로 생성되는 공간
    -   Survivor 0/1: Eden에서 참조되는 객체들이 저장되는 공간
-   Old 영역
    -   Survivor 영역에서 살아남아 오랫동안 참조 되었고 앞으로도 사용 확률이 높은 객체를 저장하는 영역

# JDK와 JRE

-   JDK = Java **Development Kit**
    -   Java 응용 프로그램 및 개발에 필요한 소프트웨어 개발 환경
    -   안에 JRE, Java, Compiler 등 필요한 툴이 들어있다.
-   JRD = Java Runtime Environment
    -   Java 응용 프로그램이 돌아가기 위한 최소한의 환경을 제공한다.
    -   JVM, 코어 클래스 및 지원 파일이 들어가있다.

![https://user-images.githubusercontent.com/13096845/101272799-f64f8b00-37d2-11eb-8dc3-12c0a8e815fa.png](https://user-images.githubusercontent.com/13096845/101272799-f64f8b00-37d2-11eb-8dc3-12c0a8e815fa.png)