## Process this file with automake to produce Makefile.in

ACLOCAL_AMFLAGS = -I m4

AUTOMAKE_OPTIONS = foreign

# Build . before src so that our all-local and clean-local hooks kicks in at
# the right time.
SUBDIRS = . src

# Always include gmock in distributions.
DIST_SUBDIRS = $(subdirs) src conformance benchmarks

# Build gmock before we build protobuf tests.  We don't add gmock to SUBDIRS
# because then "make check" would also build and run all of gmock's own tests,
# which takes a lot of time and is generally not useful to us.  Also, we don't
# want "make install" to recurse into gmock since we don't want to overwrite
# the installed version of gmock if there is one.
check-local:
	@echo "Making lib/libgmock.a lib/libgmock_main.a in gmock"
	@cd gmock && $(MAKE) $(AM_MAKEFLAGS) lib/libgmock.la lib/libgmock_main.la
	@cd gmock/gtest && $(MAKE) $(AM_MAKEFLAGS) lib/libgtest.la lib/libgtest_main.la

# We would like to clean gmock when "make clean" is invoked.  But we have to
# be careful because clean-local is also invoked during "make distclean", but
# "make distclean" already recurses into gmock because it's listed among the
# DIST_SUBDIRS.  distclean will delete gmock/Makefile, so if we then try to
# cd to the directory again and "make clean" it will fail.  So, check that the
# Makefile exists before recursing.
clean-local:
	@if test -e gmock/Makefile; then \
	  echo "Making clean in gmock"; \
	  cd gmock && $(MAKE) $(AM_MAKEFLAGS) clean; \
	fi; \
	if test -e conformance/Makefile; then \
	  echo "Making clean in conformance"; \
	  cd conformance && $(MAKE) $(AM_MAKEFLAGS) clean; \
	fi; \
	if test -e benchmarks/Makefile; then \
	  echo "Making clean in benchmarks"; \
	  cd benchmarks && $(MAKE) $(AM_MAKEFLAGS) clean; \
	fi; \

pkgconfigdir = $(libdir)/pkgconfig
pkgconfig_DATA = protobuf.pc protobuf-lite.pc

php_EXTRA_DIST=                                              \
  php/src/phpdoc.dist.xml                                    \
  php/src/Google/Protobuf/Internal/DescriptorPool.php        \
  php/src/Google/Protobuf/Internal/OneofField.php            \
  php/src/Google/Protobuf/Internal/MapEntry.php              \
  php/src/Google/Protobuf/Internal/Type.php                  \
  php/src/Google/Protobuf/Internal/InputStream.php           \
  php/src/Google/Protobuf/Internal/OutputStream.php          \
  php/src/Google/Protobuf/Internal/MessageBuilderContext.php \
  php/src/Google/Protobuf/Internal/MapField.php              \
  php/src/Google/Protobuf/Internal/RepeatedField.php         \
  php/src/Google/Protobuf/Internal/Message.php               \
  php/src/Google/Protobuf/Internal/GPBWire.php               \
  php/src/Google/Protobuf/Internal/GPBType.php               \
  php/src/Google/Protobuf/Internal/GPBLabel.php              \
  php/src/Google/Protobuf/Internal/EnumBuilderContext.php    \
  php/src/Google/Protobuf/Internal/GPBUtil.php               \
  php/src/Google/Protobuf/descriptor_internal.pb.php         \
  php/src/Google/Protobuf/descriptor.php                     \
  php/tests/encode_decode_test.php                           \
  php/tests/test.sh                                          \
  php/tests/generated_class_test.php                         \
  php/tests/array_test.php                                   \
  php/tests/php_implementation_test.php                      \
  php/tests/test_include.proto                               \
  php/tests/test_include.pb.php                              \
  php/tests/map_field_test.php                               \
  php/tests/test_base.php                                    \
  php/tests/test_util.php                                    \
  php/tests/test.proto                                       \
  php/tests/test.pb.php                                      \
  php/tests/memory_leak_test.php                             \
  php/README.md                                              \
  php/ext/google/protobuf/utf8.h                             \
  php/ext/google/protobuf/message.c                          \
  php/ext/google/protobuf/utf8.c                             \
  php/ext/google/protobuf/package.xml                        \
  php/ext/google/protobuf/upb.h                              \
  php/ext/google/protobuf/array.c                            \
  php/ext/google/protobuf/encode_decode.c                    \
  php/ext/google/protobuf/protobuf.h                         \
  php/ext/google/protobuf/type_check.c                       \
  php/ext/google/protobuf/def.c                              \
  php/ext/google/protobuf/storage.c                          \
  php/ext/google/protobuf/map.c                              \
  php/ext/google/protobuf/config.m4                          \
  php/ext/google/protobuf/upb.c                              \
  php/ext/google/protobuf/protobuf.c                         \
  phpunit.xml                                                \
  composer.json

