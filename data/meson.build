icondir = join_paths('icons', 'hicolor', 'scalable', 'apps')
install_data(
  join_paths(icondir, 'org.gnome.Nautilus' + profile + '.svg'),
  install_dir: join_paths(datadir, icondir),
  rename: '@0@.svg'.format(application_id)
)

icondir = join_paths('icons', 'hicolor', 'symbolic', 'apps')

install_data(
  join_paths(icondir, 'org.gnome.Nautilus-symbolic.svg'),
  install_dir: join_paths(datadir, icondir),
  rename: '@0@-symbolic.svg'.format(application_id)
)

po_dir = join_paths(meson.project_source_root(), 'po')

desktop_conf = configuration_data()

desktop_conf.set('icon', application_id)

desktop = i18n.merge_file(
  input: configure_file(
    input: files('org.gnome.Nautilus.desktop.in.in'),
    output: 'org.gnome.Nautilus.desktop.in',
    configuration: desktop_conf
  ),
  output: '@0@.desktop'.format(application_id),
  install: true,
  install_dir: desktopdir,
  po_dir: po_dir,
  type: 'desktop'
)

desktop_autorun_software = i18n.merge_file(
  input: 'nautilus-autorun-software.desktop.in',
  output: 'nautilus-autorun-software.desktop',
  install: true,
  install_dir: desktopdir,
  po_dir: po_dir,
  type: 'desktop'
)

# get current date, used as build date for snapshots between releases
today = 'unknown'
date = find_program('date', required: false)
# Only set dates for nightly builds to make release builds reproducible
if date.found() and profile == 'Devel'
  r = run_command(date, '-I', check: true)
  today = r.stdout().strip()
endif

appdata_conf = configuration_data()

appdata_conf.set('appid', application_id)
appdata_conf.set('build-date', today)

release_version = meson.project_version()
python = find_program('python3', required: false)
if python.found()
  args = '''exec("import re\nprint(re.sub(r'^(.+)(\\.)([a-z]+.*)$', r'\\1~\\3', '@0@'))")'''
  args = args.format(meson.project_version())
  release_version = run_command(python, '-c', args, check: true).stdout().strip()
endif
appdata_conf.set('release-version', release_version)

appdata = i18n.merge_file(
  input: configure_file(
    input: files('org.gnome.Nautilus.metainfo.xml.in.in'),
    output: 'org.gnome.Nautilus.metainfo.xml.in',
    configuration: appdata_conf
  ),
  output: '@0@.metainfo.xml'.format(application_id),
  install: true,
  install_dir: join_paths(datadir, 'metainfo'),
  po_dir: po_dir
)

service_conf = configuration_data()
service_conf.set('appid', application_id)
service_conf.set('bindir', join_paths(prefix, bindir))

configure_file(
  input: 'org.gnome.Nautilus.service.in',
  output: '@0@.service'.format(application_id),
  configuration: service_conf,
  install_dir: servicedir
)

configure_file(
  input: 'org.freedesktop.FileManager1.service.in',
  output: 'org.freedesktop.FileManager1.service',
  configuration: service_conf,
  install_dir: servicedir
)

search_provider_conf = configuration_data()

search_provider_conf.set('appid', application_id)
search_provider_conf.set('profile', profile)

configure_file(
  configuration: search_provider_conf,
  input: files('org.gnome.Nautilus.search-provider.ini.in'),
  install_dir: join_paths(datadir, 'gnome-shell', 'search-providers'),
  output: '@0@.search-provider.ini'.format(application_id)
)

gnome.compile_schemas(
  build_by_default: true,
  depend_files: 'org.gnome.nautilus.gschema.xml'
)

install_data(
  'org.gnome.nautilus.gschema.xml',
  install_dir: join_paths(datadir, 'glib-2.0', 'schemas')
)

desktop_file_validate = find_program('desktop-file-validate', required: false)
if desktop_file_validate.found()
  test(
    'validate-desktop',
    desktop_file_validate,
    args: [
      desktop.full_path()
    ],
    depends: [
      desktop,
    ]
  )
  test(
    'validate-desktop-autorun-software',
    desktop_file_validate,
    args: [
      desktop_autorun_software.full_path()
    ],
    depends: [
      desktop_autorun_software,
    ]
  )
endif

appstreamcli = find_program('appstreamcli', required: false)
if appstreamcli.found()
  test(
    'validate-appdata', appstreamcli,
    args: [
      'validate', '--no-net', appdata.full_path()
    ],
    depends: [
      appdata,
    ]
  )
endif

subdir('ontology')
