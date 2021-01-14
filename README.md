# `bazel-build_python_zip-bug-reproduction`

Reproduce the bug by cloning this repository and then running the following: 

```
bazel build //spark_hello_world:main_zip
```

### Expected


```
INFO: Analyzed target //app/datadog/bin/dashboard_import:import_datadog_dashboard (0 packages loaded, 137 targets configured).
INFO: Found 1 target...
Target //spark_hello_world:main_zip up-to-date
  bazel-bin/spark_hello_world/main_zip.zip
```


### Actual

```
INFO: Analyzed target //spark_hello_world:main_zip (0 packages loaded, 0 targets configured).
INFO: Found 1 target...
ERROR: /Users/jonathon/work/reproduce_zipapp_bug/spark_hello_world/BUILD:4:10: PythonZipper spark_hello_world/main.zip failed (Exit 255): zipper failed: error executing command external/bazel_tools/tools/zip/zipper/zipper cC bazel-out/darwin-fastbuild/bin/spark_hello_world/main.zip @bazel-out/darwin-fastbuild/bin/spark_hello_world/main.zip-0.params

Use --sandbox_debug to see verbose messages from the sandbox zipper failed: error executing command external/bazel_tools/tools/zip/zipper/zipper cC bazel-out/darwin-fastbuild/bin/spark_hello_world/main.zip @bazel-out/darwin-fastbuild/bin/spark_hello_world/main.zip-0.params

Use --sandbox_debug to see verbose messages from the sandbox
File kittens/date=2018-01/not-image.txt=external/pypi/pypi__pyspark/pyspark/data/mllib/images/partitioned/cls=kittens/date=2018-01/not-image.txt does not seem to exist.Target //spark_hello_world:main_zip failed to build
Use --verbose_failures to see the command lines of failed build steps.
INFO: Elapsed time: 0.286s, Critical Path: 0.11s
INFO: 2 processes: 2 internal.
FAILED: Build did NOT complete successfully
```

### Investigation 

I'm pretty sure this bug occurs because a file within the `pyspark` package contains an `=` character which is breaking some params file parsing
logic in the zipper: 

```
cat bazel-out/darwin-fastbuild/bin/spark_hello_world/main.zip-0.params | grep kittens

runfiles/pypi/pypi__pyspark/pyspark/data/mllib/images/origin/kittens/not-image.txt=external/pypi/pypi__pyspark/pyspark/data/mllib/images/origin/kittens/not-image.txt
runfiles/pypi/pypi__pyspark/pyspark/data/mllib/images/partitioned/cls=kittens/date=2018-01/not-image.txt=external/pypi/pypi__pyspark/pyspark/data/mllib/images/partitioned/cls=kittens/date=2018-01/not-image.txt
```
