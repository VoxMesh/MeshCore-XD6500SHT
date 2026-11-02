# MeshCore XD6500SHT RF switch test

Experimental MeshCore v1.18.0 firmware for **ProMicro nRF52840 + XD6500SHT**.

Some boards receive LoRa packets normally but have poor transmission range. An RF switch GPIO change restored connectivity to distant repeaters during field testing of the BLE companion firmware. RF output power was not measured. The `SW_CTRL` wiring has not been verified against a schematic, so the change may not suit every PCB revision.

## Firmware

Download the Nordic nRF52 DFU ZIP files from [Releases](https://github.com/VoxMesh/MeshCore-XD6500SHT/releases):

- **BLE companion:** the change has a positive field result; the release binary has not been tested separately.
- **Repeater:** compiled, not yet hardware-tested.

These builds are intended only for matching ProMicro + XD6500SHT hardware with a compatible DFU bootloader. Set a unique repeater admin password before deployment.

Test results: [GitHub Issue #2](https://github.com/VoxMesh/MeshCore-XD6500SHT/issues/2).

## Source

Based on [MeshCore commit `81dbae592ffad40b4212bc0b37e82b38da08f4bc`](https://github.com/meshcore-dev/MeshCore/tree/81dbae592ffad40b4212bc0b37e82b38da08f4bc).

The [patch](xd6500sht-rf-switch.patch) disables `RXEN` on Arduino pin 2 (nRF52840 P0.17) and holds the pin HIGH. DIO2 RF switching and the 1.8 V TCXO setting remain unchanged.

Apply it to that exact MeshCore revision, then build with PlatformIO:

```sh
git apply /path/to/xd6500sht-rf-switch.patch
pio run -e ProMicro_companion_radio_ble
pio run -e ProMicro_repeater
```

Based on [MeshCore](https://github.com/meshcore-dev/MeshCore), licensed under [MIT](LICENSE).

Related Meshtastic firmware: [Meshtastic-XD6500SHT](https://github.com/VoxMesh/Meshtastic-XD6500SHT).
