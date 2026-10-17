project('abyssland-protocols',
	version: run_command('cat', 'VERSION', check: true).stdout().strip(),
	meson_version: '>= 0.60.3',
)

abyssland_protocols_version = meson.project_version()

fs = import('fs')

protocols = {
  'abyssland-toplevel-export': ['v1'],
  'abyssland-toplevel-mapping': ['v1'],
  'abyssland-global-shortcuts': ['v1'],
  'abyssland-focus-grab': ['v1'],
  'abyssland-ctm-control': ['v1'],
  'abyssland-surface': ['v1'],
  'abyssland-lock-notify': ['v1'],
  'abyssland-input-capture': ['v1'],
}

protocol_files = []

foreach name : protocols.keys()
	foreach version : protocols.get(name)
		protocol_files += [
			'protocols/@0@-@1@.xml'.format(name, version)
		]
	endforeach
endforeach

foreach protocol_file : protocol_files
	protocol_install_dir = fs.parent(join_paths(
		get_option('datadir'),
		'abyssland-protocols',
		protocol_file,
	))
	install_data(
		protocol_file,
		install_dir: protocol_install_dir,
	)
endforeach

abyssland_protocols_srcdir = meson.current_source_dir()

pkgconfig_configuration = configuration_data()
pkgconfig_configuration.set('prefix', get_option('prefix'))
pkgconfig_configuration.set('datarootdir', join_paths('${prefix}', get_option('datadir')))
pkgconfig_configuration.set('abs_top_srcdir', abyssland_protocols_srcdir)
pkgconfig_configuration.set('PACKAGE', 'abyssland-protocols')
pkgconfig_configuration.set('ABYSSLAND_PROTOCOLS_VERSION', abyssland_protocols_version)

pkg_install_dir = join_paths(get_option('datadir'), 'pkgconfig')
configure_file(
	input: 'abyssland-protocols.pc.in',
	output: 'abyssland-protocols.pc',
	configuration: pkgconfig_configuration,
	install_dir: pkg_install_dir,
)

abyssland_protocols = declare_dependency(
	variables: {
		'pkgdatadir': abyssland_protocols_srcdir,
	},
)

meson.override_dependency('abyssland-protocols', abyssland_protocols)
