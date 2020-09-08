load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

#
# Load Bazel's support for emitting Docker images.
#

http_archive(
    name = "io_bazel_rules_docker",
    sha256 = "4521794f0fba2e20f3bf15846ab5e01d5332e587e9ce81629c7f96c793bb7036",
    strip_prefix = "rules_docker-0.14.4",
    urls = ["https://github.com/bazelbuild/rules_docker/releases/download/v0.14.4/rules_docker-v0.14.4.tar.gz"],
)

load(
    "@io_bazel_rules_docker//repositories:repositories.bzl",
    container_repositories = "repositories",
)
container_repositories()

load("@io_bazel_rules_docker//repositories:deps.bzl", container_deps = "deps")

container_deps()

load("@io_bazel_rules_docker//repositories:pip_repositories.bzl", "pip_deps")

pip_deps()

load(
    "@io_bazel_rules_docker//java:image.bzl",
    _java_image_repos = "repositories",
)

_java_image_repos()

load(
    "@io_bazel_rules_docker//container:container.bzl",
    "container_pull",
)

#
# Load base Docker container.
#

container_pull(
  name = "openjdk11_slim",
  registry = "index.docker.io",
  repository = "library/openjdk",
  tag = "11-slim"
)

#
# Use external libraries through Bazel's Maven support.
#

RULES_JVM_EXTERNAL_TAG = "3.3"
RULES_JVM_EXTERNAL_SHA = "d85951a92c0908c80bd8551002d66cb23c3434409c814179c0ff026b53544dab"

http_archive(
    name = "rules_jvm_external",
    strip_prefix = "rules_jvm_external-%s" % RULES_JVM_EXTERNAL_TAG,
    sha256 = RULES_JVM_EXTERNAL_SHA,
    url = "https://github.com/bazelbuild/rules_jvm_external/archive/%s.zip" % RULES_JVM_EXTERNAL_TAG,
)

load("@rules_jvm_external//:defs.bzl", "maven_install")

#
# Load external dependencies.
#

maven_install(
    artifacts = [
        # Accessed as dependency: @maven://io_vertx_vertx_core
        "io.vertx:vertx-core:3.9.2",
        # Accessed as dependency: @maven://io_vertx_vertx_web
        "io.vertx:vertx-web:3.9.2",
        # Accessed as dependency: @maven://io_vertx_vertx_rx_java2
        "io.vertx:vertx-rx-java2:3.9.2",
    ],
    repositories = [
        "https://repo1.maven.org/maven2",
    ]
)
