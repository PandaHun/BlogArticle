# Intellij Code Convention 자동 적용기

## 서론

프로젝트를 진행하기에 앞서 정하는 여러 규칙 중에 `Code Convention`이 있습니다.

만약 정착된 룰이라면 상관 없지만, 새로운 인원 등 여러 사항에 의해서 깨지는 모습이 있을 수 있습니다.

(물론 잘 정돈된 개발 문화라면 이미 Lint가 있을 겁니다)

그래서 최소한의 `Code Convention`을 맞추기 위한 설정을 알려드리려고 합니다.

필자는 `IntelliJ`를 기준으로 2021.03 버전을 사용하고, Mac OS 환경입니다.

---

## 본론

### Save Actions Plugin

<img src="https://user-images.githubusercontent.com/13096845/114562103-27702880-9ca9-11eb-9fd0-4f166f8559f9.png" alt="image" style="zoom:50%;" />

`Intellij`에 `Save Actions` Plugin을 설치해 줍니다.

<img src="https://user-images.githubusercontent.com/13096845/114562719-b8470400-9ca9-11eb-9d29-34ccd51a2bf4.png" alt="image" style="zoom: 50%;" />

설치를 완료하면 메뉴 바에 `Save Actions`가 추가 된 것을 알 수 있습니다

![image](https://user-images.githubusercontent.com/13096845/114563087-0d831580-9caa-11eb-840c-4faf2b6ae00d.png)

해당 메뉴 중 5가지 만을 사용하고 있습니다.

### 사용 예시

<img src="https://user-images.githubusercontent.com/13096845/114563876-c9444500-9caa-11eb-8af9-b83a43209065.png" alt="image" style="zoom: 50%;" />

작성한 코드에서 `Unused Import`가 존재하지만

<img src="https://user-images.githubusercontent.com/13096845/114564029-e8db6d80-9caa-11eb-88be-99a645d8a936.png" alt="image" style="zoom:50%;" />

저장을 하게되면 해당 `Import`는 사라집니다.

---

## 결론

Save Action Plugins를 사용해서 자동 정렬 및 `Import` 관리를 알아보았습니다.