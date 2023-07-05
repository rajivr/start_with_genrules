workspace(name = "start_with_genrules")

load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

_RULES_NIXPKGS_COMMIT = "420370f64f03ed9c1ff9b5e2994d06c0439cb1f2"

http_archive(
    name = "io_tweag_rules_nixpkgs",
    strip_prefix = "rules_nixpkgs-%s" % _RULES_NIXPKGS_COMMIT,
    urls = ["https://github.com/tweag/rules_nixpkgs/archive/%s.tar.gz" % _RULES_NIXPKGS_COMMIT],
)

load("@io_tweag_rules_nixpkgs//nixpkgs:repositories.bzl", "rules_nixpkgs_dependencies")

rules_nixpkgs_dependencies()

load("@io_tweag_rules_nixpkgs//nixpkgs:nixpkgs.bzl", "nixpkgs_http_repository")

# URL can be obtained from https://status.nixos.org/
_NIXPKGS_COMMIT = "d3bb401dcfc5a46ce51cdfb5762e70cc75d082d2"

# This is the `sha256sum <NIXPKGS_COMMIT>.tar.gz` and *not* the hash
# obtained using `nix-prefetch-url --unpack <url>`
_NIXPKGS_COMMIT_SHA256 = "039c3841c0355dd779e48c3060ec83d389011e02673418a95c5c50921cf5cb14"

nixpkgs_http_repository(
    name = "nixpkgs",
    sha256 = _NIXPKGS_COMMIT_SHA256,
    strip_prefix = "nixpkgs-%s" % _NIXPKGS_COMMIT,
    url = "https://github.com/NixOS/nixpkgs/archive/%s.tar.gz" % _NIXPKGS_COMMIT,
)

load("@io_tweag_rules_nixpkgs//nixpkgs:nixpkgs.bzl", "nixpkgs_cc_configure")

nixpkgs_cc_configure(
    name = "nixpkgs_config_cc",
    repository = "@nixpkgs",
)

load("@io_tweag_rules_nixpkgs//nixpkgs:nixpkgs.bzl", "nixpkgs_package")

nixpkgs_package(
    name = "zlib",
    repository = "@nixpkgs",
)

nixpkgs_package(
    name = "zlib.dev",
    build_file_content = """\
load("@rules_cc//cc:defs.bzl", "cc_library")
filegroup(
    name = "include",
    srcs = glob(["include/**/*.h"]),
    visibility = ["//visibility:public"],
)
cc_library(
    name = "zlib",
    srcs = ["@zlib//:lib"],
    hdrs = [":include"],
    strip_include_prefix = "include",
    visibility = ["//visibility:public"],
)
""",
    repository = "@nixpkgs",
)

http_archive(
    name = "rules_cc",
    sha256 = "3d9e271e2876ba42e114c9b9bc51454e379cbf0ec9ef9d40e2ae4cec61a31b40",
    strip_prefix = "rules_cc-0.0.6",
    urls = ["https://github.com/bazelbuild/rules_cc/releases/download/0.0.6/rules_cc-0.0.6.tar.gz"],
)

load("@rules_cc//cc:repositories.bzl", "rules_cc_dependencies", "rules_cc_toolchains")

rules_cc_dependencies()

rules_cc_toolchains()

http_archive(
    name = "libpng",
    build_file = "@//:libpng.BUILD",
    sha256 = "daeb2620d829575513e35fecc83f0d3791a620b9b93d800b763542ece9390fb4",
    strip_prefix = "libpng-1.6.37",
    urls = ["https://download.sourceforge.net/libpng/libpng-1.6.37.tar.gz"],
)

