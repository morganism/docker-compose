# Raspberry Pi 5 VM (QEMU + Docker)

Emulates a Raspberry Pi 5 class machine (ARM64 Cortex-A76) using QEMU inside Docker.

## Hardware Emulated

| Component   | Emulated As             |
|-------------|-------------------------|
| CPU         | ARM Cortex-A76 (4 core) |
| RAM         | 4GB                     |
| Storage     | virtio block device     |
| Network     | virtio-net (NAT)        |
| Display     | virtio-gpu (VNC)        |
| Machine     | QEMU virt (aarch64)     |

> Note: QEMU emulates ARM64, not the exact BCM2712 SoC. GPIO/hardware peripherals
> are not emulated. For peripheral dev you need real hardware or a Pico/ESP32.
> This is for OS-level, software, and Ruby/Python development.

## Directory Structure

```
rpi5-dev/
├── docker-compose.yml
├── Makefile
├── README.md
├── .gitignore
├── boot/          # boot files if needed
├── sdcard/        # disk images go here (gitignored)
├── ssh-keys/      # generated SSH keys (gitignored)
└── shared/        # shared folder mounted in VM
```

## First-Time Setup

```bash
# Download OS image, expand it, create data disk
make setup

# Start the VM (interactive console)
make start
```

Default RPi OS credentials: `pi` / `raspberry`

## Day-to-Day Usage

```bash
make start          # boot VM (console in terminal)
make start-detached # boot VM in background
make ssh            # SSH in on port 2222
make vnc            # open VNC (desktop builds)
make stop           # shut down
make clean          # remove everything including disk images
```

## KVM Acceleration

On Linux with KVM available, uncomment the `devices` section in docker-compose.yml
for near-native speed. On Intel Mac, KVM isn't available — QEMU runs in software
emulation mode (slower but functional).

## Shared Folder

Files placed in `./shared/` are available inside the VM at `/shared/`.

## Ruby on the Pi

```bash
make ssh
# then inside the Pi:
sudo apt install ruby-full
ruby --version
gem install bundler
```

Or use rbenv for version control:
```bash
git clone https://github.com/rbenv/rbenv.git ~/.rbenv
```
