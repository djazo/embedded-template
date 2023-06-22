project('Embeddedreality ST32 sdk',
  ['c','cpp'],
  default_options: [
    'b_lto=true',
    'buildtype=custom',
    'c_std=c17',
    'cpp_eh=none',
    'cpp_rtti=false',
    'cpp_std=c++17',
    'debug=false',
    'default_library=static',
    'optimization=s',
    'prefer_static=true',
    'strip=true',
    'warning_level=3',
    'werror=false',
    ],
  license: 'MIT',
  meson_version: '>=1.0.0')

cmake = import('cmake')

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
  asset(0, 'No board found for @0@'.format(chip))
endif

# ARM CMSIS

arm_cmsis_proj = subproject('arm-cmsis')
arm_cmsis_dep = arm_cmsis_proj.get_variable('arm_cmsis_dep')

# STM32 hal

stm_proj_names = d_cmsis_hal.get(chip)
stm_cmsis_dev_proj = subproject(stm_proj_names[0])
stm_cmsis_hal_proj = subproject(stm_proj_names[1])
stm_cmsis_dev_dep = stm_cmsis_dev_proj.get_variable('stm_cmsis_dev_dep')
stm_cmsis_hal_dep = stm_cmsis_hal_proj.get_variable('stm_cmsis_hal_dep')
