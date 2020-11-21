## Process this file with automake to produce Makefile.in

conformance_protoc_inputs =                                    \
  conformance.proto

well_known_type_protoc_inputs =                                \
  $(top_srcdir)/src/google/protobuf/any.proto                  \
  $(top_srcdir)/src/google/protobuf/duration.proto             \
  $(top_srcdir)/src/google/protobuf/field_mask.proto           \
  $(top_srcdir)/src/google/protobuf/struct.proto               \
  $(top_srcdir)/src/google/protobuf/timestamp.proto            \
  $(top_srcdir)/src/google/protobuf/wrappers.proto


protoc_outputs =                                               \
  conformance.pb.cc                                            \
  conformance.pb.h

other_language_protoc_outputs =                                \
  conformance_pb2.py                                           \
  conformance_pb.rb                                            \
  google/protobuf/any.pb.cc                                    \
  google/protobuf/any.pb.h                                     \
  google/protobuf/any.rb                                       \
  google/protobuf/any_pb2.py                                   \
  google/protobuf/duration.pb.cc                               \
  google/protobuf/duration.pb.h                                \
  google/protobuf/duration.rb                                  \
  google/protobuf/duration_pb2.py                              \
  google/protobuf/field_mask.pb.cc                             \
  google/protobuf/field_mask.pb.h                              \
  google/protobuf/field_mask.rb                                \
  google/protobuf/field_mask_pb2.py                            \
  google/protobuf/struct.pb.cc                                 \
  google/protobuf/struct.pb.h                                  \
  google/protobuf/struct.rb                                    \
  google/protobuf/struct_pb2.py                                \
  google/protobuf/timestamp.pb.cc                              \
  google/protobuf/timestamp.pb.h                               \
  google/protobuf/timestamp.rb                                 \
  google/protobuf/timestamp_pb2.py                             \
  google/protobuf/wrappers.pb.cc                               \
  google/protobuf/wrappers.pb.h                                \
  google/protobuf/wrappers.rb                                  \
  google/protobuf/wrappers_pb2.py

bin_PROGRAMS = conformance-test-runner conformance-cpp

# All source files excepet C++/Objective-C ones should be explicitly listed
# here because the autoconf tools don't include files of other languages
# automatically.
EXTRA_DIST =                  \
  README.md                   \
  conformance.proto           \
  conformance_python.py       \
  conformance_ruby.rb         \
  failure_list_cpp.txt        \
  failure_list_csharp.txt     \
  failure_list_python.txt     \
  failure_list_python_cpp.txt \
  failure_list_python-post26.txt \
  failure_list_ruby.txt

conformance_test_runner_LDADD = $(top_srcdir)/src/libprotobuf.la
conformance_test_runner_SOURCES = conformance_test.h conformance_test.cc \
                                  conformance_test_runner.cc             \
                                  third_party/jsoncpp/json.h             \
                                  third_party/jsoncpp/jsoncpp.cpp
nodist_conformance_test_runner_SOURCES = conformance.pb.cc
conformance_test_runner_CPPFLAGS = -I$(top_srcdir)/src -I$(srcdir)
conformance_test_runner_CXXFLAGS = -std=c++11
# Explicit deps beacuse BUILT_SOURCES are only done before a "make all/check"
# so a direct "make test_cpp" could fail if parallel enough.
conformance_test_runner-conformance_test.$(OBJEXT): conformance.pb.h
conformance_test_runner-conformance_test_runner.$(OBJEXT): conformance.pb.h

conformance_cpp_LDADD = $(top_srcdir)/src/libprotobuf.la
conformance_cpp_SOURCES = conformance_cpp.cc
nodist_conformance_cpp_SOURCES = conformance.pb.cc
conformance_cpp_CPPFLAGS = -I$(top_srcdir)/src
# Explicit dep beacuse BUILT_SOURCES are only done before a "make all/check"
# so a direct "make test_cpp" could fail if parallel enough.
conformance_cpp-conformance_cpp.$(OBJEXT): conformance.pb.h

