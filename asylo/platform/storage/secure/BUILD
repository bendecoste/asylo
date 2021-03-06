#
# Copyright 2018 Asylo authors
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#

licenses(["notice"])  # Apache v2.0

# Secure IO library for enclave.

load("//asylo/bazel:asylo.bzl", "cc_enclave_test")
load("//asylo/bazel:copts.bzl", "ASYLO_DEFAULT_COPTS")

package(
    default_visibility = ["//asylo:implementation"],
)

cc_library(
    name = "trusted_secure",
    copts = ASYLO_DEFAULT_COPTS,
    deps = select({
        "@com_google_asylo//asylo": [
            "aead_handler",
            "enclave_storage_secure",
        ],
        "//conditions:default": [],
    }),
)

cc_library(
    name = "authenticated_dictionary",
    srcs = [
        "ctmmt_authenticated_dictionary.cc",
    ],
    hdrs = [
        "authenticated_dictionary.h",
        "ctmmt_authenticated_dictionary.h",
    ],
    copts = ASYLO_DEFAULT_COPTS,
    deps = [
        "@com_google_absl//absl/memory",
        "@com_google_certificate_transparency//:merkletree",
    ],
)

cc_library(
    name = "aead_handler",
    srcs = ["aead_handler.cc"],
    hdrs = ["aead_handler.h"],
    copts = ASYLO_DEFAULT_COPTS,
    deps = [
        ":authenticated_dictionary",
        "//asylo/crypto/util:byte_container_view",
        "//asylo/crypto/util:bytes",
        "//asylo/platform/arch:trusted_arch",
        "//asylo/platform/crypto/gcmlib:gcm_cryptor",
        "//asylo/platform/storage/utils:fd_closer",
        "//asylo/platform/storage/utils:offset_translator",
        "@com_google_absl//absl/base:core_headers",
        "@com_google_absl//absl/container:flat_hash_map",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/synchronization",
    ],
)

cc_library(
    name = "enclave_storage_secure",
    srcs = ["enclave_storage_secure.cc"],
    hdrs = ["enclave_storage_secure.h"],
    copts = ASYLO_DEFAULT_COPTS,
    deps = [
        ":aead_handler",
        "//asylo/platform/arch:trusted_arch",
        "//asylo/platform/storage/utils:offset_translator",
    ],
)

# Secure IO Library test in enclave.
cc_enclave_test(
    name = "enclave_storage_secure_test",
    srcs = ["enclave_storage_secure_test.cc"],
    copts = ASYLO_DEFAULT_COPTS,
    deps = [
        "//asylo/test/util:status_matchers",
        "//asylo/test/util:test_flags",
        "//asylo/util:cleansing_types",
        "@com_google_googletest//:gtest",
    ],
)
