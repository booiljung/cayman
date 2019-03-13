# OpenCL Basic

## OpenCL 이란?

Open Computing Language는 C99 언어로  CPU, GPU, DSP, FPGA에 프로그래밍을 하기 위한 프레임워크입니다. OpenCL은 Apple에 의해 시작되었고, 이어서 AMD, IBM, 인텔, 엔비디아에 의해 제안서가 작성되어, 현재 크로노스 그룹에 의해 관리되고 있으며 개방형 표준입니다.

OpenCL의 목표는 CPU, GPU 및 기타 프로세서, 스마트 폰에서 수퍼 컴퓨터등 이기종간에 이전이 가능한 병렬 프로그래밍을 위한 프레임워크입니다.

## 플랫폼 모델



![1552371373884](/home/booil/.config/Typora/typora-user-images/1552371373884.png)

호스트는 컴퓨터의 메인보드에 있는 프로세서입니다. 하나의 호스트는 1개 이상의 컴퓨트 장치를 가질 수 있습니다. 각각의 컴퓨트 장치는 1개 이상의 컴퓨트 유닛을 가질 수 있으며, 각각의 컴퓨트 유닛은 1개 이상의 프로세싱 엘리먼트를 가질 수 있습니다.

메모리는 호스트 메모리와 장치 메모리로 분리됩니다.

## 메모리 모델

![1552372225738](/home/booil/.config/Typora/typora-user-images/1552372225738.png)

- Host memory: CPU에 연결되어 있습니다.
- Global and Constant Memory: 모든 Work group에 의해 액세스 할 수 있습니다.
- Local memory: Work group내의 Work item들이 함께 공유합니다.
- Private memory: Work item의 메모리 입니다.

## 맛보기 코드

다음은 CPU에서 수행되는 전통적인 `n`개의 배열 곱셈을 수행하는 코드입니다.

```c
void mul(const int n, const float *a, const float *b, const float *c)
{
    for (int i = 0; i < n; i++)
        c[i] = a[i] * b[i];
}
```

다음은 OpenCL로 수행하는 코드이며 이 커널 함수가  1개 이상의 프로세싱 유닛에 의해 동시에 수행 됩니다.

```c
__kernel void mul(__global const float *a, __global const float *b, __global float *c)
{
    int id = get_global_id(0);
    c[id] = a[id] * b[id];
}
```

## 콘텍스트와 명령큐

![1552374994162](/home/booil/.config/Typora/typora-user-images/1552374994162.png)

콘텍스트(Context)는 커널이 실행되고 동기화 및 메모리 관리가 정의되는 환경을 말하며, 한개 이상의 컴퓨트 장치, 그 장치의 메모리, 한개 이상의 메모리를 포함합니다.

커널을 실행하거나, 동기화하고, 메모리를 조작하는 장치에 보내는  모든 명령(Command)은 명령대기열(Command-Queue)에 의해 보내지며, 각각의 명령대기열은 콘텍스트안에서 단일 장치를 가리킵니다.

## 실행모델

OpenCL 실행모델 (Execution model)은 문제 도메인을 정의하고 도메인의 각 지점에서 커널의 인스턴스를 실행합니다.

kernel: 실행가능한 코드의 기본 단위 입니다.

program: 커널과 다른 함수들의 집합 입니다.

application queue kernel execution instances: 커널은 도착 하는대로 적재되고, 도착하는 순서로 실행됩니다. 

다음 커널 샘플 코드를 보겠습니다.

```c
__kernel void times_two(__global float *input, __global float *output)
{
    int id = get_global_id(0)
    output[id] = 2.0f * input[id];
}
```

1. `get_global_id(0)`에 의해 현재 프로세싱 유닛의 ID를 얻습니다.
2. `input`의 정해진 id위치에서 현재 프로세싱 유닛이 처리할 데이터를 읽습니다.
3. `2.0f`를 곱하여 `output`의 정해진 위치에 저장합니다.

만일 16개의 배열을 처리하고, 현재 프로세싱 유닛이 처리해야 할  `get_global_id(0)`의 반환 값이  `5`이라면 다음과 같습니다.

![1552375957863](/home/booil/.config/Typora/typora-user-images/1552375957863.png)

