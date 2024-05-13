# Writing Tests

We would love to have more tests shipped with Quicktest. Please consider writing new tests. It's fun, and helps contribute to the project in a meaningful way.

## Learn Quicktest

Take some time to run a few existing tests, and read the test cases in the `testcases` and texts in the `testcases/os/release/i18n` folder.

## Short cuts

First, before attempting to make a new test, try running a few of the existing ones.

## Copy an existing test

If you're writing a test for an OS or release that isn't already represented, then it's worth copying existing tests and modifying them if you can.

Copy any of the shipped tests to a different release or flavour of the same OS, or to a different OS entirely.

For example, the `test_install_entire_disk_with_defaults` test for Ubuntu MATE 24.04 is not very different from the test of the same name for Ubuntu 24.04. Only a few words on the various dialogs differ.

## Build a test from scratch

Starting from scratch isn't hard, especially for a short test. It's all just bash shell scripts, after all. Perhaps take a look at the very simple Alpine v3.11 test `test_boot_to_login`.

### Create a keymap

If a keymap for your local country and language doesn't exist, then create one in the `testcases/keymaps` folder. Copy an existing one and adjust for your keyboard layout. This is only required to ensure `quicktest` sends the correct key-presses to the Qemu monitor when running tests.

### Capture screenshots and text

Copy `test_no_test_gather_screenshots_and_text_only` to your `testcases/os/release` folder.

Run the test, which will launch your OS and be ready to start taking screenshots and OCRing them. Use the `TESSERACT_LANG` environment variable to determine which languages files `tesseract` will load. You may need to install these separately. 

For example:

```bash
sudo apt update ; sudo apt install tesseract-ocr-fra
TESSERACT_LANG=fra ./quicktest test_no_test_gather_screenshots_and_text_only
```

Go through a normal install, remembering to press `[RETURN]` in the `quicktest` instance that launched the VM.

The results folder will have a sequence of `screenshot_nnnn_no_test.ppm` images and their respective OCR text files in the same folder. These can be used to figure out what Tesseract can 'see' on the screen.
