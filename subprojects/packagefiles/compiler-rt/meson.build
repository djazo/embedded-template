project('Embeddedreality LLVM compiler-rt',
  ['c','cpp'],
  default_options: [
    'c_std=c11','build.c_std=c11',
    'cpp_std=c++17','build.cpp_std=c++17',
    ],
  license: 'MIT',
  meson_version: '>=0.60.0')

assert(meson.is_cross_build(), 'This is now for cross compiling at the moment, sorry')

build_type = get_option('buildtype')

native_os = host_machine.system()
build_os = build_machine.system()

# I'm just pre-populating native_compiler here, no need for it
native_compiler = meson.get_compiler('cpp', native: true)
target_compiler = meson.get_compiler('cpp')

# build options

enable_werror = get_option('enable-werror')
enable_pedantic_error = get_option('enable-pedantic-error')
skip_atomic_builtins = get_option('exclude-atomic-builtins')

native_architecture = build_machine.cpu_family()
target_architecture = host_machine.cpu_family()

builtins_compiler_args = [
  '-fno-builtin',
  '-fvisibility=hidden',
  '-fomit-frame-pointer',
  '-ffreestanding',
  '-Wno-pedantic',
	'-Wno-undef',
	'-Wno-sign-conversion',
	'-Wno-double-promotion',
	'-Wno-float-equal',
	'-Wno-float-conversion',
	'-Wno-conversion',
	'-Wno-missing-noreturn',
	'-Wno-unused-parameter',
	'-Wno-format-nonliteral',
	'-Wno-unused-macros',
  ]

if target_compiler.has_argument('-Wno-builtin-declaration-mismatch')
	builtins_compiler_args += '-Wno-builtin-declaration-mismatch'
endif

if target_compiler.has_argument('-Wno-shift-sign-overflow')
	builtins_compiler_args += '-Wno-shift-sign-overflow'
endif

if target_compiler.has_argument('-Wno-shorten-64-to-32')
	builtins_compiler_args += '-Wno-shorten-64-to-32'
endif

if target_compiler.has_argument('-Wno-unreachable-code-break')
	builtins_compiler_args += '-Wno-unreachable-code-break'
endif

if target_compiler.has_argument('-Wno-conditional-uninitialized')
	builtins_compiler_args += '-Wno-conditional-uninitialized'
endif

if target_compiler.has_argument('-Wno-missing-variable-declarations')
	builtins_compiler_args += '-Wno-missing-variable-declarations'
endif

if target_compiler.has_argument('-Wno-reserved-id-macro')
	builtins_compiler_args += '-Wno-reserved-id-macro'
endif

if target_compiler.has_argument('-Wno-missing-prototypes') and target_compiler.get_id() != 'gcc'
	builtins_compiler_args += '-Wno-missing-prototypes'
endif

