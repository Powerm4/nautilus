tracker_sandbox = find_program('localsearch')

tests = [
  ['test-directory', [
    'test-directory.c'
  ]],
  ['test-file', [
    'test-file.c'
  ]],
  ['test-file-metadata', [
    'test-file-metadata.c'
  ]],
  ['test-file-operations-archive', [
    'test-file-operations-archive.c'
  ]],
  ['test-file-operations-copy-files', [
    'test-file-operations-copy-files.c'
  ]],
  ['test-file-operations-dir-has-files', [
    'test-file-operations-dir-has-files.c'
  ]],
  ['test-file-operations-move-files', [
    'test-file-operations-move-files.c'
  ]],
  ['test-file-operations-trash-or-delete', [
    'test-file-operations-trash-or-delete.c'
  ]],
  ['test-file-utilities-get-common-filename-prefix', [
    'test-file-utilities-get-common-filename-prefix.c'
  ]],
  ['test-filename-common-prefix', [
    'test-filename-common-prefix.c'
  ]],
  ['test-filename-utilities', [
    'test-filename-utilities.c'
  ]],
  ['test-nautilus-search-engine', [
    'test-nautilus-search-engine.c'
  ]],
  ['test-nautilus-search-engine-model', [
    'test-nautilus-search-engine-model.c'
  ]],
  ['test-nautilus-search-engine-simple', [
    'test-nautilus-search-engine-simple.c'
  ]],
  ['test-ui-utilities', [
    'test-ui-utilities.c'
  ]],
]

localsearch_tests = [
  ['test-nautilus-search-engine-localsearch', [
    'test-nautilus-search-engine-localsearch.c',
  ]],
]

# disable localsearch tests for now, until issues with accessing it from
# within the sandbox are resolved
localsearch_tests = []

foreach t: tests
  test(
    t[0],
    executable(t[0], t[1], dependencies: [libnautilus_dep, libtestutils_dep]),
    env: [
      test_env,
      'G_TEST_BUILDDIR=@0@'.format(meson.current_build_dir()),
      'G_TEST_SRCDIR=@0@'.format(meson.current_source_dir())
    ],
    timeout: 480,
    suite: ['displayless'],
  )
endforeach



# Tests that read and write from the Tracker index are run using 'tracker-sandbox'
# script to use a temporary instance of tracker-miner-fs instead of the session one.
foreach t: localsearch_tests
  test_exe = executable(t[0], t[1], dependencies: [libnautilus_dep, libtestutils_dep])
  test(
    t[0],
    tracker_sandbox,
    args: ['test-sandbox', '--store-tmpdir', '--index-recursive-tmpdir', test_exe],
    env: [
      test_env,
      'G_TEST_BUILDDIR=@0@'.format(meson.current_build_dir()),
      'G_TEST_SRCDIR=@0@'.format(meson.current_source_dir())
    ],
    timeout: 480,
    suite: ['displayless', 'tracker'],
  )
endforeach