python_EXTRA_DIST=                                                           \
  python/MANIFEST.in                                                         \
  python/google/__init__.py                                                  \
  python/google/protobuf/__init__.py                                         \
  python/google/protobuf/descriptor.py                                       \
  python/google/protobuf/descriptor_database.py                              \
  python/google/protobuf/descriptor_pool.py                                  \
  python/google/protobuf/internal/__init__.py                                \
  python/google/protobuf/internal/_parameterized.py                          \
  python/google/protobuf/internal/any_test.proto                             \
  python/google/protobuf/internal/any_test.proto                             \
  python/google/protobuf/internal/api_implementation.cc                      \
  python/google/protobuf/internal/api_implementation.py                      \
  python/google/protobuf/internal/containers.py                              \
  python/google/protobuf/internal/decoder.py                                 \
  python/google/protobuf/internal/descriptor_database_test.py                \
  python/google/protobuf/internal/descriptor_pool_test.py                    \
  python/google/protobuf/internal/descriptor_pool_test1.proto                \
  python/google/protobuf/internal/descriptor_pool_test2.proto                \
  python/google/protobuf/internal/descriptor_test.py                         \
  python/google/protobuf/internal/encoder.py                                 \
  python/google/protobuf/internal/enum_type_wrapper.py                       \
  python/google/protobuf/internal/factory_test1.proto                        \
  python/google/protobuf/internal/factory_test2.proto                        \
  python/google/protobuf/internal/file_options_test.proto                    \
  python/google/protobuf/internal/generator_test.py                          \
  python/google/protobuf/internal/import_test_package/__init__.py            \
  python/google/protobuf/internal/import_test_package/inner.proto            \
  python/google/protobuf/internal/import_test_package/outer.proto            \
  python/google/protobuf/internal/json_format_test.py                        \
  python/google/protobuf/internal/message_factory_test.py                    \
  python/google/protobuf/internal/message_listener.py                        \
  python/google/protobuf/internal/message_set_extensions.proto               \
  python/google/protobuf/internal/message_test.py                            \
  python/google/protobuf/internal/missing_enum_values.proto                  \
  python/google/protobuf/internal/more_extensions.proto                      \
  python/google/protobuf/internal/more_extensions_dynamic.proto              \
  python/google/protobuf/internal/more_messages.proto                        \
  python/google/protobuf/internal/packed_field_test.proto                    \
  python/google/protobuf/internal/proto_builder_test.py                      \
  python/google/protobuf/internal/python_message.py                          \
  python/google/protobuf/internal/reflection_test.py                         \
  python/google/protobuf/internal/service_reflection_test.py                 \
  python/google/protobuf/internal/symbol_database_test.py                    \
  python/google/protobuf/internal/test_bad_identifiers.proto                 \
  python/google/protobuf/internal/test_util.py                               \
  python/google/protobuf/internal/testing_refleaks.py                        \
  python/google/protobuf/internal/text_encoding_test.py                      \
  python/google/protobuf/internal/text_format_test.py                        \
  python/google/protobuf/internal/type_checkers.py                           \
  python/google/protobuf/internal/unknown_fields_test.py                     \
  python/google/protobuf/internal/well_known_types.py                        \
  python/google/protobuf/internal/well_known_types.py                        \
  python/google/protobuf/internal/well_known_types_test.py                   \
  python/google/protobuf/internal/well_known_types_test.py                   \
  python/google/protobuf/internal/wire_format.py                             \
  python/google/protobuf/internal/wire_format_test.py                        \
  python/google/protobuf/json_format.py                                      \
  python/google/protobuf/message.py                                          \
  python/google/protobuf/message_factory.py                                  \
  python/google/protobuf/proto_builder.py                                    \
  python/google/protobuf/pyext/README                                        \
  python/google/protobuf/pyext/__init__.py                                   \
  python/google/protobuf/pyext/cpp_message.py                                \
  python/google/protobuf/pyext/descriptor.cc                                 \
  python/google/protobuf/pyext/descriptor.h                                  \
  python/google/protobuf/pyext/descriptor_containers.cc                      \
  python/google/protobuf/pyext/descriptor_containers.h                       \
  python/google/protobuf/pyext/descriptor_database.cc                        \
  python/google/protobuf/pyext/descriptor_database.h                         \
  python/google/protobuf/pyext/descriptor_pool.cc                            \
  python/google/protobuf/pyext/descriptor_pool.h                             \
  python/google/protobuf/pyext/extension_dict.cc                             \
  python/google/protobuf/pyext/extension_dict.h                              \
  python/google/protobuf/pyext/map_container.cc                              \
  python/google/protobuf/pyext/map_container.h                               \
  python/google/protobuf/pyext/message.cc                                    \
  python/google/protobuf/pyext/message.h                                     \
  python/google/protobuf/pyext/message_factory.cc                            \
  python/google/protobuf/pyext/message_factory.h                             \
  python/google/protobuf/pyext/message_module.cc                             \
  python/google/protobuf/pyext/proto2_api_test.proto                         \
  python/google/protobuf/pyext/python.proto                                  \
  python/google/protobuf/pyext/python_protobuf.h                             \
  python/google/protobuf/pyext/repeated_composite_container.cc               \
  python/google/protobuf/pyext/repeated_composite_container.h                \
  python/google/protobuf/pyext/repeated_scalar_container.cc                  \
  python/google/protobuf/pyext/repeated_scalar_container.h                   \
  python/google/protobuf/pyext/scoped_pyobject_ptr.h                         \
  python/google/protobuf/reflection.py                                       \
  python/google/protobuf/service.py                                          \
  python/google/protobuf/service_reflection.py                               \
  python/google/protobuf/symbol_database.py                                  \
  python/google/protobuf/text_encoding.py                                    \
  python/google/protobuf/text_format.py                                      \
  python/mox.py                                                              \
  python/setup.cfg                                                           \
  python/setup.py                                                            \
  python/stubout.py                                                          \
  python/tox.ini                                                             \
  python/README.md