common_sources = files(
  'lib/builtins/absvdi2.c',
	'lib/builtins/absvsi2.c',
	'lib/builtins/absvti2.c',
	'lib/builtins/adddf3.c',
	'lib/builtins/addsf3.c',
	'lib/builtins/addtf3.c',
	'lib/builtins/addvdi3.c',
	'lib/builtins/addvsi3.c',
	'lib/builtins/addvti3.c',
	'lib/builtins/apple_versioning.c',
	'lib/builtins/ashldi3.c',
	'lib/builtins/ashlti3.c',
	'lib/builtins/ashrdi3.c',
	'lib/builtins/ashrti3.c',
	'lib/builtins/clear_cache.c',
	'lib/builtins/clzti2.c',
	'lib/builtins/cmpdi2.c',
	'lib/builtins/cmpti2.c',
	'lib/builtins/comparedf2.c',
	'lib/builtins/ctzdi2.c',
	'lib/builtins/ctzsi2.c',
	'lib/builtins/ctzti2.c',
	'lib/builtins/divdc3.c',
	'lib/builtins/divdf3.c',
	'lib/builtins/divdi3.c',
	'lib/builtins/divmoddi4.c',
	'lib/builtins/divsc3.c',
	'lib/builtins/divsf3.c',
	'lib/builtins/divtc3.c',
	'lib/builtins/divti3.c',
	'lib/builtins/divtf3.c',
	'lib/builtins/extendsfdf2.c',
	'lib/builtins/extendhfsf2.c',
	'lib/builtins/ffsdi2.c',
	'lib/builtins/ffssi2.c',
	'lib/builtins/ffsti2.c',
	'lib/builtins/fixdfdi.c',
	'lib/builtins/fixdfsi.c',
	'lib/builtins/fixdfti.c',
	'lib/builtins/fixsfdi.c',
	'lib/builtins/fixsfsi.c',
	'lib/builtins/fixsfti.c',
	'lib/builtins/fixunsdfdi.c',
	'lib/builtins/fixunsdfsi.c',
	'lib/builtins/fixunsdfti.c',
	'lib/builtins/fixunssfdi.c',
	'lib/builtins/fixunssfsi.c',
	'lib/builtins/fixunssfti.c',
	'lib/builtins/floatsidf.c',
	'lib/builtins/floatsisf.c',
	'lib/builtins/floattidf.c',
	'lib/builtins/floattisf.c',
	'lib/builtins/floatunsidf.c',
	'lib/builtins/floatunsisf.c',
	'lib/builtins/floatuntidf.c',
	'lib/builtins/floatuntisf.c',
	'lib/builtins/int_util.c',
	'lib/builtins/lshrdi3.c',
	'lib/builtins/lshrti3.c',
	'lib/builtins/moddi3.c',
	'lib/builtins/modti3.c',
	'lib/builtins/muldc3.c',
	'lib/builtins/muldf3.c',
	'lib/builtins/muldi3.c',
	'lib/builtins/mulodi4.c',
	'lib/builtins/mulosi4.c',
	'lib/builtins/muloti4.c',
	'lib/builtins/mulsc3.c',
	'lib/builtins/mulsf3.c',
	'lib/builtins/multi3.c',
	'lib/builtins/multf3.c',
	'lib/builtins/mulvdi3.c',
	'lib/builtins/mulvsi3.c',
	'lib/builtins/mulvti3.c',
	'lib/builtins/negdf2.c',
	'lib/builtins/negdi2.c',
	'lib/builtins/negsf2.c',
	'lib/builtins/negti2.c',
	'lib/builtins/negvdi2.c',
	'lib/builtins/negvsi2.c',
	'lib/builtins/negvti2.c',
	#'lib/builtins/os_version_check.c',
	'lib/builtins/paritydi2.c',
	'lib/builtins/paritysi2.c',
	'lib/builtins/parityti2.c',
	'lib/builtins/popcountdi2.c',
	'lib/builtins/popcountsi2.c',
	'lib/builtins/popcountti2.c',
	'lib/builtins/powidf2.c',
	'lib/builtins/powisf2.c',
	'lib/builtins/powitf2.c',
	'lib/builtins/subdf3.c',
	'lib/builtins/subsf3.c',
	'lib/builtins/subvdi3.c',
	'lib/builtins/subvsi3.c',
	'lib/builtins/subvti3.c',
	'lib/builtins/subtf3.c',
	'lib/builtins/trampoline_setup.c',
	'lib/builtins/truncdfhf2.c',
	'lib/builtins/truncdfsf2.c',
	'lib/builtins/truncsfhf2.c',
	'lib/builtins/ucmpdi2.c',
	'lib/builtins/ucmpti2.c',
	'lib/builtins/udivdi3.c',
	'lib/builtins/udivmoddi4.c',
	'lib/builtins/udivmodti4.c',
	'lib/builtins/udivti3.c',
	'lib/builtins/umoddi3.c',
	'lib/builtins/umodti3.c',
  )

common_nonarm_sources = files(
  'lib/builtins/bswapdi2.c',
	'lib/builtins/bswapsi2.c',
	'lib/builtins/clzdi2.c',
	'lib/builtins/clzsi2.c',
	'lib/builtins/comparesf2.c',
	'lib/builtins/divmodsi4.c',
	'lib/builtins/divsi3.c',
	'lib/builtins/modsi3.c',
	'lib/builtins/udivmodsi4.c',
	'lib/builtins/udivsi3.c',
	'lib/builtins/umodsi3.c',
  )

common_nonx86_64_sources = files(
	'lib/builtins/floatdidf.c',
	'lib/builtins/floatdisf.c',
	'lib/builtins/floatundidf.c',
	'lib/builtins/floatundisf.c',
  )

common_tf_sources = files(
	'lib/builtins/comparetf2.c',
	'lib/builtins/extenddftf2.c',
	'lib/builtins/extendsftf2.c',
	'lib/builtins/fixtfdi.c',
	'lib/builtins/fixtfsi.c',
	'lib/builtins/fixtfti.c',
	'lib/builtins/fixunstfdi.c',
	'lib/builtins/fixunstfsi.c',
	'lib/builtins/fixunstfti.c',
	'lib/builtins/floatditf.c',
	'lib/builtins/floatsitf.c',
	'lib/builtins/floattitf.c',
	'lib/builtins/floatunditf.c',
	'lib/builtins/floatunsitf.c',
	'lib/builtins/floatuntitf.c',
	'lib/builtins/multc3.c',
	'lib/builtins/trunctfdf2.c',
	'lib/builtins/trunctfsf2.c',
  )

