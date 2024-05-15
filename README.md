# Quicktest

**Quickly and automatically test systems inside Quickemu virtual machines**

Quicktest runs test cases automatically in [Qemu](https://www.qemu.org/) virtual machines using [Quickemu](https://github.com/quickemu-project/quickemu) by injecting keypresses and reading the screen with [Tesseract OCR](https://github.com/tesseract-ocr/tesseract).  

## Introduction

**Quicktest** is a companion for the excellent [Quickemu](https://github.com/quickemu-project/quickemu) that attempts to automate some testing usually performed by volunteers in their own time, before release.

However, it could be expanded beyond that initial remit. 

## Video Demo

Here's a ~6 minute video explaining Quicktest, with a (speeded up) demo.

[![Introduction to Quicktest video](https://img.youtube.com/vi/jAvEgBWedOM/0.jpg)](https://www.youtube.com/watch?v=jAvEgBWedOM)

Here's an asciinema of running a test case in quicktest.

[![quicktest demo](https://asciinema.org/a/V7bgjMrx0omiGzKgJOwIJFZqb.svg)](https://asciinema.org/a/V7bgjMrx0omiGzKgJOwIJFZqb)

This is the resulting video which is a timelapse of all the screenshots taken during the above session.

[![quicktest demo result](https://img.youtube.com/vi/sDBB7G9ht5E/0.jpg)](https://www.youtube.com/watch?v=sDBB7G9ht5E)


## Goals

At a high level, this is what Quicktest is all about:

- Providing a utility that enthusiasts can use to automate *some* OS tests in the Linux community
- Creating a set of tests that are understandable and maintainable
- Enabling technically savvy enthusiasts to develop and contribute new tests

## Quick Start

**Quicktest** requires [quickemu](https://github.com/quickemu-project/quickemu) (and quickget), `qemu`, `ffmpeg`, `socat`, `convert` (from imagemagick) and `tesseract`

e.g. `sudo apt install socat ffmpeg imagemagick tesseract`

1. Follow the installation instructions for [quickemu](https://github.com/quickemu-project/quickemu) 4.9.3 or newer
2. Install [tesseract-ocr](https://github.com/tesseract-ocr/tesseract) along with appropriate language packages, e.g. `tesseract-ocr-fra` for French.

e.g. `sudo apt install tesseract-ocr`

3. Clone this repo

`git clone https://github.com/quickemu-project/quicktest`

4. Run a test

*This will download an ISO image for Alpine Linux using `quickget`, start a VM using `quickemu` then run automated tests.*

Do not interact with the Qemu window as it may invalidate the test results.

```bash
cd quicktest
QUICKEMU_WIDTH=800 QUICKEMU_HEIGHT=600 ./quicktest test_boot_to_login alpine v3.19
```

There are many options, which can be overridden at launch.

```bash
DEBUG=true \
QT_KEEP_SCREENSHOTS=false \
QT_KEEP_TESSERACT_TEXT=false \
QT_CREATE_TIMELAPSE=true \
QUICKEMU_WIDTH=800 \
QUICKEMU_HEIGHT=600 \
./quicktest test_boot_to_login alpine v3.19
```

Results will be found in `./results/`.

```bash
tree results/
results/
└── alpine
    └── v3.19
        └── test_boot_to_login
            └── 20240513-133341
                ├── quicktest.log
                └── quicktest.mp4
```

### Further tests

There are other tests that ship with Quicktest. List them all with `quicktest -ls`. We are keen to add to the selection of tests. Please consider writing and maintaining new tests. See [WRITING_TESTS.md](./WRITING_TESTS.md) for some tips.

Ubuntu is the default OS when launching `quicktest` and `24.04` is the default release.

`./quicktest test_install_entire_disk_with_defaults`

To do a similar test with Ubuntu MATE 24.04:

For example, to do an install test of Ubuntu MATE 24.04:

`./quicktest test_install_entire_disk_with_defaults ubuntu-mate 24.04`

## Philosophy

**Quicktest** aims to treat the machine being tested as a user would. That is, it
injects keypresses and mouse clicks as a human tester would, and inspects the screen 
as a human subject would.

The aim is (where possible) to do everything necessary from 'outside' the virtual machine.
That is, not to install software to operate the test machine inside the guest, but start 
the machine and use the keyboard and mouse only.

This is to ensure the guest OS is not tainted which could change the outcome of the test.

Enthusiastic users should be able to both run and contribute to tests. New tests are welcome
as pull requests. 

## Important Notes

**Quicktest** main goal is to automate *some* of the testing required by Linux distro
maintainers and release managers. It is not designed, nor should it be perceived to 
replace human testing. This is a convenience script.

**Quicktest** is very new and not widely tested yet. As such the features, names of
options, file locations and behaviours may change before the first stable release.

## MVP

The Minimum Viable Product (MVP) should be able to successfully run a test or two.
The results should be human-understandable. The test case should be usable as a
guide for further test cases, so will likely contain many of the common attributes
of a typical test case.

As such things may be a bit messy, and unfinished. This is to be expected, as this was
only started recently, and has had less modest development work put into it. Feedback
is welcome. but please be kind.

- [X] Interpret command line options
- [X] Display command line help
- [X] Display version string
- [X] Display available tests
- [X] Prepare and launch the required VM using quickget/quickemu
- [X] Send all valid keypresses into the VM
- [X] Interpret test script, to perform an installation of Ubuntu
- [X] Generate log file
- [x] Take screenshots
- [X] Grep for text in screenshots
- [X] Add option to wait, and take periodic screenshots - basically already there, just make numbers bigger
- [X] Fix send_string upper case (likely related to next issue) (it's not)
- [X] Debug and fix why some characters don't get sent to qemu (! and .)
- [X] Eject medium and reboot at end of installation in `install_entire_disk_with_defaults`
- [X] Add post-install reboot, and start the machine up - after reboot, login, first run wizard
- [X] Test the GTK backend for Qemu rather than Spice - sadly same annoying dialog
- [X] Complete one full test case - default install process
- [X] Keep all screenshots and texts
- [X] Make all log output prefixed with emoji
- [X] Truncate all informational text to 80 cols
- [X] Try scaling screenshots 300% to improve results
- [X] Optimize getting words from screenshots to reduce the number of shots taken. e.g. title and body
- [X] Add a variant test that disables network
- [X] Add a test which does an install in a different language
- [ ] Debug fr_FR switch in `QT_TEST_LANG=fr_FR ./quicktest test_install_entire_disk_with_defaults` - relative path on load
- [X] Work out why the i18n file won't load
- [X] Add a very small distro as a ci test (alpine, test boot to grub only and power off)
- [X] Figure out how to deal with tests written in other languages than en_GB
- [X] Get QT_SCALE_IMAGE working
- - [ ] If using spice display, have an option to poke the annoying spice dialog, off by default
- [X] Write a test for Ubuntu daily live
- [X] Stitch together the screenshot_*.ppm files with 5 seconds wait on each frame as a video
- [X] Maybe move VMs and ISOs elsewhere in a subdirectory like vm/
- [X] Give screenshots meaningful names
- [X] Break up test into named steps (usable in screenshot names)
- [X] Consider shipping one big shell script, only keepin the tests and texts separate
- [X] Figure out where to put lang texts and what to call them
- [X] Allow configuration overrides in the environment
- [X] Create quickcapture, which is a cut-down quicktest that only takes periodic screenshots and tesseracts them
- [X] Figure out optimal tesseract settings
- [X] Tidy up -ls output
- [X] Force resolution of qemu window
- [X] Thoughtfully consider config option naming
- [X] Perhaps check the disk to see if it has already been written to
- [X] Enable use of --status-quo to throw away disk commits (works via QUICKEMU_OPTS=--status-quo)
- [X] Enable daily testing of a flagship distro a possibility

## TO-DO

### High

- [ ] Have default retry and interval settings for qt_wait_for_text - figure out how to pass function name?
- [ ] Fix French keymap - if it can be - as an example for non-en_US testing
- [ ] Add a non-Linux test as an example - e.g. CalibreOS
- [ ] Add a test case that uses an encrypted disk - reboot and test it encrypted it
- [ ] Figure out where buttons are on screen so maybe we can mouse click them
- [ ] Test mouse click top right, down arrow, right right, fire (maybe use --width --height to get mouse top right corner)

### Medium

- [ ] Detect language rather than hard wire it?
- [ ] Add a function to hold a key down for a period rather than tap it
- [ ] If using spice display, have an option to poke the annoying spice dialog, off by default

### Low

- [ ] Use escalating delay times rather than screenshotting every n seconds to test a condition
- [ ] Allow send_key_combo to take a number so we can do tabx2
- [ ] Offer a snapshot once complete
- [ ] Enable starting from a snapshot
- [ ] Optional notifications at each step
- [ ] Ensure quicktest works in WSL on Windows, so people can run tests outside of Ubuntu

### Unprioritised

- [ ] Send mouse events into the VM
- [ ] Create quicktestrecorder to generate test files
- [ ] Write test for Haiku (haiku r1beta3 x86_64)

## Open Questions

- How do we insert characters such as CJK characters into Qemu?
- How should this be packaged? (currently, clone the repo and run from that folder)
- Do we dynamically adjust delay timings based on detected disk/cpu speed of machine?
- Should we snapshot of the installation prior to starting at end?
- Should we allow skipping tests with a --start-on to make it skip tests you already did?
- Should we train Tesseract on the Ubuntu font?

## Other uses of quicktest

- Automated application browser/website testing
- Automate complex installation processes (e.g. macos in quickemu)
- Add automation to OS installs that don't have automation as a convenience

## Limitations

- Tesseract has trouble identifying text above 2560x1600.
- Qemu doesn't present a screen size below 30" so at high resolutions, hidpi is not enabled in desktops

## EDID notes

- 2560x1600, xrandr reports a 30.2 inch monitor with edid unspecified, or edid=on specified.
