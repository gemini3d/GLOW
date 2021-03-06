project('GLOW', 'fortran',
  version : '0.981',
  meson_version : '>= 0.51.2',
  default_options : ['default_library=static', 'buildtype=release', 'warning_level=0', 'fortran_std=legacy'])

fc = meson.get_compiler('fortran')
fargs_old = [fc.first_supported_argument(['-w', '-nowarn', '/nowarn']),
            fc.first_supported_argument(['-std=legacy', '-nostand', '/nostand'])]


cglow_src = files('glow.f90', 'cglow.f90', 'fieldm.f', 'solzen.f90', 'ssflux.f90', 'rcolum.f90', 'qback.f90', 'etrans.f90', 'exsect.f', 'gchem.f90', 'bands.f90', 'ephoto.f90', 'egrid.f90', 'maxt.f90')
snoem_src = files('snoem.f90', 'snoemint.f90', 'geomag.f90')
utils_src = files('mzgrid.f90', 'conduct.f90', 'iri90.f')

data_dir = meson.current_source_dir() / 'data'

# this avoids duplicated symbol linking error on MacOS
if not meson.is_subproject()
  utils_src += files('nrlmsise00.f')
endif

cglow = library('glow',
  sources: [cglow_src, snoem_src, utils_src],
  fortran_args : fargs_old)

if meson.is_subproject()
  subdir_done()
endif

glow_basic_exe = executable('glowbasic', 'glowbasic.f90',
  link_with: cglow)

test('glow_basic', glow_basic_exe,
  args: ['basic.nml'],
  workdir: meson.current_source_dir(),
  timeout: 10)
