project('libgusb', 'c',
  version : '0.3.1',
  license : 'LGPL-2.1+',
  meson_version : '>=0.37.0',
  default_options : ['c_std=c99']
)

gusb_version = meson.project_version()
varr = gusb_version.split('.')
gusb_major_version = varr[0]
gusb_minor_version = varr[1]
gusb_micro_version = varr[2]

conf = configuration_data()
conf.set_quoted('VERSION', gusb_version)

# libtool versioning - this applies to libgusb
lt_current = '2'
lt_revision = '10'
lt_age = '0'
lt_version = '@0@.@1@.@2@'.format(lt_current, lt_age, lt_revision)

# get supported warning flags
test_args = [
  '-fstack-protector-strong',
  '-Waggregate-return',
  '-Wunused',
  '-Warray-bounds',
  '-Wcast-align',
  '-Wclobbered',
  '-Wdeclaration-after-statement',
  '-Wempty-body',
  '-Wextra',
  '-Wformat=2',
  '-Wformat-nonliteral',
  '-Wformat-security',
  '-Wformat-signedness',
  '-Wignored-qualifiers',
  '-Wimplicit-function-declaration',
  '-Winit-self',
  '-Wmissing-declarations',
  '-Wmissing-format-attribute',
  '-Wmissing-include-dirs',
  '-Wmissing-noreturn',
  '-Wmissing-parameter-type',
  '-Wmissing-prototypes',
  '-Wnested-externs',
  '-Wno-discarded-qualifiers',
  '-Wno-missing-field-initializers',
  '-Wno-strict-aliasing',
  '-Wno-suggest-attribute=format',
  '-Wno-unused-parameter',
  '-Wold-style-definition',
  '-Woverride-init',
  '-Wpointer-arith',
  '-Wreturn-type',
  '-Wshadow',
  '-Wsign-compare',
  '-Wstrict-aliasing',
  '-Wstrict-prototypes',
  '-Wswitch-default',
  '-Wtype-limits',
  '-Wundef',
  '-Wuninitialized',
  '-Wunused-but-set-variable',
  '-Wwrite-strings'
]
cc = meson.get_compiler('c')
foreach arg: test_args
  if cc.has_argument(arg)
    add_project_arguments(arg, language : 'c')
  endif
endforeach

# enable full RELRO where possible
# FIXME: until https://github.com/mesonbuild/meson/issues/1140 is fixed
global_link_args = []
test_link_args = [
  '-Wl,-z,relro',
  '-Wl,-z,now',
]
foreach arg: test_link_args
  if cc.has_argument(arg)
    global_link_args += arg
  endif
endforeach
add_global_link_arguments(
  global_link_args,
  language: 'c'
)

libgio = dependency('gio-2.0', version : '>= 2.44.0')
libusb = dependency('libusb-1.0', version : '>= 1.0.19')

gnome = import('gnome')

add_project_arguments('-DGUSB_COMPILATION', language: 'c')

prefix = get_option('prefix')
bindir = join_paths(prefix, get_option('bindir'))

configure_file(
  output : 'config.h',
  configuration : conf
)

root_incdir = include_directories('.')

subdir('gusb')
subdir('tools')
subdir('docs')
