# tensorflow_source

## Install
### macOS Monterey (Apple M1 Max)

```sh
brew install bazel
```

```sh
git clone 
cd tensorflow
git checkout r2.8
./configure

bazel build --config=macos_arm64 --config=dbg --local_ram_resources=200 --local_cpu_resources=10 --verbose_failures tensorflow/tools/pip_package:build_pip_package
```

```
生产wheel
./bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg
```

```
解决error:Could not find a version that satisfies the requirement tensorflow-io-gcs-filesystem>=0.23.1 (from tensorflow) (from versions: none)
git clone https://github.com/tensorflow/io.git
cd io
python setup.py -q bdist_wheel --project tensorflow_io_gcs_filesystem
pip install --no-deps dist/tensorflow_io_gcs_filesystem-0.25.0-cp39-cp39-macosx_12_0_arm64.whl
python setup.py -q bdist_wheel
pip install --no-deps dist/tensorflow_io-0.25.0-cp39-cp39-macosx_12_0_arm64.whl
```

#### Errors  

1. `ERROR: xxx/external/local_config_cc/BUILD:48:19: in cc_toolchain_suite rule @local_config_cc//:toolchain: cc_toolchain_suite '@local_config_cc//:toolchain' does not contain a toolchain for cpu 'darwin_arm64'`

```
vim $(bazel info output_base)/external/local_config_cc/BUILD
```
```
 48 cc_toolchain_suite(
 49     name = "toolchain",
 50     toolchains = {
 51         "darwin|clang": ":cc-compiler-darwin",
 52         "darwin_arm64|clang": ":cc-compiler-darwin",                // add
 53         "darwin": ":cc-compiler-darwin",
 54         "darwin_arm64": ":cc-compiler-darwin",                      // add
 55         "armeabi-v7a|compiler": ":cc-compiler-armeabi-v7a",
 56         "armeabi-v7a": ":cc-compiler-armeabi-v7a",
 57     },
 58 )
```

```
 60 cc_toolchain(                                  // add
 61     name = "cc-compiler-darwin_arm64",         // add
 62     toolchain_identifier = "local",            // add
 63     toolchain_config = ":local",               // add
 64     all_files = ":compiler_deps",              // add
 65     ar_files = ":compiler_deps",               // add
 66     as_files = ":compiler_deps",               // add
 67     compiler_files = ":compiler_deps",         // add
 68     dwp_files = ":empty",                      // add
 69     linker_files = ":compiler_deps",           // add
 70     objcopy_files = ":empty",                  // add
 71     strip_files = ":empty",                    // add
 72     supports_param_files = 1,                  // add
 73     module_map = ":module.modulemap",          // add
 74 )                                              // add
 75 cc_toolchain(
 76     name = "cc-compiler-darwin",
 77     toolchain_identifier = "local",
 78     toolchain_config = ":local",
 79     all_files = ":compiler_deps",
 80     ar_files = ":compiler_deps",
 81     as_files = ":compiler_deps",
 82     compiler_files = ":compiler_deps",
 83     dwp_files = ":empty",
 84     linker_files = ":compiler_deps",
 85     objcopy_files = ":empty",
 86     strip_files = ":empty",
 87     supports_param_files = 1,
 88     module_map = ":module.modulemap",
 89 )
```

```
bazel clean --expunge
```

2. `ERROR: xxx/tensorflow/lite/python/BUILD:62:10 Middleman _middlemen/tensorflow_Slite_Spython_Stflite_Uconvert-runfiles failed: (Exit 1): bash failed: error executing command`



