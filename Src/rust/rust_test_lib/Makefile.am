noinst_libraries = librust_test_lib.a
librust_test_lib_a_sources = $(srcdir)/src/lib.rs

src = $(wildcard src/*.rs)

all: target/debug/librust_test_lib.a target/include/rust_test_lib.h

install: all

target/debug/librust_test_lib.a: $(src)
	cd $(srcdir) && cargo build

target/include/rust_test_lib.h: src/lib.rs
	cd $(srcdir) && cbindgen --config cbindgen.toml --output $@