platform(bare6502 COMPLETE HOSTED PARENT common)

if(NOT CMAKE_CROSSCOMPILING)
  return()
endif()

install(FILES
  chrin.h
TYPE INCLUDE)
install(FILES link.ld TYPE LIB)

add_platform_library(bare6502-crt0
  crt0/reset.S
  crt0/serial.S
)
merge_libraries(bare6502-crt0
  # common-crt0
  common-copy-data
  common-init-stack
  common-zero-bss
  common-exit-loop
)

add_platform_library(bare6502-c
  getchar.c
  putchar.c
)

target_compile_options(bare6502-crt0 PUBLIC -mcpu=mos6502)
target_link_libraries(bare6502-crt0 PRIVATE common-asminc)

target_include_directories(bare6502-c BEFORE PUBLIC .)
target_compile_options(bare6502-c PUBLIC -mcpu=mos6502)
target_link_libraries(bare6502-c PRIVATE common-asminc)