## 프로그램 오브젝트 빌드 

프로그램 오브젝트 (Program Object)는 콘텍스트, 프로그램 소스 또는 바이너리, 타겟 컴퓨트 장치 목록과 빌드 옵션을 캡슐화 합니다. 

```c
__kernel void times_two(__global float *input, __global float *output)
{
    int id = get_global_id(0)
    output[id] = 2.0f * input[id];
}
```

GPU나 CPU를 위해 컴파일 될 수 있습니다.

## 벡터의 합 예제

전통적인 방법으로 N차원의 벡터끼리 더하는 방법은 다음과 같습니다.

```c
void vadd(const float *a, const float *b, float *c, const int N)
{
    for (int i = 0; i < N; i++)
    	c[i] = a[i] + b[i];
}
```

이 전통적인 코드는 호스트에서만 실행될 것입니다.

OpenCL을 위한 벡터의 합은 다음과 같습니다.

```c
__kernel void vadd(__global const float a, __global const float b, __global float c)
{
    int id = get_global_id(0);
    c[id] = a[id] + b[id];
}
```

OpenCL은 호스트를 위한 코드와 커널을 위한 코드로 2부분으로 나눠진 코드를 생성합니다.



## 호스트 프로그램

호스트 프로그램 (host program)은 호스트에서 실행되며 다음을 수행 합니다.

- OpenCL 프로그램을 위한 환경을 설정합니다.
- 커널 (kernel)을 생성하고 관리 합니다.

기본적으로 호스트 프로그램은 다음 5단계를 밟습니다.

- 플랫폼을 정의합니다. 플랫폼은 컴퓨트 장치, 콘텍스트, 명령대기열을 포함합니다.
- 커널 프로그램을 생성하고 빌드 합니다. 커널 프로그램은 동적 라이브러리입니다.
- 메모리 오브젝트를 설정합니다.
- 커널 함수에 인자를 붙여서 커널을 정의 합니다.
- 명령을 인가하여 메모리 오브젝트를 전송하고 커널을 실행하게 합니다.

## C++ 인터페이스

크로노스 그룹은 OpenCL을 위한 높은 수준의 인터페이스를 포함하는 공용  C ++ 헤더 파일 cl.hpp를 정의했으며 주요 특징은 다음과 같습니다.

- 플랫폼 및 명령대기열에 대한 공용 기본값을 사용하여 프로그래머가 가장 일반적인 사용 사례에 대한 추가 코딩을 하지 않아도 됩니다.

- 반복적인 인수 목록을 필요로 하지 않고 주요 매개 변수를 객체와 묶음으로써 기본 API를 단순화합니다.
- 일반 함수처럼 호스트의 커널 - C ++ 예외로 오류 검사를 수행 할 수 있습니다.

## 호스트 프로그램 설정

### 예외

예외를 활성화 하려면 `__CL_ENABLE_EXCEPTION`을 정의 합니다.

```c
#define __CL_ENABLE_EXCEPTION
```

이것은 `cl.hpp`를 포함하기 전에 정의하거나 컴파일 명령어 줄에 기술 합니다.

### 헤더파일

보통 호스트 프로그램은 다음 헤더파일들을 포함합니다.

```c++
#include <CL/cl.hpp>
#include <cstdio>
#include <iostream>
#include <vector>
```

### 콘텍스트 생성

사용할 장치 타입을 지정하여 콘텍스트를 생성 합니다.

```c++
cl::Context context(CL_DEVICE_TYPE_DEFAULT);
```

### 명령대기열 생성

생성한 콘테스트에 명령대기열을 생성합니다.

```c++
cl::CommandQueue queue(context);
```

명령은 다음을 포함합니다.

- 커널 실행 (kernel executions)
- 메모리 오브젝트 관리 (Memory object managements)
- 동기화 (Synchronization)

명령은 명령대기열을 통해 일방향으로 컴퓨트 장치로 전송할 수 있으며, 콘텍스트 안에서 각각의 명령대기열은  하나의 컴퓨트 장치를 가리킵니다.

동기화가 필요하지 않은 독립적인 명령 스트림을 사용 한다면 하나이 컴퓨트 장치에 다수의 명령대기열을 가리키게 할 수 있습니다.

