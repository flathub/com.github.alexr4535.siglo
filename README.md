# siglo Flatpak

## Build

The following instructions describe how to build the siglo Flatpak.
These instructions assume that you have already installed Flatpak on your system.

Get the source code.

    git clone https://github.com/flathub/com.github.theironrobin.siglo.git
    cd com.github.theironrobin.siglo

Add the Flathub repository.

    flatpak remote-add --user --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo

Install Flatpak Builder.

    sudo apt install flatpak-builder

Build the Flatpak.

    flatpak-builder --user --install --install-deps-from=flathub --force-clean --repo=repo build-dir com.github.theironrobin.siglo.yaml

Run the Flatpak.

    flatpak run com.github.theironrobin.siglo

## Update

The Python dependencies for the Flatpak are generated with the help of the [Flatpak Pip Generator](https://github.com/flatpak/flatpak-builder-tools/tree/master/pip).
This tool produces `json` files for Python packages to be included in the Flatpak manifest's `modules` section.
In order to update or add dependencies in the Flatpak, these dependencies can be generated using the following instructions.

First, install the Python dependency `requirements-parser`.

    python3 -m pip install requirements-parser

Install the GNOME SDK.

    flatpak install --user flathub org.gnome.Sdk//40

Clone the `flatpak-builder-tools` repository.

    git clone https://github.com/flatpak/flatpak-builder-tools.git

Now run the Flatpak Pip Generator script for the packages `dbus-python`, `gatt`, `requests`, and `pyxdg`.

    python3 flatpak-builder-tools/pip/flatpak-pip-generator --runtime org.gnome.Sdk//40 dbus-python gatt requests pyxdg

If you have `org.gnome.Sdk//40` installed in *both* the user and system installations, the Flatpak Pip Generator will choke generating the manifest.
The best option at the moment is to temporarily remove either the user or the system installation until this issue is fixed upstream.

