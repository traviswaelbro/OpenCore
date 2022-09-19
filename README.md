# OpenCore

_Configuration & References for setting up a Hackintosh, mainly focused on boot configuration._


This is nearly all based on instructions provided by the [OpenCore
Install Guide (`dortania`)](https://dortania.github.io/OpenCore-Install-Guide/).

Additional references include:
- [almshary/Asus-Z170-A-Opencore 0.7.6](https://github.com/almshary/Asus-Z170-A-Opencore-0.7.6) - Hardware-specific configuration reference (same motherboard & CPU)
- [shiruken/hackintosh](https://github.com/shiruken/hackintosh) - General instructions & references

---

## Contents

* [Submodules](#submodules)
* [BIOS Configuration](#bios-configuration)
* [Drivers, Kexts, & SSDTs](#drivers-kexts--ssdts)
* [First-Time Repo Setup](#first-time-repo-setup)
    * [GenSMBIOS](#gensmbios)
    * [MountEFI](#mountefi)
    * [ProperTree](#propertree)
* [Managing the OpenCore Configuration File](#managing-the-opencore-configuration-file)
* [Graduating from USB to Native Boot](#graduating-from-usb-to-native-boot)
* [Dual Boot](#dual-boot)

## Submodules

- [OpenCorePkg](https://github.com/acidanthera/OpenCorePkg)
    - This is the OpenCore source repository, which contains all of the code and documentation needed to set up a Hackintosh.
- [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS)
    - This was suggested in the [Fixing iMessage and other services with OpenCore](https://dortania.github.io/OpenCore-Post-Install/universal/iservices.html) section; it is used to generate Serial, Board Serial, and SmUUID. See that page's _Using GenSMBIOS_ and _ Serial Number Validity_ for additional details.
- [MountEFI](https://github.com/corpnewt/MountEFI)
    - This was suggested in the [Mount your EFI](https://dortania.github.io/OpenCore-Post-Install/universal/update.html#_2-mount-your-efi) section; it is used to easily mount the EFI partition so that the OpenCore configuration can be managed.
- [ProperTree](https://github.com/corpnewt/ProperTree)
    - This was suggested in the [config.plist Setup](https://dortania.github.io/OpenCore-Install-Guide/config.plist) section; it is used to edit `.plist` files.

## BIOS Configuration

The BIOS recommendations from OpenCore and other sources were mostly accurate. The main differences in my current configuration **bolded** below.

_TODO: Build a more comprehensive list, including BIOS option paths._

| Name | Option |
| ---- | ------ |
| **Intel Virtual Technology (VT-x)** | **Enabled** |
| **VT-d** | **Enabled** |
| CFG Lock | Disabled |
| Fast Boot | Disabled |
| Secure Boot | Disabled |
| OS Type | Other OS |
| SW Guard Extensions (SGX) | Disabled |

## Drivers, Kexts, & SSDTs

These other directories were used to gather the appropriate files, as instructed per the documentation's [Gathering files](https://dortania.github.io/OpenCore-Install-Guide/ktext.html) section. These are the important configuration pieces for my particular setup. These are unlikely to be relevant for other hardware configurations; see the documentation for details.

These are found in the `EFI/` backup directory, `./EFI/OC/`. _Note: SSDTs are `.aml` files found under the `ACPI` subdirectory._

## First-Time Repo Setup

When cloning this repo, you need to first initialize the submodules:

```
git submodule update --init --recursive
```

### GenSMBIOS

You'll need to allow execution of the `GenSMBIOS.command` file before you can run it.

```
chmod +x ./GenSMBIOS/GenSMBIOS.command
```

### MountEFI

You'll need to allow execution of the `MountEFI.command` file before you can run it.

```
chmod +x ./MountEFI/MountEFI.command
```

### ProperTree

You can create a proper (dedicated) `/Applications` entry for `ProperTree` by running its `buildapp.command`. This creates the `ProperTree.app` file, which can then be manually moved to `/Applications`.

_You may need to check [the ProperTree FAQ](https://github.com/corpnewt/ProperTree/tree/94e5ed616abb5175dca1dc49bd2292e8d007fd4d#faq) for details on getting this working, depending on your macOS version._

```
./ProperTree/Scripts/buildapp-select.command
mv ./ProperTree/ProperTree.app /Applications/
```

## Managing the OpenCore Configuration File

1. Edit the backup file under this repo's `EFI/OC/config.plist` (using `ProperTree`)
    - Any other changes (such as additional Drivers, Kexts, etc.) should also be made directly to this repo's `EFI/` directory.
2. Once the local `EFI/` directory is ready to try out, mount the appropriate "real" `EFI` volume _(either the primary macOS boot drive or the bootable USB)_
    - `./MountEFI/MountEFI.command`, then complete the prompts
3. Copy the local `EFI/` directory to overwrite the "real" `EFI` volume's `EFI/` directory
    - This ensures that changes are made with version control first (to hopefully reduce the risk of FUBAR) and also incentivises it since we're regularly overwriting that "real" directory.
    - `cp ./EFI /Volumes/EFI/EFI`
4. Unmount the `EFI` volume, reboot, and cross your fingers

## Graduating from USB to Native Boot

Once you're able to successfully (and reliably) boot from your USB drive, you can just follow instructions from [Moving OpenCore from USB to macOS Drive](https://dortania.github.io/OpenCore-Post-Install/universal/oc2hdd.html) so that the USB drive is no longer the most valuable component in your computer...

## Dual Boot

If OpenCore isn't detecting your Windows boot drive/partition, you can reference [Dualbooting with Windows](https://dortania.github.io/OpenCore-Multiboot/oc/win.html#dualbooting-with-windows) for help.
