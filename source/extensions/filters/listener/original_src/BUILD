licenses(["notice"])  # Apache 2

# A filter for mirroring the downstream remote address on the upstream's source.

load(
    "//bazel:envoy_build_system.bzl",
    "envoy_cc_extension",
    "envoy_cc_library",
    "envoy_package",
)

envoy_package()

envoy_cc_library(
    name = "config_lib",
    srcs = ["config.cc"],
    hdrs = ["config.h"],
    deps = ["@envoy_api//envoy/extensions/filters/listener/original_src/v3:pkg_cc_proto"],
)

envoy_cc_library(
    name = "original_src_lib",
    srcs = ["original_src.cc"],
    hdrs = ["original_src.h"],
    deps = [
        ":config_lib",
        "//include/envoy/buffer:buffer_interface",
        "//include/envoy/network:address_interface",
        "//include/envoy/network:connection_interface",
        "//include/envoy/network:filter_interface",
        "//source/common/common:assert_lib",
        "//source/common/common:minimal_logger_lib",
        "//source/extensions/filters/common/original_src:socket_option_factory_lib",
    ],
)

envoy_cc_extension(
    name = "config",  # The extension build system requires a library named config
    srcs = ["original_src_config_factory.cc"],
    hdrs = ["original_src_config_factory.h"],
    security_posture = "robust_to_untrusted_downstream",
    status = "alpha",
    deps = [
        ":config_lib",
        ":original_src_lib",
        "//include/envoy/registry",
        "//include/envoy/server:filter_config_interface",
        "//source/extensions/filters/listener:well_known_names",
        "@envoy_api//envoy/extensions/filters/listener/original_src/v3:pkg_cc_proto",
    ],
)
