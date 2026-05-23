# Reset OpenPGP Smartcard (FIDO2)

## ⚠ Important Notice

 This guide provides general instructions for resetting OpenPGP-compatible devices.

 - The reset process may vary depending on the device manufacturer
 - Not all devices support reset via software
 - Incorrect usage may temporarily or permanently render the device unusable

## ❗ Disclaimer

 - loss of cryptographic keys
 - loss of access to the device
 - misuse of commands

⚠ Use of untrusted or third-party software may compromise security.

Always prefer official or well-established tools.

## 🔐 When to perform a reset

 - Forgotten or blocked PIN
 - Corrupted or unusable key slots
 - Errors identified during GPG usage
 - Full reinitialization of the device


## 🧠 Check device status

 Before performing any reset, verify if the device is detected:

 ```bash
  gpg --card-status
 ```

![smartcard-active](../images/smartcard-active.png)

 If the device is not detected, refer to the troubleshooting section.

## 🔄 Reset via GPG (when supported)

 Some devices allow reset directly via terminal:

 ```bash
gpg --card-edit
 ```

Inside the interactive prompt:

 ```text
 admin
 factory-reset
 ```

⚠ Not all devices support this command.

## 🖥 Reset via GUI (Kleopatra)

The Kleopatra application can be used to:

 - manage keys
 - reset PINs
 - initialize the smartcard

 ⚠ Important:

 - Not all versions support full reset functionality
 - Interface behavior may vary depending on the system
 - Ensure the software is official and trustworthy

## 🧩 Manufacturer-specific reset

If the reset does not work using the methods above:

 - Check the official documentation of your device manufacturer
 - Some devices require:
 - proprietary software
 - physical interaction (button + USB connection)
 - specific initialization procedures



## 🔐 After reset

After resetting the device:

 - set a new PIN and Admin PIN
 - generate new cryptographic keys
 - do not reuse previously compromised keys

Verify device status:

``` bash
gpg --card-status
```

⚠ Security best practices

- Avoid unknown or unverified software
- Do not execute commands you do not understand
- Always verify the origin of binaries and tools
- Keep secure backups of your keys (if applicable)


## 📌 Final note

Resetting the device is a destructive operation.

Only proceed if you fully understand the consequences.

