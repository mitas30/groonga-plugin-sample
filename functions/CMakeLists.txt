# Groonga プラグインを共有モジュール (.so) としてビルド
add_library(sample MODULE sample.c)

set_target_properties(sample PROPERTIES
  PREFIX ""          # "lib" プレフィックスを付けない
  OUTPUT_NAME "sample"
  NO_SONAME ON       # -avoid-version 相当: sample.so.0 等を生成しない
)

target_include_directories(sample PRIVATE ${GROONGA_INCLUDE_DIRS})
target_compile_options(sample PRIVATE ${GROONGA_CFLAGS_OTHER})
target_link_libraries(sample PRIVATE ${GROONGA_LIBRARIES})
target_link_directories(sample PRIVATE ${GROONGA_LIBRARY_DIRS})

# -no-undefined 相当
if(CMAKE_SYSTEM_NAME STREQUAL "Linux")
  target_link_options(sample PRIVATE "LINKER:--no-undefined")
endif()

# install: <groonga_plugins_dir>/functions/sample.so
install(TARGETS sample
  LIBRARY DESTINATION "${GROONGA_PLUGINS_DIR}/functions"
)
