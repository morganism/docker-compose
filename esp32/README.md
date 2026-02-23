# ESP32 Virtual Dev Environment

Docker Compose setup for ESP32 development and QEMU emulation on Intel Mac.

## Directory Structure

```
esp32-dev/
├── docker-compose.yml   # Services: esp32-dev, esp32-qemu
├── Makefile             # Convenience targets
├── project/             # Your ESP-IDF project goes here
└── firmware/            # Merged flash images land here
```

## Quickstart

```bash
# Drop your ESP-IDF project into ./project/
# then:

make shell        # interactive shell inside IDF container
make build        # compile project
make flash-image  # create merged binary for QEMU
make qemu         # boot in QEMU (blocks, ctrl-c to stop)
make gdb          # attach GDB to running QEMU instance
make clean        # tear down containers and volumes
```

## GDB Debugging

QEMU starts with `-S` (halted, waiting for debugger). In a second terminal:

```bash
make gdb
# then in GDB:
(gdb) continue
```

## QEMU Caveat

If `qemu-system-xtensa` from apt doesn't support the `esp32` machine target,
you'll see an error. In that case you need Espressif's QEMU fork built from source:
https://github.com/espressif/qemu

A multi-stage Dockerfile to build it can be added on request.

## New IDF Project

```bash
make shell
cp -r $IDF_PATH/examples/get-started/hello_world /project/
cd /project/hello_world
idf.py set-target esp32
idf.py build
```
