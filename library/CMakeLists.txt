cmake_minimum_required(VERSION 3.1)
project(lib)
set(CMAKE_INCLUDE_CURRENT_DIR_IN_INTERFACE ON)
set(CMAKE_INSTALL_PREFIX ${CMAKE_SOURCE_DIR}/../install)

add_library(test_lib SHARED src/test.cpp)
target_include_directories(test_lib PRIVATE src)
set_target_properties(test_lib PROPERTIES
    PUBLIC_HEADER src/test.hpp
)
include(FetchContent)
set(FETCHCONTENT_BASE_DIR ${PROJECT_SOURCE_DIR}/vendor)
FetchContent_Declare(
    googletest
    GIT_REPOSITORY https://github.com/google/googletest.git
    GIT_TAG release-1.12.1
)
FetchContent_MakeAvailable(googletest)

target_link_libraries(test_lib PUBLIC GTest::gtest_main)

install(TARGETS test_lib
    EXPORT foo-export # 5.a EXPORT オプションで エクスポート名を指定する
    LIBRARY DESTINATION lib # 5.b ライブラリのインストール先
    INCLUDES DESTINATION include # 5.c エクスポート時にincludeパスに含めるパスを指定する
    PUBLIC_HEADER DESTINATION include/) # ヘッダのインストール先

# 6. foo-config.cmake を自動生成してインストールする。
install(EXPORT foo-export # 6.a
    FILE foo-config.cmake # 6.b ファイル名を指定する
    DESTINATION cmake/ # 6.c インストール先
    EXPORT_LINK_INTERFACE_LIBRARIES) # 6.d 同時にリンクすべきライブラリをエクスポートする
