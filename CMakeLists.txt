cmake_minimum_required (VERSION 3.7)
PROJECT(iothub_device_sample_project C)

# Set Azure IoT SDK C settings
set(use_mqtt ON CACHE  BOOL "Set mqtt on" FORCE )
set(skip_samples ON CACHE  BOOL "Set slip_samples on" FORCE )
set(BUILD_TESTING OFF CACHE  BOOL "Set BUILD_TESTING off" FORCE )

# Add Azure IoT SDK C
add_subdirectory(azure-iot-sdk-c out)

compileAsC99()

set(iothub_project_files
    iothub_device_sample.c
)


IF(WIN32)
    #windows needs this define
    add_definitions(-D_CRT_SECURE_NO_WARNINGS)
ENDIF(WIN32)

#Conditionally use the SDK trusted certs in the samples
if(${use_sample_trusted_cert})
    add_definitions(-DSET_TRUSTED_CERT_IN_SAMPLES)
    include_directories(${PROJECT_SOURCE_DIR}/certs)
    set(iothub_c_files ${iothub_c_files} ${PROJECT_SOURCE_DIR}/certs/certs.c)
endif()

include_directories(.)
include_directories(${PROJECT_SOURCE_DIR}/azure-iot-sdk-c/deps/umock-c/inc)
include_directories(${PROJECT_SOURCE_DIR}/azure-iot-sdk-c/deps/azure-macro-utils-c/inc)

add_executable(iothub_device_sample ${iothub_project_files})

target_link_libraries(iothub_device_sample iothub_client)