if USE_EXTERNAL_PROTOC

# Some implementations include pre-generated versions of well-known types.
protoc_middleman: $(conformance_protoc_inputs) $(well_known_type_protoc_inputs)
	$(PROTOC) -I$(srcdir) -I$(top_srcdir) --cpp_out=. --ruby_out=. --python_out=. $(conformance_protoc_inputs)
	$(PROTOC) -I$(srcdir) -I$(top_srcdir) --cpp_out=. --ruby_out=. --python_out=. $(well_known_type_protoc_inputs)
	## $(PROTOC) -I$(srcdir) -I$(top_srcdir) $(conformance_protoc_inputs) $(well_known_type_protoc_inputs)
	touch protoc_middleman

else

# We have to cd to $(srcdir) before executing protoc because $(protoc_inputs) is
# relative to srcdir, which may not be the same as the current directory when
# building out-of-tree.
protoc_middleman: $(top_srcdir)/src/protoc$(EXEEXT) $(conformance_protoc_inputs) $(well_known_type_protoc_inputs)
	oldpwd=`pwd` && ( cd $(srcdir) && $$oldpwd/../src/protoc$(EXEEXT) -I. -I$(top_srcdir)/src --cpp_out=$$oldpwd --ruby_out=$$oldpwd --python_out=$$oldpwd $(conformance_protoc_inputs) )
	oldpwd=`pwd` && ( cd $(srcdir) && $$oldpwd/../src/protoc$(EXEEXT) -I. -I$(top_srcdir)/src --cpp_out=$$oldpwd --ruby_out=$$oldpwd --python_out=$$oldpwd $(well_known_type_protoc_inputs) )
	## @mkdir -p lite
	## oldpwd=`pwd` && ( cd $(srcdir) && $$oldpwd/../src/protoc$(EXEEXT) -I. -I$(top_srcdir)/src $(conformance_protoc_inputs) $(well_known_type_protoc_inputs) )
	touch protoc_middleman

endif

$(protoc_outputs): protoc_middleman

$(other_language_protoc_outputs): protoc_middleman

BUILT_SOURCES = $(protoc_outputs) $(other_language_protoc_outputs)

CLEANFILES = $(protoc_outputs) protoc_middleman conformance-csharp $(other_language_protoc_outputs)

MAINTAINERCLEANFILES =   \
  Makefile.in

# Currently the conformance code is alongside the rest of the C#
# source, as it's easier to maintain there. We assume we've already
# built that, so we just need a script to run it.
conformance-csharp: $(other_language_protoc_outputs)
	@echo "Writing shortcut script conformance-csharp..."
	@echo '#! /bin/sh' > conformance-csharp
	@echo 'dotnet ../csharp/src/Google.Protobuf.Conformance/bin/Release/netcoreapp1.0/Google.Protobuf.Conformance.dll "$$@"' >> conformance-csharp
	@chmod +x conformance-csharp

# Targets for actually running tests.
test_cpp: protoc_middleman conformance-test-runner conformance-cpp
	./conformance-test-runner --enforce_recommended --failure_list failure_list_cpp.txt ./conformance-cpp

test_csharp: protoc_middleman conformance-test-runner conformance-csharp
	./conformance-test-runner --enforce_recommended --failure_list failure_list_csharp.txt ./conformance-csharp

test_ruby: protoc_middleman conformance-test-runner $(other_language_protoc_outputs)
	RUBYLIB=../ruby/lib:. ./conformance-test-runner --enforce_recommended --failure_list failure_list_ruby.txt ./conformance_ruby.rb

# These depend on library paths being properly set up.  The easiest way to
# run them is to just use "tox" from the python dir.
test_python: protoc_middleman conformance-test-runner
	./conformance-test-runner --enforce_recommended --failure_list failure_list_python.txt ./conformance_python.py

test_python_cpp: protoc_middleman conformance-test-runner
	./conformance-test-runner --enforce_recommended --failure_list failure_list_python_cpp.txt ./conformance_python.py

