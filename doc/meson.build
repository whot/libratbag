if get_option('documentation')

cmd_sphinx = find_program('sphinx-build-3')
cmd_doxygen = find_program('doxygen')

doc_config = configuration_data()
doc_config.set('PACKAGE_NAME', meson.project_name())
doc_config.set('PACKAGE_VERSION', meson.project_version())
doc_config.set('top_srcdir', meson.source_root())
doc_config.set('top_builddir', meson.build_root())

# We have a dependency chain here:
# - generate doxygen xml from the source
# - convert the xml to rst with xstlproc
# - generate the sphinx conf
# - run sphinx against the generated rst
doxyfile = configure_file(input : 'libratbag.doxygen.in',
                          output : 'libratbag.doxygen',
                          configuration : doc_config,
                          install : false)

src_doxygen = [ doxyfile,
                files('../src/libratbag-enums.h') ]
doxygen = custom_target('doxygen',
			input : src_doxygen,
			output : [ 'xml' ],
			command : [ cmd_doxygen, doxyfile ],
			install : false,
			build_by_default : true)

dst_rsts = files('index.rst', 'dbus.rst', 'enums.rst')

sphinx_conf = configure_file(input : 'conf.py.in',
			     output : 'conf.py',
			     configuration : doc_config,
			     install : false)

custom_target('sphinx',
		input : [ sphinx_conf, dst_rsts ],
		output : 'html',
		command : [ cmd_sphinx, '-b', 'html', '-q', '-n',
                            '-c', join_paths(meson.build_root(), 'doc'),
				join_paths(meson.source_root(), 'doc'),
				join_paths(meson.build_root(), 'doc', 'html') ],
                build_by_default : true,
		install : false)

endif
