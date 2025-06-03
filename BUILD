load("@gazelle//:def.bzl", "gazelle")
load("@io_bazel_rules_go//go:def.bzl", "go_binary", "go_library")
load("@rules_pkg//pkg:deb.bzl", "pkg_deb")
load("@rules_pkg//pkg:tar.bzl", "pkg_tar")

gazelle(name = "gazelle")

go_library(
    name = "go_default_library",
    srcs = ["main.go"],
    importpath = "github.com/werkt/bf-client",
    visibility = ["//visibility:private"],
    deps = [
        "//client:go_default_library",
        "//view:go_default_library",
        "@com_github_gizak_termui_v3//:go_default_library",
        "@com_github_gizak_termui_v3//widgets:go_default_library",
        "@com_github_nsf_termbox_go//:go_default_library",
    ],
)

go_binary(
    name = "client",
    embed = [":go_default_library"],
    visibility = ["//visibility:public"],
)

go_binary(
    name = "test",
    srcs = ["test.go"],
    deps = [
        "@com_github_golang_protobuf//proto:go_default_library",
        "@remoteapis//build/bazel/remote/execution/v2:go_default_library",
    ],
)

go_binary(
    name = "client_x86",
    embed = [":go_default_library"],
    goarch = "amd64",
    goos = "linux",
    visibility = ["//visibility:public"],
    out = "bf-client",  # Renames the output binary
)

go_binary(
    name = "client_arm",
    embed = [":go_default_library"],
    goarch = "arm64",
    goos = "linux",
    visibility = ["//visibility:public"],
    out = "bf-client",  # Renames the output binary
)

pkg_tar(
    name = "tar_x86",
    srcs = [":client_x86"],
    out = "bf-client-x86.tar",
    mode = "0755",
    package_dir = "/usr/bin",
)

pkg_tar(
    name = "tar_arm",
    srcs = [":client_arm"],
    out = "bf-client-arm.tar",
    mode = "0755",
    package_dir = "/usr/bin",
)

pkg_deb(
    name = "deb_x86",
    out = "bf-client-x86.deb",
    architecture = "amd64",
    data = ":tar_x86",
    description = "A simple TUI for monitoring and exploring a buildfarm cluster.",
    maintainer = "TODO",
    package = "bf-client",
    version = "0.1.0",
)

pkg_deb(
    name = "deb_arm",
    out = "bf-client-arm.deb",
    architecture = "arm64",
    data = ":tar_arm",
    description = "A simple TUI for monitoring and exploring a buildfarm cluster.",
    maintainer = "TODO",
    package = "bf-client",
    version = "0.1.0",
)
