# tensorflow_source

## Install
### macOS Monterey (Apple M1 Max)

```sh
brew install bazel
```

```sh
git clone 
cd tensorflow
git checkout r2.7
./configure

bazel build --config=macos_arm64 --config=dbg --local_ram_resources=200 --local_cpu_resources=10 --verbose_failures tensorflow/tools/pip_package:build_pip_package
```