nonbaremetal_sources = files(
	'lib/builtins/emutls.c',
	'lib/builtins/enable_execute_stack.c',
	'lib/builtins/eprintf.c',
  )

x86_sources = files(
	'lib/builtins/cpu_model.c',
	'lib/builtins/divxc3.c',
	'lib/builtins/fixxfdi.c',
	'lib/builtins/fixxfti.c',
	'lib/builtins/fixunsxfdi.c',
	'lib/builtins/fixunsxfsi.c',
	'lib/builtins/fixunsxfti.c',
	'lib/builtins/floatdixf.c',
	'lib/builtins/floattixf.c',
	'lib/builtins/floatundixf.c',
	'lib/builtins/floatuntixf.c',
	'lib/builtins/mulxc3.c',
	'lib/builtins/powixf2.c',
  )

x86_64_sources = files(
	'lib/builtins/x86_64/floatdidf.c',
	'lib/builtins/x86_64/floatdisf.c',
	'lib/builtins/x86_64/floatdixf.c',
	'lib/builtins/x86_64/floatundidf.S',
	'lib/builtins/x86_64/floatundisf.S',
	'lib/builtins/x86_64/floatundixf.S',
  )

arm_sources = files(
	'lib/builtins/arm/fp_mode.c',
	'lib/builtins/arm/bswapdi2.S',
	'lib/builtins/arm/bswapsi2.S',
	'lib/builtins/arm/clzdi2.S',
	'lib/builtins/arm/clzsi2.S',
	'lib/builtins/arm/comparesf2.S',
	'lib/builtins/arm/divmodsi4.S',
	'lib/builtins/arm/divsi3.S',
	'lib/builtins/arm/modsi3.S',
	'lib/builtins/arm/sync_fetch_and_add_4.S',
	'lib/builtins/arm/sync_fetch_and_add_8.S',
	'lib/builtins/arm/sync_fetch_and_and_4.S',
	'lib/builtins/arm/sync_fetch_and_and_8.S',
	'lib/builtins/arm/sync_fetch_and_max_4.S',
	'lib/builtins/arm/sync_fetch_and_max_8.S',
	'lib/builtins/arm/sync_fetch_and_min_4.S',
	'lib/builtins/arm/sync_fetch_and_min_8.S',
	'lib/builtins/arm/sync_fetch_and_nand_4.S',
	'lib/builtins/arm/sync_fetch_and_nand_8.S',
	'lib/builtins/arm/sync_fetch_and_or_4.S',
	'lib/builtins/arm/sync_fetch_and_or_8.S',
	'lib/builtins/arm/sync_fetch_and_sub_4.S',
	'lib/builtins/arm/sync_fetch_and_sub_8.S',
	'lib/builtins/arm/sync_fetch_and_umax_4.S',
	'lib/builtins/arm/sync_fetch_and_umax_8.S',
	'lib/builtins/arm/sync_fetch_and_umin_4.S',
	'lib/builtins/arm/sync_fetch_and_umin_8.S',
	'lib/builtins/arm/sync_fetch_and_xor_4.S',
	'lib/builtins/arm/sync_fetch_and_xor_8.S',
	'lib/builtins/arm/udivmodsi4.S',
	'lib/builtins/arm/udivsi3.S',
	'lib/builtins/arm/umodsi3.S',
  )

arm_thumb1_sources = files(
	'lib/builtins/arm/addsf3.S',
	'lib/builtins/arm/divsi3.S',
	'lib/builtins/arm/udivsi3.S',
	'lib/builtins/arm/comparesf2.S',
	# Thumb1_JT
	'lib/builtins/arm/switch16.S',
	'lib/builtins/arm/switch32.S',
	'lib/builtins/arm/switch8.S',
	'lib/builtins/arm/switchu8.S',
	# Thumb1 ICache Sources
	'lib/builtins/arm/sync_synchronize.S',
  )

arm_vfp2_common_sources = files(
	# SjLj EH Sources
	'lib/builtins/arm/restore_vfp_d8_d15_regs.S',
	'lib/builtins/arm/save_vfp_d8_d15_regs.S',
	# Thumb1_VFPv2_sources
	'lib/builtins/arm/addsf3vfp.S',
	'lib/builtins/arm/divsf3vfp.S',
	'lib/builtins/arm/eqdf2vfp.S',
	'lib/builtins/arm/eqsf2vfp.S',
	'lib/builtins/arm/fixsfsivfp.S',
	'lib/builtins/arm/fixunssfsivfp.S',
	'lib/builtins/arm/floatsisfvfp.S',
	'lib/builtins/arm/floatunssisfvfp.S',
	'lib/builtins/arm/gedf2vfp.S',
	'lib/builtins/arm/gesf2vfp.S',
	'lib/builtins/arm/gtdf2vfp.S',
	'lib/builtins/arm/gtsf2vfp.S',
	'lib/builtins/arm/ledf2vfp.S',
	'lib/builtins/arm/lesf2vfp.S',
	'lib/builtins/arm/ltdf2vfp.S',
	'lib/builtins/arm/ltsf2vfp.S',
	'lib/builtins/arm/mulsf3vfp.S',
	'lib/builtins/arm/nedf2vfp.S',
	'lib/builtins/arm/negdf2vfp.S',
	'lib/builtins/arm/negsf2vfp.S',
	'lib/builtins/arm/nesf2vfp.S',
	'lib/builtins/arm/subsf3vfp.S',
	'lib/builtins/arm/unorddf2vfp.S',
	'lib/builtins/arm/unordsf2vfp.S',
  )

