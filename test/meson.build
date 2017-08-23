vvp = find_program('vvp')
iverilog = find_program('iverilog')

objcopy = find_program('/home/jpakkane/HDMI2USB-litex-firmware/build/conda/bin/lm32-elf-objcopy')

link_script = files('linker.ld')

hello_world = executable('hello_world', 'crt.S', 'hello_world.c',
  link_args : ['-T' + '@0@/@1@'.format(meson.source_root(), link_script[0])],
  link_depends : link_script,
  name_suffix : 'elf',
)

pipe1 = executable('pipe1', 'pipe1.S',
  link_args : ['-T' + '@0@/@1@'.format(meson.source_root(), link_script[0])],
  link_depends : link_script,
  name_suffix : 'elf',
)

v_sources = files(
  'tb_lm32_system.v',
  'lm32_config.v',
  '../rtl/lm32_adder.v',
  '../rtl/lm32_addsub.v',
  '../rtl/lm32_cpu.v',
  '../rtl/lm32_dcache.v',
  '../rtl/lm32_debug.v',
  '../rtl/lm32_decoder.v',
  '../rtl/lm32_dp_ram.v',
  '../rtl/lm32_icache.v',
  '../rtl/lm32_instruction_unit.v',
  '../rtl/lm32_interrupt.v',
  '../rtl/lm32_jtag.v',
  '../rtl/lm32_load_store_unit.v',
  '../rtl/lm32_logic_op.v',
  '../rtl/lm32_mc_arithmetic.v',
  '../rtl/lm32_multiplier.v',
  '../rtl/lm32_ram.v',
  '../rtl/lm32_shifter.v',
  '../rtl/lm32_itlb.v',
  '../rtl/lm32_dtlb.v',
  '../rtl/lm32_top.v',
)

prog_src = custom_target('hello_world_v',
  output : 'hello_world.vh',
  input : hello_world,
  command : [objcopy, '-O', 'verilog', '@INPUT@', '@OUTPUT@'],
)

#v_sources += prog_src

tb_lm32_system = custom_target('tb_lm32_system',
  output : 'tb_lm32_system',
  input : v_sources,
  command : [iverilog,
    '-I' + meson.current_source_dir(),
    '-I' + join_paths(meson.current_source_dir(), '../rtl'),
    '-o', '@OUTPUT@', '@INPUT@'],
  build_by_default : true,
  )

run_target('sim_hello_world',
  command : [vvp, tb_lm32_system, '+prog=' + prog_src.full_path()],
  depends : prog_src)
