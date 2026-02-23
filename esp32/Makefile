build:
	docker compose run --rm esp32-dev idf.py build

flash-image:
	docker compose run --rm esp32-dev esptool.py \
		--chip esp32 merge_bin \
		-o /firmware/flash_image.bin \
		@flash_args

qemu:
	docker compose up esp32-qemu

gdb:
	docker compose run --rm esp32-dev xtensa-esp32-elf-gdb \
		build/your_app.elf \
		-ex "target remote localhost:5555"

shell:
	docker compose run --rm esp32-dev bash

clean:
	docker compose down -v