ruby_EXTRA_DIST=                                                             \
  ruby/Gemfile                                                               \
  ruby/Gemfile.lock                                                          \
  ruby/.gitignore                                                            \
  ruby/README.md                                                             \
  ruby/Rakefile                                                              \
  ruby/ext/google/protobuf_c/defs.c                                          \
  ruby/ext/google/protobuf_c/encode_decode.c                                 \
  ruby/ext/google/protobuf_c/extconf.rb                                      \
  ruby/ext/google/protobuf_c/map.c                                           \
  ruby/ext/google/protobuf_c/message.c                                       \
  ruby/ext/google/protobuf_c/protobuf.c                                      \
  ruby/ext/google/protobuf_c/protobuf.h                                      \
  ruby/ext/google/protobuf_c/repeated_field.c                                \
  ruby/ext/google/protobuf_c/storage.c                                       \
  ruby/ext/google/protobuf_c/upb.c                                           \
  ruby/ext/google/protobuf_c/upb.h                                           \
  ruby/google-protobuf.gemspec                                               \
  ruby/lib/google/protobuf/message_exts.rb                                   \
  ruby/lib/google/protobuf/repeated_field.rb                                 \
  ruby/lib/google/protobuf/well_known_types.rb                               \
  ruby/lib/google/protobuf.rb                                                \
  ruby/pom.xml                                                               \
  ruby/src/main/sentinel.proto                                               \
  ruby/tests/basic.rb                                                        \
  ruby/tests/repeated_field_test.rb                                          \
  ruby/tests/stress.rb                                                       \
  ruby/tests/generated_code.proto                                            \
  ruby/tests/test_import.proto                                               \
  ruby/tests/generated_code_test.rb                                          \
  ruby/tests/well_known_types_test.rb                                        \
  ruby/travis-test.sh

