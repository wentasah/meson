project('mingw dll versioning', 'c')

cc = meson.get_compiler('c')

if cc.get_id() == 'msvc'
  error('MESON_SKIP_TEST: test is only for MinGW')
endif

# Test that MinGW/GCC creates correctly-named dll files and dll.a files,
# and also installs them in the right place
some = shared_library('some', 'lib.c',
  version : '1.2.3',
  soversion : '0',
  install : true)

noversion = shared_library('noversion', 'lib.c',
  install : true)

onlyversion = shared_library('onlyversion', 'lib.c',
  version : '1.4.5',
  install : true)

onlysoversion = shared_library('onlysoversion', 'lib.c',
  # Also test that int soversion is acceptable
  soversion : 5,
  install : true)

# Hack to make the executables below depend on the shared libraries above
# without actually adding them as `link_with` dependencies since we want to try
# linking to them with -lfoo linker arguments.
out = custom_target('library-dependency-hack',
  input : 'exe.orig.c',
  output : 'exe.c',
  depends : [some, noversion, onlyversion, onlysoversion],
  command : ['cp', '@INPUT@', '@OUTPUT@'])

# Manually test if the linker can find the above libraries
# i.e., whether they were generated with the right naming scheme
test('manually linked 1', executable('manuallink1', out,
  link_args : ['-L.', '-lsome']))

test('manually linked 2', executable('manuallink2', out,
  link_args : ['-L.', '-lnoversion']))

test('manually linked 3', executable('manuallink3', out,
  link_args : ['-L.', '-lonlyversion']))

test('manually linked 4', executable('manuallink4', out,
  link_args : ['-L.', '-lonlysoversion']))