arm_vfp_double_precision_sources = files(
	'lib/builtins/arm/adddf3vfp.S',
	'lib/builtins/arm/divdf3vfp.S',
	'lib/builtins/arm/extendsfdf2vfp.S',
	'lib/builtins/arm/fixdfsivfp.S',
	'lib/builtins/arm/fixunsdfsivfp.S',
	'lib/builtins/arm/floatunssidfvfp.S',
	'lib/builtins/arm/floatsidfvfp.S',
	'lib/builtins/arm/muldf3vfp.S',
	'lib/builtins/arm/subdf3vfp.S',
	'lib/builtins/arm/truncdfsf2vfp.S',
  )

arm_eabi_sources = files(
	'lib/builtins/arm/aeabi_cdcmp.S',
	'lib/builtins/arm/aeabi_cdcmpeq_check_nan.c',
	'lib/builtins/arm/aeabi_cfcmp.S',
	'lib/builtins/arm/aeabi_cfcmpeq_check_nan.c',
	'lib/builtins/arm/aeabi_dcmp.S',
	'lib/builtins/arm/aeabi_div0.c',
	'lib/builtins/arm/aeabi_drsub.c',
	'lib/builtins/arm/aeabi_fcmp.S',
	'lib/builtins/arm/aeabi_frsub.c',
	'lib/builtins/arm/aeabi_idivmod.S',
	'lib/builtins/arm/aeabi_ldivmod.S',
	'lib/builtins/arm/aeabi_memcmp.S',
	'lib/builtins/arm/aeabi_memcpy.S',
	'lib/builtins/arm/aeabi_memmove.S',
	'lib/builtins/arm/aeabi_memset.S',
	'lib/builtins/arm/aeabi_uidivmod.S',
	'lib/builtins/arm/aeabi_uldivmod.S',
  )

arm_file_group = [
	arm_sources,
	arm_eabi_sources,
	arm_thumb1_sources,
  ]

thumb1_file_group = [
	arm_eabi_sources,
	arm_thumb1_sources,
  ]

atomic_sources = files(
	'lib/builtins/atomic.c'
  )

compiler_rt_sources = [
  common_sources,
  ]

if not skip_atomic_builtins
  compiler_rt_sources += atomic_sources
endif

# architecture selection of files

if target_architecture == 'arm'
  compiler_rt_sources += [
    arm_file_group,
    generic_nonx86_64_sources,
    ]

  arm_fp_setting = target_compiler.get_define('__ARM_FP')
  arm_version = target_compiler.get_define('__ARM_ARCH')

  if arm_fp_setting == '4' or arm_fp_setting == '12' or arm_fp_setting == '14'
    if(arm_version.to_int() < 7)
      message('Enabling single-precision ARM VFP Builtins')
      compiler_rt_sources += arm_vfp2_common_sources
    endif
  endif

  if arm_fp_setting == '8' or arm_fp_setting == '12' or arm_fp_setting == '14'
    message('Enabling double-precision ARM VFP Builtins')
    compiler_rt_sources += arm_vfp_double_precision_sources
  endif

elif target_architecture == 'aarch64' or target_architecture == 'arm64'
  compiler_rt_sources += [
    common_tf_sources,
    common_nonx86_64_sources,
    common_nonarm_sources
    ]

elif target_architecture == 'x86'
  compiler_rt_sources += [
    x86_sources,
    common_nonx86_64_sources,
    common_nonarm_sources
    ]

elif target_architecture == 'x86_64'
  compiler_rt_sources += [
    x86_sources,
    x86_64_sources,
    common_nonarm_sources
    ]
else
  assert(0, "Unsupported architecture: '+ target_architecture')
endif

compiler_rt_lib = static_library('compiler_rt',
  compiler_rt_sources,
  c_args: builtins_compiler_args,
  cpp_args: builtins_compiler_args,
  pic: true
  )

compiler-rt_dep = declare_dependency(
  link_with: compiler_rt_builtins_lib
  )
