# FIDO2 OpenPGP Smartcard Setup on Linux (Debian & Ubuntu)

This guide documents the full setup process for using a FIDO2 device as an OpenPGP smartcard on Linux (Debian/Ubuntu).
It includes dependency setup, smartcard detection, GPG configuration, and key creation.

## Requirements

- GnuPG installed
- pcscd service available
- FIDO2 device connected via USB

## Step 1 — Install base packages

```bash
sudo apt install gnupg pcscd pcsc-tools libccid
```

Step 2 — Enable smartcard service
```bash
sudo systemctl start pcscd
sudo systemctl enable pcscd
```

Step 3 — Verify USB device detection

Plug in your device and check if it is detected at the USB level:

```bash
lsusb
```
![lsusb-look-device-smartcard](../images/restart-services-smartcard.png)

Step 4 — Test smartcard detection

Plug in your device and run:

```bash
pcsc_scan
```

![pcscd service active](../images/pcscd_scan.png)

You should see the device being detected.

- If nothing appears, check:

- The device is connected

- pcscd is running

- The CCID driver is installed

Step 5 — Check CCID version (important)

Some Debian-based systems ship outdated CCID versions that may affect FIDO2 OpenPGP support.

Check installed version

```bash
dpkg -l | grep ccid
```
![search-new-version-libccid](../images/search-new-version-libccid.png)

Step 6 — Fix CCID version (if needed)

Update only the CCID package:

```bash
sudo apt install --only-upgrade libccid
```
![install-libccid-upgrade](../images/install-libccid-upgrade.png)

Reboot after installation:

```bash
sudo reboot
```

Step 7 — Confirm smartcard is working

After reboot:

```bash
pcsc_scan
```
Then:

```bash
gpg --card-status
```
![smartcard-active](../images/smartcard-active.png)

You should now see smartcard details instead of: "No smartcard available"

Step 8 — Initialize OpenPGP card

Using a GUI tool like Kleopatra:

- Set User PIN

- Set Admin PIN

- Set PUK


⚠ Write them down on paper. Do NOT store digitally.

Step 9 — Generate GPG keys on hardware

Using Kleopatra:

Create a new OpenPGP key directly on the smartcard


Verify:

```bash
gpg --card-status
```
![gpg_status-card-okay](../images/gpg-status-okay-gpg-card.png)

You should see fingerprints for:

- Signature

- Encryption

- Authentication

## Troubleshooting

GPG does not detect the smartcard

Restart services:

```bash
gpgconf --kill scdaemon
sudo systemctl restart pcscd
```
Then try again:

```bash
gpg --card-status
```

Device detected by lsusb but not by GPG

This usually indicates a communication issue between scdaemon and pcscd.

Fix:

```bash
gpgconf --kill scdaemon 
sudo systemctl restart pcscd
```
Final Result

✅ FIDO2 is fully working as an OpenPGP smartcard
✅ GPG can access keys stored in hardware
✅ Secure cryptographic operations enabled
