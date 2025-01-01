
# Bash 스크립트 작성 및 활용법

Bash 스크립트는 반복 작업을 자동화하고 효율적으로 시스템을 관리하는 데 유용함. 

이번 글에서는 Bash 스크립트 작성법, PATH 변수 설정, 실행 권한 부여, 조건문 사용, 그리고 날씨 정보를 제공하는 간단한 스크립트를 작성하는 방법을 다룸.

---

## Bash 스크립트 시작하기: Shebang!

스크립트의 첫 줄에 Shebang(#!)을 작성하면, 해당 스크립트를 실행할 때 사용할 인터프리터를 지정할 수 있음. 

예를 들어 Bash 스크립트를 작성하려면 아래와 같이 시작함:

```bash
#! /bin/bash

# my first script
echo "Hello there, $USER"
echo "Today is $(date)"
echo "last ran hi at $(date)" >> hi.log
```

위 코드를 `hi`라는 파일로 저장한 후 아래 명령으로 실행 가능:
```bash
bash hi
```

출력 예시:
```
Hello there, mane
Today is Wed Jan 1 15:09:33 UTC 2025
```

---

## PATH 변수 설정

`PATH`는 실행 파일을 찾는 경로를 정의함. 사용자 지정 스크립트를 실행하려면, 스크립트가 `PATH`에 포함된 디렉토리에 위치해야 함.

### .profile 수정
`~/.profile` 파일에 사용자 정의 경로를 추가:
```bash
if [ -d "$HOME/bin" ] ; then
    PATH="$HOME/bin:$PATH"
fi
```
수정 후 변경 사항을 적용하려면:
```bash
source .profile
```

### .bashrc 수정
`~/.bashrc` 파일에 아래 내용을 추가해도 동일한 효과를 얻을 수 있음:
```bash
PATH="$HOME/bin:$PATH"
```

---

## 실행 권한 부여

스크립트 파일에 실행 권한을 부여하려면 `chmod` 명령어를 사용:
```bash
chmod 755 hi
```
이제 스크립트를 직접 실행 가능:
```bash
./hi
```

---

## 조건문과 스크립트 확장

스크립트에서 조건문을 사용하면 다양한 입력에 따라 다른 동작을 수행할 수 있음. 아래는 간단한 조건문 예제:

```bash
case $1 in
-h | --help)
    echo "WELCOME TO WEATHER HELP"
    ;;
-3)
    echo "YOU PROVIDED -3"
    ;;
*)
    echo "ANY OTHER VALUE"
    ;;
esac
```

---

## 날씨 정보를 제공하는 Bash 스크립트

`wttr.in` 서비스를 활용하여 날씨 정보를 출력하는 스크립트를 작성할 수 있음:

```bash
#!/bin/bash

case $1 in
-h | --help)
    echo "WELCOME TO WEATHER HELP"
    echo "-3 for next 3 days of weather"
    ;;
-3)
    echo "YOU PROVIDED -3"
    curl "wttr.in/"
    ;;
-l | --location)
    curl "wttr.in/$2"
    ;;
*)
    curl "wttr.in?m1"
    ;;
esac
```

### 실행 예시

- 기본 날씨 정보 출력:
  ```bash
  ./weather
  ```
- 특정 지역 날씨 정보:
  ```bash
  ./weather -l Seoul
  ```
- 도움말 확인:
  ```bash
  ./weather -h
  ```

출력 예시:
```
mane@9c2dc75278e0:~/bin$ weather
Weather report: Seoul, South Korea

      \   /     Clear
       .-.      -2(-3) °C
    ― (   ) ―   → 5 km/h
       `-’      10 km
      /   \     0.0 mm
                                                       ┌─────────────┐
┌──────────────────────────────┬───────────────────────┤  Wed 01 Jan ├───────────────────────┬──────────────────────────────┐
│            Morning           │             Noon      └──────┬──────┘     Evening           │             Night            │
├──────────────────────────────┼──────────────────────────────┼──────────────────────────────┼──────────────────────────────┤
│    \  /       Partly Cloudy  │     \   /     Sunny          │     \   /     Clear          │     \   /     Clear          │
│  _ /"".-.     +1(0) °C       │      .-.      +5(3) °C       │      .-.      +4(1) °C       │      .-.      +2(0) °C       │
│    \_(   ).   ↑ 4-8 km/h     │   ― (   ) ―   → 8-11 km/h    │   ― (   ) ―   → 10-15 km/h   │   ― (   ) ―   → 5-8 km/h     │
│    /(___(__)  10 km          │      `-’      10 km          │      `-’      10 km          │      `-’      10 km          │
│               0.0 mm | 0%    │     /   \     0.0 mm | 0%    │     /   \     0.0 mm | 0%    │     /   \     0.0 mm | 0%    │
└──────────────────────────────┴──────────────────────────────┴──────────────────────────────┴──────────────────────────────┘

Follow @igor_chubin for wttr.in updates
...
```

---

## 결론

Bash 스크립트를 통해 작업을 자동화하고 효율성을 높일 수 있음.

이번 글의 내용을 참고하여 실습을 통해 자신만의 스크립트를 작성해 보길 권장함.

---