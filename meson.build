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

# dict for chips

d_cmsis_hal = {
  'stm32f0': ['cmsis_device_f0','stm32f0_hal'],
  'stm32f1': ['cmsis_device_f1','stm32f1_hal'],
  'stm32f2': ['cmsis_device_f2','stm32f2_hal'],
  'stm32f3': ['cmsis_device_f3','stm32f3_hal'],
  'stm32f4': ['cmsis_device_f4','stm32f4_hal'],
  'stm32f7': ['cmsis_device_f7','stm32f7_hal'],
  'stm32g0': ['cmsis_device_g0','stm32g0_hal'],
  'stm32g4': ['cmsis_device_g4','stm32g4_hal'],
  'stm32h7': ['cmsis_device_h7','stm32h7_hal'],
  'stm32l0': ['cmsis_device_l0','stm32l0_hal'],
  'stm32l1': ['cmsis_device_l1','stm32l1_hal'],
  'stm32l4': ['cmsis_device_l4','stm32l4_hal'],
  'stm32l5': ['cmsis_device_l5','stm32l5_hal'],
  'stm32wb': ['cmsis_device_wb','stm32wb_hal'],
  'stm32wl': ['cmsis_device_wl','stm32wl_hal']
  }

chip = meson.get_external_property('chip')

if chip not in d_cmsis_hal
  asset(0, 'No board foud for @0@'.format(chip))
endif

# picolibc

picolibc_proj = subproject('picolibc',
  default_options: [
    'multilib=false',
    'picocrt=false'
    ]
  )
picolibc_dep = picolibc_proj.get_variable('picolibc_dep')

# CMSIS 

arm_cmsis_proj = subproject('arm-cmsis')
arm_cmsis_dep = arm_cmsis_proj.get_variable('arm_cmsis_dep')

# cmsis ST
stm_proj_names = d_cmsis_hal.get(chip)
stm_cmsis_dev_proj = subproject(stm_proj_names[0])
stm_cmsis_hal_proj = subproject(stm_proj_names[1])
stm_cmsis_dev_dep = stm_cmsis_dev_proj.get_variable('stm_cmsis_dev_dep')
stm_cmsis_hal_dep = stm_cmsis_hal_proj.get_variable('stm_cmsis_hal_dep')

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

# llvm stuff

llvm_proj = subproject('llvm')
compiler_rt_dep = llvm_proj.get_variable('compiler_rt_dep')
libcxxabi_dep = llvm_proj.get_variable('libcxxabi_dep')
libcxx_dep = llvm_proj.get_variable('libcxx_dep')

