# Building and testing

Building the firmware, the tests that need no hardware, and the rig. Back to the [README](../README.md).

The rig's host is a Feather RP2040 with an Adafruit Floppy FeatherWing, running Adafruit's Greaseweazle-compatible firmware, so the Greaseweazle tools (`gw`) drive it from the Mac. The firmware lives in [firmware/](../firmware), and every command below runs from there; the board designs are in [hardware/](../hardware/README.md).

```sh
cd firmware
test/run_native_tests.sh                 # C++ unit tests (no hardware)
brew install mtools dosfstools           # for the DOS disk tests
cd host && python3 -m venv .venv && .venv/bin/pip install -r requirements.txt
.venv/bin/pytest                         # simulator and parser tests (no hardware)

pio run -e bb48_drive -t upload          # build and flash the drive
pio run -t upload                        # or the listen-only build
pio device monitor                       # h help, p pin levels, s stats

scripts/rig.sh -m slice0                 # everything, against the real host
```

Read the power-order rule in [wiring.md](wiring.md) before connecting a host. The firmware files keep the project's old name (`fddsynthpro-*.uf2`), and so does the board's USB name.

## Where to look next

| Page | What it covers |
|---|---|
| [plan.md](plan.md) | The slices the drive was built in |
| [wiring.md](wiring.md), [wiring.html](wiring.html) | The connections, with an interactive diagram and a soldering checklist |
| [bringup.md](bringup.md) | The checklist for hardware day |
| [parts.md](parts.md) | The parts list |
| [hardware-tests.md](hardware-tests.md) | What the tests cover, on the rig and in simulation |
| [recording.md](recording.md) | Recording a real drive for a sample pack |
| [CLAUDE.md](../firmware/CLAUDE.md) | The rules that keep the cable safe, and the project's conventions |