js_EXTRA_DIST=                              \
  js/README.md                              \
  js/binary/arith.js                        \
  js/binary/arith_test.js                   \
  js/binary/constants.js                    \
  js/binary/decoder.js                      \
  js/binary/decoder_test.js                 \
  js/binary/encoder.js                      \
  js/binary/proto_test.js                   \
  js/binary/reader.js                       \
  js/binary/reader_test.js                  \
  js/binary/utils.js                        \
  js/binary/utils_test.js                   \
  js/binary/writer.js                       \
  js/binary/writer_test.js                  \
  js/commonjs/export.js                     \
  js/commonjs/export_asserts.js             \
  js/commonjs/export_testdeps.js            \
  js/commonjs/import_test.js                \
  js/commonjs/jasmine.json                  \
  js/commonjs/rewrite_tests_for_commonjs.js \
  js/commonjs/test6/test6.proto             \
  js/commonjs/test7/test7.proto             \
  js/data.proto                             \
  js/debug.js                               \
  js/debug_test.js                          \
  js/gulpfile.js                            \
  js/jasmine.json                           \
  js/map.js                                 \
  js/maps_test.js                           \
  js/message.js                             \
  js/message_test.js                        \
  js/node_loader.js                         \
  js/package.json                           \
  js/proto3_test.js                         \
  js/proto3_test.proto                      \
  js/test.proto                             \
  js/test2.proto                            \
  js/test3.proto                            \
  js/test4.proto                            \
  js/test5.proto                            \
  js/test_bootstrap.js                      \
  js/testbinary.proto                       \
  js/testempty.proto

all_EXTRA_DIST=$(php_EXTRA_DIST) $(python_EXTRA_DIST) $(ruby_EXTRA_DIST) $(js_EXTRA_DIST)

EXTRA_DIST = $(@DIST_LANG@_EXTRA_DIST)   \
  autogen.sh                             \
  generate_descriptor_proto.sh           \
  README.md                              \
  LICENSE                                \
  CONTRIBUTORS.txt                       \
  CHANGES.txt                            \
  update_file_lists.sh                   \
  BUILD                                  \
  gmock.BUILD                            \
  WORKSPACE                              \
  cmake/CMakeLists.txt                   \
  cmake/README.md                        \
  cmake/examples.cmake                   \
  cmake/extract_includes.bat.in          \
  cmake/install.cmake                    \
  cmake/libprotobuf.cmake                \
  cmake/libprotobuf-lite.cmake           \
  cmake/libprotoc.cmake                  \
  cmake/protobuf-config-version.cmake.in \
  cmake/protobuf-config.cmake.in         \
  cmake/protobuf-module.cmake.in         \
  cmake/protobuf-options.cmake           \
  cmake/protoc.cmake                     \
  cmake/tests.cmake                      \
  editors/README.txt                     \
  editors/proto.vim                      \
  editors/protobuf-mode.el               \
  examples/CMakeLists.txt                \
  examples/README.txt                    \
  examples/Makefile                      \
  examples/addressbook.proto             \
  examples/add_person.cc                 \
  examples/add_person.go                 \
  examples/add_person_test.go            \
  examples/list_people.cc                \
  examples/list_people.go                \
  examples/CMakeLists.txt                \
  examples/add_person.py                 \
  examples/list_people.py                \
  examples/list_people_test.go           \
  protobuf.bzl                           \
  six.BUILD                              \
  util/python/BUILD

# Deletes all the files generated by autogen.sh.
MAINTAINERCLEANFILES =   \
  aclocal.m4             \
  ar-lib                 \
  config.guess           \
  config.sub             \
  configure              \
  depcomp                \
  install-sh             \
  ltmain.sh              \
  Makefile.in            \
  missing                \
  mkinstalldirs          \
  config.h.in            \
  stamp.h.in             \
  m4/ltsugar.m4          \
  m4/libtool.m4          \
  m4/ltversion.m4        \
  m4/lt~obsolete.m4      \
  m4/ltoptions.m4
