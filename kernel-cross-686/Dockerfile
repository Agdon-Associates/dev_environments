FROM agdon01/dev_environments:kernel-dev AS base


# Install binutils
WORKDIR /tmp

RUN <<EOF
  git clone git://sourceware.org/git/binutils-gdb.git binutils
  git clone git://gcc.gnu.org/git/gcc.git
EOF

ARG PREFIX="/opt/gcc-i686/cross"
ARG TARGET="i686-elf"
ARG PATH="${PREFIX}/bin:$PATH"

RUN <<EOF
  cd /tmp/binutils

# Configure binutils to build for the i686-elf target architecture
  mkdir -p ${PREFIX}
  ./configure --target=${TARGET} --prefix=${PREFIX} \
    --with-sysroot --disable-nls --disable-werror
  make
  make install
  cd /tmp
  #rm -rf binutils
EOF

# Build gcc for cross-compilation
RUN <<EOF
  
  cd /tmp/gcc
  ./contrib/download_prerequisites
  cd /tmp
  mkdir build-gcc
  cd /tmp/build-gcc
  ../gcc/configure --target=${TARGET} --prefix="${PREFIX}" \
    --disable-nls --enable-languages=c,c++ --without-headers
  make all-gcc
  make all-target-libgcc
  make install-gcc
  make install-target-libgcc
  cd /tmp
  #rm -rf gcc
EOF

FROM agdon01/dev_environments:kernel


COPY --from=base /opt /opt