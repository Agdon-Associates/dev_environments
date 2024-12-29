FROM agdon01/dev_environments:kernel-dev AS base


# clone binutils and gcc
WORKDIR /tmp

RUN <<EOF
  git clone --branch binutils-2_43 git://sourceware.org/git/binutils-gdb.git binutils
  git clone git://gcc.gnu.org/git/gcc.git gcc
EOF

ARG PREFIX="/opt/i686-elf"
ARG TARGET="i686-elf"
ARG PATH="${PREFIX}/bin:$PATH"

# Configure binutils to build for the i686-elf target architecture
RUN <<EOF
  mkdir -p ${PREFIX}
  cd /tmp
  mkdir build-binutils
  cd build-binutils

  ../binutils/configure --target=${TARGET} --prefix=${PREFIX} \
    --with-sysroot --disable-nls --disable-werror
  make
  make install
EOF

# Build gcc for cross-compilation
RUN <<EOF
  cd /tmp
  mkdir build-gcc
  cd build-gcc
  ../gcc/contrib/download_prerequisites
  ../gcc/configure --target=${TARGET} --prefix="${PREFIX}" \
    --disable-nls --enable-languages=c,c++ --without-headers
  make all-gcc
  make all-target-libgcc
  make install-gcc
  make install-target-libgcc
EOF

FROM agdon01/dev_environments:kernel-dev

COPY --from=base /opt /opt