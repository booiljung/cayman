[Up](./index.md)

# AVD 실행.

AVD란 Android Virtual Device를 말합니다. qeum를 사용하여 PC에서 안드로이드 장치를 가상으로 에뮬레이션 해줍니다. 기본적으로 안드로이드 스튜디오에서 AVD를 생성, 실행 합니다. 리액트 네이티브를 사용한다면 이 과정이 시간이 걸립니다. 안드로이드 스튜디오가 느려요.

생성된 ADV를 CLI에서 바로 올릴 수 있는데요. 부족한 점이 있어 잘 되지 않습니다. 여러 시도 끝에 여기에 기록 합니다.

환경 변수 PATH에 Android SDK를 지정하여도 AVD를 찾을 수 없거나 공유 라이브러리를 찾을 수 없다며 실행되지 않습니다. AVD를 실행하려면 반드시 `$ANDROID_HOME/tools`에서 해야 합니다.

다음 셸 스크립트를 $PATH 환경 변수 경로에 작성하여 잘 작동하게 할 수 있습니다.

```shell
pushd ~/android/sdk/emulator
./emulator $1 $2 $3 $4 $5 $6
popd
```

