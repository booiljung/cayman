# OpenCV 빌드 방법

## 주의

### 파일 시스템

OpenCV를 윈도우나 리눅스에서 CMake로 빌드시 NTFS나 ext4 파일 시스템에서 진행해야 합니다. 다른 파일 시스템에서 tar 파일을 압축을 해제하지 못하여 오류가 발생 합니다.

## 빌드 도구 준비

먼저 빌드 도구를 설치합니다.

```sh
sudo apt update
sudo apt install build-essential
sudo apt install git cmake cmake-gui
```

git을 처음 설치하면 [글로벌 변수를 설정](../git/install_git_on_ubnutu.md)합니다.

## 의존 개발 패키지 설치

[`eigen3`](https://launchpad.net/ubuntu/bionic/+package/libeigen3-dev) 를 설치합니다.

```sh
sudo apt install libeigen3-dev
```

## 소스 다운로드

`opencv` 를 다운로드 합니다.

```sh
git clone --recursive https://github.com/opencv/opencv
```

다음은 `opencv_contrib`를 다운로드 합니다.

```sh
git clone --recursive https://github.com/opencv/opencv_contrib
```

## cmake-gui를 통한 구성

`cmake-gui`를 실행 합니다.

`Browse Source ...` 버튼을 눌러서 다운로드 한  `opencv ` 폴더를 지정해 줍니다.

`Brows Build ...` 버튼을 눌러서 빌드 폴더를 지정해 줍니다.

`Configure` 버튼을 누르면 컴파일러 등의 빌드 도구를 선택 하는 대화상자가 표시됩니다. 원하는 컴파일러를 선택 합니다.

CMake가 `CMakeLists.txt` 를 읽어들여 파싱하여 구성을 할 것입니다. 구성이 끝날때까지 기다립니다. 아래 출력 창에 `Configuring done`이 표시되면 성공적으로 1차 구성되었습니다.

CMake 변수들이 표시되고 이를 선택할 수 있습니다. 

먼저 검색창에 `eigen`을 입력하여 해당 변수들을 찾습니다.

`EIGEN_INCLUDE_PATH` 변수에 다운로드한 `libeigen` 경로를 지정합니다. (apt 로 설치한 경우 디폴트 /usr/lib로 가능 한지 확인 필요)

다음 검색창에 `test`를 입려하여 해당 변수들을 찾습니다.

테스트 관련 모든 변수를 `false`로 지정합니다.

다음 검색창에 `extra`를 입력하여 해당 변수들을 찾습니다.

`OPENCV_EXTRA_MODULES_PATH`에 다운로드한 `opencv_contribute` 폴더 안의 `modules` 경로를 지정합니다.

다음 검색창에 `nonfree`를 입력하여 해당 변수들을 찾습니다.

`OPENCV_ENABLE_NONFREE` 변수를 `true`로 지정 합니다.

다음 검색창에 `install`을 입력하여 해당 변수들을 찾습니다.

`CMAKE_INSTALL_PREFIX`의 기본 값은 현재 폴더의 경로입니다. 이것을 원하는 경로로 변경합니다.

필요에 따라 `INSTALL_C_EXAMPLES`등의 변수를 `true`로 지정합니다.

다음 검색창에 `build`를 입력하여 빌드할 타겟들을 지정합니다.

빌드 시간을 단축하기 위해 필요 없는 빌드 타겟을 끕니다. `BUILD_opencv_ts`, `BUILD_JAVA`, `BUILD_PACKAGE`를 `false`로 지정 할 수도 있습니다.

다음 검색창에 `with`를 입력하여 해당 변수들을 찾습니다.

요즘 1394는 사용하지 않으므로 `WITH_1394` 에 `false`를 지정합니다.

`WITH_GSTREAMER` 변수에 `false`를 지정합니다.

`WITH_VTK` 변수에 `false`를 지정합니다.

`WITH_LAPACK` 변수에 `false`를 지정합니다.



변수 값을 변경하였으면 다시 `Configure` 버튼을 눌러서 재구성을 합니다.  `Configuring done.`이 표시되면 재구성이 성공하였습니다.



검색창에 `world`를 입력하여, `BUILD_opencv_world`를 `true`로 지정합니다.



다시 `Configure` 버튼을 눌러서 재구성을 합니다.  `Configuring done.`이 표시되면 재구성이 성공하였습니다.



구성이 끝났으면 `Generate`버튼을 눌러서 Makefile을 생성합니다. 생성이 끝나면 `Generating done.` 이 표시됩니다.



### Windows 10

Generating 을 하고나서 `Open Project`  버튼을 누르면 Visual Studio가 솔루션 파일을 오픈합니다.

원하는 빌드 구성을 Debug나 Release중 선택하고, 솔루션 탐색기에서 `CMakeTarget`에서 `INSTALL`에서 콘텍스 메뉴를 표시하여 `빌드` 메뉴를 선택 합니다.

빌드가 완료되면 `OUTPUT` 패인에 `Build: Success: ??, Fail: 0 ...`이 표시 됩니다.



## 참조

- [빌드 동영상 튜토리얼](https://webnautes.tistory.com/1036)