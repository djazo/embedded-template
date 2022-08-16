project('Embeddedreality ARM sdk',
  ['c','cpp'],
  default_options: [
    'warning_level=3',
    'werror=false',
    'cpp_std=c++17', 'build.cpp_std=c++17',
    'debug=false',
    'optimization=s',
    'default_library=static',
    ],
  license: 'MIT',
  meson_version: '>=0.63.0')

fs = import('fs')

# pull in the freertos
freertos_config_proj = subproject('freertos_config')
freertos_config_dep = freertos_config_proj.get_variable('freertos_config_dep')
freertos_proj = subproject('freertos',
  default_options: [
    'freertos_port=@0@'.format(host_machine.cpu()),
    'freertos_heap=1'
    ]
  )
freertos_dep = freertos_proj.get_variable('freertos_dep')
# get the freertos-posix
freertos_posix_proj = subproject('freertos-posix')
freertos_posix_dep = freertos_posix_proj.get_variable('freertos_posix_dep')

# picolibc

picolibc_proj = subproject('picolibc',
  default_options: [
    'multilib=false',
    'picocrt=false'
    ]
  )
picolibc_dep = picolibc_proj.get_variable('picolibc_dep')

# llvm stuff

llvm_proj = subproject('llvm')
compiler_rt_dep = llvm_proj.get_variable('compiler_rt_dep')
libcxxabi_dep = llvm_proj.get_variable('libcxxabi_dep')
libcxx_dep = llvm_proj.get_variable('libcxx_dep')

