<!--
SPDX-FileCopyrightText: 2026 Red Hat, Inc.
SPDX-License-Identifier: CC-BY-SA-4.0
-->

# Background Logo Extension

A GNOME Shell extension that overlays a logo on the desktop background. This is useful for adding branding to a background without restricting the artwork itself.

## Features

- **Logo overlay**: Display a logo image on top of the desktop background
- **Dark mode support**: Separate logo files for light and dark color schemes
- **Flexible positioning**: Place logo at any corner, center, or edge (top-left, top-center, top-right, center, bottom-left, bottom-center, bottom-right)
- **Configurable size**: Adjust logo size as percentage of screen width (5-15%)
- **Configurable border**: Add spacing around the logo (0-100 pixels)
- **Configurable opacity**: Control logo transparency (0-255)
- **Always visible option**: Show logo even on non-default backgrounds
- **Live preview**: Preferences dialog shows real-time preview of logo placement
- **Multi-monitor support**: Logo scales and positions per monitor

## Requirements

- **GNOME Shell**: 51.x (tested on 51.1)
- **Fedora**: 45 or later
- **GTK**: 4.x
- **GLib**: 2.76+
- **Fedora Logos**: `/usr/share/fedora-logos/` (default logo paths)

## Installation

### From Source (Development)

```bash
# Clone the repository
git clone https://github.com/psam21/background-logo-extension.git
cd background-logo-extension

# Build and install locally
meson setup builddir --prefix=$HOME/.local
meson install -C builddir

# Restart GNOME Shell (logout/login or Alt+F2 → r on X11)
# Then enable the extension:
gnome-extensions enable background-logo@fedorahosted.org
```

### System-wide Installation (Fedora)

```bash
# Build with system prefix
meson setup builddir --prefix=/usr
sudo meson install -C builddir

# Compile schemas
sudo glib-compile-schemas /usr/share/glib-2.0/schemas/
```

## Configuration

The extension can be configured via:

1. **GNOME Extensions App** (recommended) - Open "Extensions" app, find "Background Logo", click the gear icon
2. **Command line** using `gsettings`:

```bash
# Logo file (light mode)
gsettings set org.gnome.shell.extensions.background-logo logo-file '/path/to/logo.svg'

# Logo file (dark mode)
gsettings set org.gnome.shell.extensions.background-logo logo-file-dark '/path/to/dark-logo.svg'

# Position: center, bottom-left, bottom-center, bottom-right, top-left, top-center, top-right
gsettings set org.gnome.shell.extensions.background-logo logo-position 'bottom-right'

# Size as percentage of screen width (5.0-15.0)
gsettings set org.gnome.shell.extensions.background-logo logo-size 9.0

# Border in pixels (0-100)
gsettings set org.gnome.shell.extensions.background-logo logo-border 50

# Opacity (0-255)
gsettings set org.gnome.shell.extensions.background-logo logo-opacity 255

# Show on all backgrounds
gsettings set org.gnome.shell.extensions.background-logo logo-always-visible true
```

### Default Logo Paths

- Light: `/usr/share/fedora-logos/fedora_lightbackground.svg`
- Dark: `/usr/share/fedora-logos/fedora_darkbackground.svg`

## Architecture

The extension uses modern GNOME Shell APIs:

- **ES Modules** (`gi://`, `resource://` imports)
- **InjectionManager** for safe method overriding
- **Extension** base class from `resource:///org/gnome/shell/extensions/extension.js`
- **Clutter/St** for UI rendering
- **GSettings** for configuration persistence

Key components:
- `extension.js` - Main extension logic, background actor injection, logo rendering
- `prefs.js` - Preferences UI with live preview
- `schemas/org.gnome.shell.extensions.background-logo.gschema.xml` - GSettings schema

## Development

### Linting

```bash
cd tools
npm install
npm run lint
```

### Building

```bash
meson setup builddir
meson compile -C builddir
```

### Testing

Run the extension in a nested GNOME Shell session or use the GNOME Shell looking glass (Alt+F2 → lg) for debugging.

## Compatibility

| GNOME Shell | Fedora | Status |
|-------------|--------|--------|
| 51.x        | 45+    | ✅ Supported |
| 50.x        | 44     | Legacy (schema: org.fedorahosted.background-logo-extension) |
| 48.x        | 42-43  | Legacy |

**Note**: Version 51+ uses schema ID `org.gnome.shell.extensions.background-logo`. Older versions used `org.fedorahosted.background-logo-extension`. When upgrading from Fedora 44 to 45, the old schema must be removed and the extension reinstalled.

## Reporting Bugs

Bugs should be reported to the [issue tracker](https://github.com/psam21/background-logo-extension/issues).

For support questions, feedback on changes or general discussions, use the [#workstation matrix room](https://matrix.to/#/#workstation:fedoraproject.org).

## License

The extension is distributed under the terms of the GNU General Public License, version 2 or later. See the [COPYING](COPYING) file for details.

## Changelog

See [NEWS](NEWS) for detailed release history.

### Recent Changes (51.alpha)

- Renamed GSettings schema from `org.fedorahosted.background-logo-extension` to `org.gnome.shell.extensions.background-logo`
- Added `version` field to metadata.json for proper version tracking
- Updated for GNOME Shell 51.1 compatibility
- Modern ES module imports throughout

