project('wl-gammactl', 'c')

# Define dependecies
dep_gtk3 = dependency('gtk+-3.0')
dep_wlroots = dependency('wlroots-0.19')
dep_wayland_client = dependency('wayland-client')

# Compile gresources
gres_xml = join_paths(meson.project_source_root(), 'glade/window_main.gresource.xml')
gres_src = join_paths(meson.project_source_root(), 'src/resources.c')
glib_compiler = find_program('glib-compile-resources')
run_command(
	glib_compiler,
	'--target=@0@'.format(gres_src),
	'--generate-source', gres_xml,
	check: true,
)

# # Get protocol extensions
# git = find_program('git')
# run_command(
# 	git,
# 	'submodule',
# 	'add',
# 	'https://github.com/swaywm/wlr-protocols.git',
# 	'wlr-protocols/',
# 	check: true,
# )
# run_command(git, 'submodule', 'update', '--init', '--recursive', check: true)

# Run wayland-scanner
wayland_scanner = find_program('wayland-scanner')
run_command(
	wayland_scanner,
	'private-code',
	'wlr-protocols/unstable/wlr-gamma-control-unstable-v1.xml',
	'src/wlr-gamma-control-unstable-v1-client-protocol.c',
	check: true,
)
run_command(
	wayland_scanner,
	'client-header',
	'wlr-protocols/unstable/wlr-gamma-control-unstable-v1.xml',
	'src/wlr-gamma-control-unstable-v1-client-protocol.h',
	check: true,
)

sources = [
	'src/main.c',
	'src/wlr-gamma-control-unstable-v1-client-protocol.c',
	'src/wlr-gamma-control-unstable-v1-client-protocol.h',
	'src/resources.c',
]

executable(
	'wl-gammactl',
	sources,
	dependencies: [dep_wlroots, dep_wayland_client, dep_gtk3],
	link_args: ['-rdynamic', '-lm'],
	install: true,
)