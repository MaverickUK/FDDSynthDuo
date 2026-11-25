# Status and testing

What the drive does, the machines it has run on, and what's next. Back to the [README](../README.md).

## What works

The drive build (`pio run -e bb48_drive`) is the FDD Synth Duo, on an Olimex RP2350-PICO2-BB48R on the rev B carrier board ([hardware](../hardware/README.md)):

- it tracks the head, and drives TRK00, a 300 or 360 RPM index, write protect and disk change (or READY, for Shugart hosts);
- it streams an image from the SD card on RDATA, or, without one, a welcome disk whose `README.TXT` explains how to set the drive up: PC disks from 160 KB to 1.68 MB (5.25- and 3.5-inch) by their size, and other machines' disks, MFM or FM (Roland, Ensoniq, Kaypro, the Osborne 1, 8-inch CP/M and the like), from GoTek's `IMG.CFG`;
- it serves HFE flux images too: tracks as recorded, so copy-protected disks keep their bad CRCs, long tracks and weak bits;
- it takes writes back into the image, from single sectors to whole tracks, and DOS FORMAT;
- it swaps between the images on the card, GoTek style, whatever their formats;
- it answers as drive A or B, past a twist or on a straight cable, or as Shugart DS0–DS2, or as two drives at once, each with its own disk ([config.md](config.md));
- on an Amiga it runs the bus at hardware speed in PIO, since an A1200 selects its drive for only a few microseconds at a time;
- it reports what it does as sound events (motor, clicks, seeks, bumps against the end stops, disk changes), and plays them with a sample pack through an I2S amp ([activity.md](activity.md)); `samples/Recorded/` is a pack cut from a recording of a real drive ([recording.md](recording.md)), `samples/Placeholder/` a synthesised stand-in;
- [SynthMinder](https://github.com/MaverickUK/SynthMinder) can find it from DOS and change its disk, pack, volume and settings, through a mailbox at cylinder 82 ([synthminder.md](synthminder.md); in development).

The listen build (`pio run`) records every drive select, motor, side, density and write-gate change and every head step it sees, without driving any pin, and streams the trace over USB serial.

## Machines

| Machine | What ran | Result |
|---|---|---|
| Evesham, Athlon XP 2000+ (PC floppy controller) | DOS: reads, writes, copies and FORMAT; SynthMinder's exchanges with a disk in | Works |
| Amiga 1200 (Kickstart 3.0) | Workbench from the disk; Robocod and Turrican boot | Works, one drive (`drive=df0` or `df1`) |
| Feather RP2040 with a Floppy FeatherWing (Greaseweazle tools) | The rig's tests, from the Mac | Works |

Its logic is also tested against virtual hosts (a PC's BIOS and controller, an Amiga, the Feather and a Greaseweazle), and its PIO programs in a simulator whose output Greaseweazle's own decoder reads back ([hardware-tests.md](hardware-tests.md)).

## Next

- **Rev C**, the integrated board: the RP2350B, PSRAM, microSD, the amp, power from the floppy connector or USB-C, and the disk-change line wired for the Amiga ([pcb-revc.md](pcb-revc.md)).
- **SynthMinder with the drive empty:** the BIOS won't reach cylinder 82 without a disk in, so SynthMinder will talk to the floppy controller directly.

The plan, in slices: [plan.md](plan.md). Every change: [CHANGELOG.md](../CHANGELOG.md).