### 명령대기열의 실행

명령대기열은 명령을 어떻게 실행할지 제어핟록 다르게 구성 됩니다.

- 순차대기열: 명령은 대기열에 들어가고 호스트 프로그램에 나타나는 순서대로 완료됩니다.
- 비순차대기열: 명령은 프로그램 순서로 대기열에 포함되지만 임의의 순서로 실행 (따라서 완료) 될 수 있습니다.

명령대기열에서 명령의 실행은 동기화 지점에서 완료되도록 보장됩니다.

## 프로그램의 생성과 빌드

커널 프로그램의 소스 코드를 문자열 리터럴 또는 파일에서 읽도록 정의 합니다.

프로그램 오브젝트를 만들고 컴파일하여 특정 커널을 가져올 수있는 "동적 라이브러리"를 생성합니다.

```c++
cl::Program program(context, KernelSource, true);
```

두번째 인자 `KernelSource`는 호스트 프로그램에서 정적으로 설정되거나 파일에서 커널 코드를 로드하는 함수에서 반환되는 문자열입니다.

세번째 인자 `true`는 OpenCL이 프로그램 오브젝트를 컴파일하고 빌드하다록 지정합니다.

## 메모리 오브젝트 설정

이전의 예제 커널 함수 `vadd`를 보겠습니다.

```c
__kernel void vadd(__global const float a, __global const float b, __global float c)
{
    int id = get_global_id(0);
    c[id] = a[id] + b[id];
}
```

`vadd`의 경우, 입력 벡터 `a`와 `b` 각각에 대해 하나씩, 출력 벡터 `c`에 대해 하나씩, 3개의 메모리 객체를 필요로 합니다.

호스트 상에서 할당할 입력 벡터 변수를 생성 합니다.

```c
#define N 16
std::vector<float> h_a(N), h_b(N), h_c(N);
for (i = 0; i < N; i++)
{
    h_a[i] = i * 1.0f;
    h_b[i] = i * 2.0f;
}
```

OpenCL 장치 버퍼를 정의하고 호스트 버퍼로 부터 복사해옵니다.

```c
cl::Buffer d_a(context, h_a.begin(), h_a.end(), true);
cl::Buffer d_b(context, h_b.begin(), h_b.end(), true);
cl::Buffer d_c(context, CL_MEM_WRITE_ONLY, sizeof(float)*N);
```

여기서 세번째 장치 버퍼 `d_c`는 `CL_MEM_WRITE_ONLY`로 지정되었습니다. 이 파라미터는

- `CL_MEM_READ_ONLY`
- `CL_MEM_WRITE_ONLY`
- `CL_MEM_READ_WRITE`

중에서 선택 할 수 있습니다.

메모리 오브젝트는 글로벌 영역에 있으며 참조 카운터를 다루며 두가지 종류가 있습니다.

### 버퍼 오브젝트 (Buffer object)

선형 바이트 collection을 정의하며, 버퍼 오브젝트의 내용은 커널 내에서 완전히 노출되어 있으며 포인터를 사용하여 액세스 할 수 있습니다.

### 이미지 오브젝트 (Image object)

2차원 또는 3차원 메모리 영역을 정의 하며, 이미지 데이터는 읽기 및 쓰기 기능으로만 액세스 할 수 있습니다. 즉, 불투명 한 데이터 구조입니다. 읽기 함수는 샘플러를 사용합니다.

## 버퍼 오브젝트 생성 및 조작

호스트상에서 버퍼 오브젝트의 타입은 `cl:Buffer`로 정의 됩니다. 호스트 메모리의 배열은 원래의 호스트 측 데이터를 보유합니다.

```c++
std::vector<float> h_a, h_b;
```

장치 측 버퍼 `d_a`를 만들고, 호스트 배열 `h_a`을 보유하고 읽기 전용 메모리를 할당하여 장치 메모리에 복사합니다.

```c++
cl::Buffer d_a(context, h_a.begin(), h_a.end(), true);
```

마지막 인수는 장치에 의한 버퍼에 대한 읽기 / 쓰기 액세스를 설정합니다. `true`는 "읽기 전용"을 의미하고 `false` (기본값)는 "읽기 / 쓰기"를 의미합니다.

