workspace(name = "reproduce_zipapp_bug")

load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

rules_python_version = "0.1.0"

http_archive(
    name = "rules_python",
    sha256 = "b6d46438523a3ec0f3cead544190ee13223a52f6a6765a29eae7b7cc24cc83a0",
    url = "https://github.com/bazelbuild/rules_python/releases/download/{version}/rules_python-{version}.tar.gz".format(version = rules_python_version),
)

load("@rules_python//python:repositories.bzl", "py_repositories")

py_repositories()

load("@rules_python//python:pip.bzl", "pip_install")

pip_install(
    name = "pypi",
    timeout = 900,
    requirements = "//:requirements.txt",
)

