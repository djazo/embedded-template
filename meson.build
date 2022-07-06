project('Embeddedreality sdk',
  ['cpp'],
  default_options: [
    'warning_level=3',
    'werror=false',
    'cpp_std=c++17', 'build.cpp_std=c++17',
    'debug=false',
    'optimization=s',
    ],
  license: 'MIT',
  meson_version: '>=0.60.0')

# get dependencies to build proper c++

compiler-rt_proj = subproject('compiler-rt')
compiler-rt_dep = compiler-rt_proj.get_variable('compiler-rt_dep')

libunwind_proj = subproject('libunwind')
libunwind_dep = libunwind_proj.get_variable('libunwind_dep')

libcxxabi_proj = subproject('libcxxabi')
libcxxabi_dep = libcxxabi_proj.get_variable('libcxxabi_dep')

libcxx_proj = subproject('libcxx')
libcxx_dep = libcxx_proj.get_variable('libcxx_dep')

libcxx_headers_proj = subproject('libcxx-headers')
libcxx_headers_dep = libcxx_headers_proj.get_variable('libcxx_headers_dep')
libcxxabi_headers_proj = subproject('libcxxabi-headers')
libcxxabi_headers_dep = libcxxabi_headers_proj.get_variable('libcxxabi_headers_dep')
libcxxabi_proj = subproject('libcxxabi')
libcxxabi_dep = libcxxabi_proj.get_variable('libcxxabi_dep')
libcxx_proj = subproject('libcxx')
libcxx_dep = libcxx_proj.get_variable('libcxx_dep')
