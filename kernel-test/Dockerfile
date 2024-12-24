FROM agdon01/dev_environments:cpp as cppbase

# Install additional base packages.
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends \
    build-essential \
    bison \
    flex \
    libgmp3-dev \
    libmpc-dev \
    libmpfr-dev \
    texinfo \
    libisl-dev \
    nasm \
    grub-efi \
    xorriso \
    mtools \
    qemu-system-x86 \
    && apt-get -y autoremove \
    && apt-get -y clean \
    && rm -rf /var/lib/apt/lists/*

# Install binutils
WORKDIR /tmp
RUN git clone git://sourceware.org/git/binutils-gdb.git binutils
WORKDIR /tmp/binutils

# Configure binutils to build for the i686-elf target architecture
ARG PREFIX="/opt/gcc-i686/cross"
ARG TARGET="i686-elf"
ARG PATH="${PREFIX}/bin:$PATH"
RUN mkdir -p ${PREFIX}
RUN ./configure --target=${TARGET} --prefix=${PREFIX} \
    --with-sysroot --disable-nls --disable-werror
RUN make
RUN make install
WORKDIR /tmp
RUN rm -rf binutils

# Build gcc for cross-compilation
WORKDIR /tmp
RUN git clone git://gcc.gnu.org/git/gcc.git
WORKDIR /tmp/gcc
RUN ./contrib/download_prerequisites
WORKDIR /tmp
RUN mkdir build-gcc
WORKDIR /tmp/build-gcc
RUN ../gcc/configure --target=${TARGET} --prefix="${PREFIX}" \
    --disable-nls --enable-languages=c,c++ --without-headers
RUN make all-gcc
RUN make all-target-libgcc
RUN make install-gcc
RUN make install-target-libgcc
WORKDIR /tmp
RUN rm -rf gcc

FROM cppbase