장치 버퍼를 배열의 호스트 메모리로 다시 복사하라는 명령을 내 보냅니다.

```c++
cl::copy(queue, d_c, h_c.begin(), h_c.end());
```

장치 버퍼에 호스트 메모리 복사 할 수 있습니다.

```c++
cl::copy(queue, h_c.begin(), h_c.end(), d_c);
```

## 커널 정의 하기

프로그램 오브젝트 안의 커널을 후촐 하기 위해 커널 함수자(kernel functor)를 정의 합니다.

```c++
cl::make_kernel<cl::Buffer, cl::Buffer, cl::Buffer> vadd(program, "vadd");
```

여기서 `<cl:Buffer, cl:Buffer, cl::Buffer>`는 커널 인자의 패턴과 일치해야 하며, `program`은 사전에 생성된 프로그램 오브젝트로 커널 동적 라이브러리로 제공 되며, `"vadd"`는 사용될 커널 함수의 이름입니다.

즉, 커널을 대기열에 넣기 위해 호스트 코드에서 커널을 '함수'로 '호출'할 수 있습니다.

### 대기열에 명령을 넣기

커널을 런치하기 위해서는 글로벌 차원과 로컬 차원을 구분해야 합니다.

```c++
cl::NDRange global(1024)
cl::NDRange local(64)
```

로컬 차원을 구분하지 않으면, `cl::NullRange`로 간주되며, 런타임에 크기가 선택됩니다.

다음은 커널을 실행하기 위해 대기열에 삽입 합니다. 이것은 논블로킹 조작으로 바로 반환 됩니다.

```c++
vadd(cl::EnqueueArgs(queue, global), d_a, d_b, d_c);
```

블로킹 조작에서 반환 값을 바로 읽습니다. 이전 명령이 읽기가 시작되기 전에 완료되었는지 확인하기 위해 순서대로 대기열을 사용합니다.

```c++
cl::copy(queue, d_c, h_c.begin(), h_c.end());
```

### 예제: vadd 프로그램

커널은 모든 동작이 OpenCL 프로그램에 있으며, 커널 사용 단계는 다음과 같습니다.

1. 파일로부터 커널 소스 코드를 프로그램 오브젝트에 로드 합니다.
2.  커널 함수자를 프로그램 내의 함수에서 만듭니다.
3. 장치 메모리 초기화 합니다.
4. 메모리 오브젝트와 전역/로컬 크기를 지정하여 커널 함수자를 호출 합니다.
5. 장치에서 결과를 다시 읽습니다.

커널 함수 인수 목록은 호스트의 커널 정의와 일치해야 합니다.

```c++
// 호스트 벡터 초기화
std::vector<float> h_a(N), h_b(N), h_c(N);

// 디바이스 버퍼 선언
cl::Buffer d_a, d_b, d_c;

cl::Context context(CL_DEVICE_TYPE_DEFAULT);

cl::CommandQueue queue(context);

// 파일에서 커널 프로그램 로드 및 컴파일
cl::Program program(context, loadprogram("vadd.cl"), true);

// 커널 함수자 생성
cl::make_kernel<cl::Buffer, cl::Buffer, cl::Buffer, int> vadd(program, "vadd");

// 장치 버퍼 생성
d_a = cl::Buffer(context, h_a.begin(), h_a.end(), true); // CL_MEM_READ_ONLY
d_b = cl::Buffer(context, h_b.begin(), h_b.end(), true); // CL_MEM_READ_ONLY
d_c = cl::Buffer(context, CL_MEM_READ_WRITE, sizeof(float) * N);

// 명령대기열에 삽입
vadd(cl::EnqueueArgs(queue, cl::NDRange(count)), d_a, d_b, d_c, count);
cl::copy(queue, d_c, h_c.begin(), h_c.end());
```

## 




















## 참조

- [OpenCL hands on intro](https://www.nersc.gov/assets/pubs_presos/MattsonTutorialSC14.pdf)

- [OpenCL Basics](https://www.fz-juelich.de/SharedDocs/Downloads/IAS/JSC/EN/slides/opencl/opencl-03-basics.pdf)