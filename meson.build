project('Embeddedreality sdk',
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
  meson_version: '>=0.60.0')

# picolibc

picolibc_proj = subproject('picolibc',
  default_options: [
    'multilib=false',
    ]
  )
picolibc_dep = picolibc_proj.get_variable('picolibc_dep')

# freertos

freertos_proj = subproject('freertos')
freertos_dep = freertos_proj.get_variable('freertos_dep')

# llvm stuff

llvm_proj = subproject('llvm')
compiler_rt_dep = llvm_proj.get_variable('compiler_rt_dep')
libcxxabi_dep = llvm_proj.get_variable('libcxxabi_dep')
libcxx_dep = llvm_proj.get_variable('libcxx_dep')

