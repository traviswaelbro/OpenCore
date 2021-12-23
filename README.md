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
* [Drivers, Kexts, & SSDTs](#drivers-kexts-ssdts)
* [First-Time Repo Setup](#first-time-repo-setup)
    * [OpenCorePkg](#opencorepkg)
    * [GenSMBIOS](#gensmbios)
    * [MountEFI](#mountefi)
    * [ProperTree](#propertree)
* [BIOS Configuration](#bios-configuration)

## Submodules

- [OpenCorePkg](https://github.com/acidanthera/OpenCorePkg)
    - This is the OpenCore source repository, which contains all of the code and documentation needed to set up a Hackintosh.
- [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS)
    - This was suggested in the [Fixing iMessage and other services with OpenCore](https://dortania.github.io/OpenCore-Post-Install/universal/iservices.html) section; it is used to generate Serial, Board Serial, and SmUUID. See that page's _Using GenSMBIOS_ and _ Serial Number Validity_ for additional details.
- [MountEFI](https://github.com/corpnewt/MountEFI)
    - This was suggested in the [Mount your EFI](https://dortania.github.io/OpenCore-Post-Install/universal/update.html#_2-mount-your-efi) section; it is used to easily mount the EFI partition so that the OpenCore configuration can be managed.
- [ProperTree](https://github.com/corpnewt/ProperTree)
    - This was suggested in the [config.plis Setup](https://dortania.github.io/OpenCore-Install-Guide/config.plist) section; it is used to edit `.plist` files.

## Drivers, Kexts, & SSDTs

These other directories were used to gather the appropriate files, as instructed per the documentation's [Gathering files](https://dortania.github.io/OpenCore-Install-Guide/ktext.html) section. These are the important configuration pieces for my particular setup. These are unlikely to be relevant for other hardware configurations; see the documentation for details.

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
