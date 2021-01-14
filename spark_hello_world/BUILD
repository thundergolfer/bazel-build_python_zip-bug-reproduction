load("@rules_python//python:defs.bzl", "py_binary", "py_library", "py_test")
load("@pypi//:requirements.bzl", "requirement")

py_binary(
    name = "main",
    srcs = glob(["*.py"]),
    deps = [
        "@rules_python//python/runfiles",
        requirement("pyspark"),
    ],
    python_version = "PY3",
)

filegroup(
  name = "main_zip",
  srcs = [":main"],
  output_group = "python_zip_file",
  tags = [
      "spark-artifact"
  ]
)

