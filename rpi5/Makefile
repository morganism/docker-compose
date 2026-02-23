IMAGE_URL  = https://downloads.raspberrypi.com/raspios_lite_arm64/images/raspios_lite_arm64-2024-11-19/2024-11-19-raspios-bookworm-arm64-lite.img.xz
IMAGE_XZ   = sdcard/rpi-os.img.xz
IMAGE_RAW  = sdcard/rpi-os.img
DATA_DISK  = sdcard/data.qcow2

download-image:
	@echo "Downloading Raspberry Pi OS Lite (arm64)..."
	curl -L -o $(IMAGE_XZ) $(IMAGE_URL)
	xz -d $(IMAGE_XZ)
	@echo "Image ready: $(IMAGE_RAW)"

create-data-disk:
	docker run --rm -v $(PWD)/sdcard:/sdcard debian:bookworm-slim \
		bash -c "apt-get update -q && apt-get install -y -q qemu-utils && \
		qemu-img create -f qcow2 /sdcard/data.qcow2 32G"

expand-image:
	docker run --rm -v $(PWD)/sdcard:/sdcard debian:bookworm-slim \
		bash -c "apt-get update -q && apt-get install -y -q qemu-utils && \
		qemu-img resize /sdcard/rpi-os.img 8G"

setup: download-image expand-image create-data-disk
	@echo "Setup complete. Run: make start"

start:
	docker compose up rpi5-qemu

start-detached:
	docker compose up -d rpi5-qemu

stop:
	docker compose down

ssh:
	ssh -p 2222 -i ssh-keys/id_rpi5 pi@localhost

ssh-copy-id:
	ssh-keygen -t ed25519 -f ssh-keys/id_rpi5 -N ""
	ssh-copy-id -i ssh-keys/id_rpi5.pub -p 2222 pi@localhost

vnc:
	@echo "Connect VNC to localhost:5900"
	open vnc://localhost:5900 2>/dev/null || echo "Open your VNC client to localhost:5900"

logs:
	docker compose logs -f rpi5-qemu

shell:
	docker compose exec rpi5-qemu bash

clean:
	docker compose down -v
	rm -f $(IMAGE_RAW) $(IMAGE_XZ) $(DATA_DISK)
