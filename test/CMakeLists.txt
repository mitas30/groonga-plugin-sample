# run-test.sh を CTest に登録する
# - NO_MAKE=yes: スクリプト内の make 呼び出しをスキップ
# - GROONGA: groonga バイナリを直接指定 (make plugin-groonga を回避)
# - BUILD_DIR: CMake のバイナリディレクトリ内の test/ を指定
#   → top_dir = build/  → GRN_PLUGINS_DIR = build/
#   → build/functions/sample.so が Groonga に読み込まれる
add_test(
  NAME groonga_plugin_sample
  COMMAND ${CMAKE_CURRENT_SOURCE_DIR}/run-test.sh
  WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
)
set_tests_properties(groonga_plugin_sample PROPERTIES
  ENVIRONMENT
    "NO_MAKE=yes;GROONGA=${GROONGA_BINARY};BUILD_DIR=${CMAKE_CURRENT_BINARY_DIR}"
)
