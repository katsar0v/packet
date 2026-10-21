# <img src="data/icons/io.github.nozwock.Packet.svg" /> Packet

A partial implementation of Google's Quick Share protocol that lets you send and receive files wirelessly from Android devices using Quick Share, or another device with Packet.

> [!NOTE]
> **This fork (`hotspot-ble` branch) also works when the PC is connected to the phone's own Wi-Fi hotspot** ([nozwock/packet#10](https://github.com/nozwock/packet/issues/10)).
> On its own hotspot the phone never looks for Quick Share devices over mDNS, so upstream Packet never shows up.
> This fork additionally advertises Packet as a Quick Share receiver over Bluetooth LE: the phone finds and connects to it over BLE, then the transfer is upgraded to Wi-Fi over the hotspot, so file speed is unchanged.
> The protocol work lives in [katsar0v/rquickshare@hotspot-ble](https://github.com/katsar0v/rquickshare/tree/hotspot-ble), based on Martin Alderson's [BLE receiver prototype](https://github.com/martinalderson/rquickshare/blob/feat/ble-receiver-connect-back/docs/BLE_RECEIVER_DISCOVERY.md).
> Tested with an OPPO Find X9 Pro (Android 16) as the hotspot; the consent prompt appears within about a second of tapping the PC.
>
> Build and install it next to the Flathub app (as `io.github.nozwock.Packet.Devel`); quit the regular Packet first so only one receiver runs:
> ```sh
> flatpak-builder --user --install --install-deps-from=flathub --force-clean _build build-aux/io.github.nozwock.Packet.Devel.json
> ```

<div align="center">
    <img src="data/resources/screenshots/packet-receive.png" alt="screenshot" />
</div>

## Installation

[![flathub-installs-badge]][flathub]

<a href="https://flathub.org/apps/details/io.github.nozwock.Packet">
<img src="https://flathub.org/api/badge?svg&locale=en&dark" width="190px" />
</a>

#### Nightly
Nightly Flatpak builds are available from [here][nightly-build].

---

Third-party packages are also available on the [AUR] and [nixpkgs].

## Requirements
Since only the Wi-Fi LAN medium is implemented, Packet requires Bluetooth to be enabled and the devices to be connected to a Wi-Fi network with mDNS.

## Translations

<!--
If you'd like to help translate Packet to your native language, you can do so using the [Weblate][translation-platform] platform.

[![Translation status][translation-status-widget]][translation-platform]
-->

Weblate is no longer available as the project was permanently deleted (nozwock/packet#116), but you can still contribute translations locally:

1. Edit an existing translation in [`po/`](po/) (sync it with [`po/packet.pot`](po/packet.pot) using your editor's "Update from POT" feature if needed), or create a new one from the template using a gettext PO editor such as [Poedit] or [Gtranslator].
2. Submit your changes by opening a Pull Request, or attach the `.po` file in a new issue and it will be committed on your behalf.

## FAQ

#### Can't send to app from other devices

Your firewall may be blocking Packet's port. Enable *Static Port* in Preferences and allow it through the firewall. See issue [#35](https://github.com/nozwock/packet/issues/35).

#### "Couldn't open files"

This error occurs if the file is invalid (e.g. empty) or, on Flatpak, if you're sending files (via the Nautilus plugin) from a location Packet can't access.

Consider granting "All system files" access via Flatseal to allow sending from any location. See [Downloads folder keeps resetting](#downloads-folder-keeps-resetting) for details.

#### Downloads folder keeps resetting

In Flatpak, folder access is temporary and resets after a session restart because static access can't be requested. To set a permanent downloads folder, grant access in advance using Flatseal or run:

```sh
flatpak override --user io.github.nozwock.Packet --filesystem='/path/to/your/folder/here'
```

## Plugin Requirements

<!-- Don't change the heading since a link to it is being used in the app. -->

To use the Nautilus plugin, install the required packages:

- Ubuntu/Debian:\
`sudo apt install python3-dbus python3-nautilus`
- Fedora:\
`sudo dnf install python3-dbus nautilus-python`
- Arch:\
`sudo pacman -S python-dbus nautilus-python`
- Fedora Silverblue (rpm-ostree):\
`rpm-ostree install python3-dbus nautilus-python`

## Building

See [CONTRIBUTING.md](CONTRIBUTING.md) for instructions on how to build, and run the project.

## D-Bus API

Packet exposes a session D-Bus interface for external integrations (e.g. desktop extensions, scripts), currently providing read-only configured device name and read/write device visibility:

- **Service:** `io.github.nozwock.Packet.Api`
- **Object Path:** `/io/github/nozwock/Packet`
- **Interface:** `io.github.nozwock.Packet1`

The interface can be introspected from a running instance of Packet using tools like [D-spy].

## Acknowledgments
- [Dominik Baran][dominik] for creating the icon and working on the app's design.
- [NearDrop][neardrop] for reverse-engineering the closed-source Quick Share implementation in Android's GMS.
- [rquickshare] for their internal Rust implementation of the Quick Share protocol.

## Code of Conduct
Packet follows the [GNOME Code of Conduct][gnome-coc].

[d-spy]: https://flathub.org/en/apps/org.gnome.dspy
[poedit]: https://poedit.net/
[gtranslator]: https://wiki.gnome.org/Apps/Gtranslator
[nightly-build]: https://nightly.link/nozwock/packet/workflows/ci/main?preview
[translation-platform]: https://hosted.weblate.org/engage/packet/
[translation-status-widget]: https://hosted.weblate.org/widget/packet/multi-auto.svg
[dominik]: https://gitlab.gnome.org/wallaby
[neardrop]: https://github.com/grishka/NearDrop/
[rquickshare]: https://github.com/Martichou/rquickshare/
[nixpkgs]: https://search.nixos.org/packages?query=packet#show=packet
[aur]: https://aur.archlinux.org/packages/packet
[flathub]: https://flathub.org/apps/details/io.github.nozwock.Packet
[flathub-installs-badge]: https://img.shields.io/badge/dynamic/json?label=Installs&url=https%3A%2F%2Fflathub.org%2Fapi%2Fv2%2Fstats%2Fio.github.nozwock.Packet&query=%24.installs_total&logo=flathub&color=007ec6
[gnome-coc]: https://conduct.gnome.org/
