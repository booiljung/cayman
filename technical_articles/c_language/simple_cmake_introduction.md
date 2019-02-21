# CMake 간단한 안내서

CMake는 [Kitware]()에서 개발하고 있으며, [cmake.org](https://cmake.org/)에서 학습하고 다운로드 할 수 있습니다. Makefile를 작성할때 의존관계를 개발자가 지정해야 하며, 정확한 빌드를 위해 많은 노력이 필요 합니다. 의존관계를 빠뜨리면 컴파일이 되지 않거나 잘못 컴파일 되어 런타임 오류가 발생하기도 합니다.  CMake는 소스파일을 파싱하여 의존관계를 파악하여 자동으로 Makefile이 작성되도록 돕는 것이 1차 목표 입니다.

## 특징

CMake는 다음과 같은 일을 할 수 있습니다.

## 스크립트

CMake는 별도로 지정하지 않는다면 `CMakeLists.txt`파일을 파싱하고 실행 합니다.

### 주석

```cmake
# 주석은 #문자로 지정합니다.
```

### 버전 제약: 예제

```cmake
cmake_minimum_required(VERSION 3.10)
```

이 cmake 스크립트는 3.10 이상 버전의 cmake를 필요로 합니다.

### 프로젝트 이름: project

```cmake
project(my_project)
```

프로젝트에서 언어와 버전을 지정할 수도 있습니다.

```cmake
prject(my_project LANGUAGE CXX VERSION 1.2.3)
```

### 실행 파일 빌드

```cmake
add_executable(my_executable main.cpp data.cpp network.cppp ...)
```

### 라이브러리 파일 빌드

```cmake
add_library(my_lib main.cpp data.cpp network.cpp ...)
```

#### 정적 라이브러리

```cmake
add_library(my_lib STATIC main.cpp data.cpp network.cpp ...)
```

#### 동적 라이브러리

```cmake
add_library(my_lib SHARED main.cpp data.cpp network.cpp ...)
```

### 서브 디렉토리

서브 디렉토리에는 별도의 `CMakeLists.txt`파일이 존재해야 합니다.

```cmake
add_subdirectory(my_subdirectory)
```

현재 디렉토리의 하위 디렉토리가 아니라면 빌드 디렉토리를 지정해야 합니다.

```cmake
add_subdirectory(../other_directory/subdirectory ./build/other_subdirectory)
```

## 실행

build 결과가 프로젝트에 포함되는 것을 막기 위해, 프로젝트 경로 밖에서 `build`폴더를 만들고, 프로젝트 경로를 지정하여 빌드하는 것이 일반적입니다. CMake는 Configure와 Generate 2단계로 구성 됩니다.

### Windows

```powershell
cmake ./project_path/ -G "visual Studio 15 2017 Win64"
```

### Unix/Linux

```bash
cmake ./project_path/ -G Ninja
```

### MacOS

```bash
cmake ./project_path/ -G XCode
```



## 참조

- [CMake Official Reference Documentation](https://cmake.org/documentation)
- [CMake 할때 쪼오오금 도움이 되는 문서](https://gist.github.com/luncliff/6e2d4eb7ca29a0afd5b592f72b80cb5c#cmake-%ED%8A%9C%ED%86%A0%EB%A6%AC%EC%96%BC-lv2)
- [CGold: The Hitchhiker’s Guide to the CMake](https://cgold.readthedocs.io/en/latest/)
- [CMake and Visual Studio](https://cognitivewaves.wordpress.com/cmake-and-visual-studio/)
- 