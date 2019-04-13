# [Unofficial] RISC-V Dockerfiles

Dockerfiles for [RISC-V] - The Free and Open RISC Instruction Set Architecture.

## Dockers

All docker images can be found in [my page][my-docker-hub-url] at Docker Hub
Registry.

### RISCV-GNU-Toolchain

[RISC-V GNU Compiler Toolchain] is the RISC-V C and C++ cross-compiler.

Supported tags and respective `Dockerfile` links:
  - [`rv64imc-newlib-20190414`]

Start the docker as follows:

```bash
docker run --rm -it \
    yangby0cryptape/riscv-gnu-toolchain:rv64imc-newlib-20190414 \
    /bin/bash
```

### RISCV-Tools

[RISC-V Tools] is a set of RISC-V simulators and other tools
(include [RISC-V GNU Compiler Toolchain]).

Supported tags and respective `Dockerfile` links:
  - [`rv64imc-newlib-20190414`]

Start the docker as follows:

```bash
docker run --rm -it \
    yangby0cryptape/riscv-tools:rv64imc-newlib-20190414 \
    /bin/bash
```

## Testing

Start the `riscv-tools` docker:

```bash
docker run --rm -it \
    yangby0cryptape/riscv-tools:rv64imc-newlib-20190414 \
    /bin/bash
```

Create a "Hello world!" program:

```bash
echo -e '#include <stdio.h>\n int main(void) { printf("Hello world!\\n"); return 0; }' > hello.c
```

Then, build it with `riscv64-unknown-elf-gcc`.

```bash
riscv64-unknown-elf-gcc -o hello hello.c
```

Run it by the RISC-V architectural simulator:

```bash
spike pk hello
```

Last, enjoy it!

[RISC-V]: https://riscv.org/
[RISC-V GNU Compiler Toolchain]: https://github.com/riscv/riscv-gnu-toolchain
[RISC-V Tools]: https://github.com/riscv/riscv-tools
[my-docker-hub-url]: https://hub.docker.com/u/yangby0cryptape/
[`rv64imc-newlib-20190414`]: riscv-gnu-toolchain
[`rv64imc-newlib-20190414`]: riscv-tools
