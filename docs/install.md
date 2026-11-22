# Installing

HomeNodes needs **Home Assistant 2024.12 or later** and an administrator account.

## 1. Get the files

=== "Marketplace (2026.11 and later)"

    Home Assistant 2026.11 replaced HACS with the built-in Marketplace. Until HomeNodes is in its
    catalog, add it from a link:

    [![Open your Home Assistant instance and open a repository in the Marketplace.](https://my.home-assistant.io/badges/marketplace_repository.svg)](https://my.home-assistant.io/redirect/marketplace_repository/?owner=joerdav&repository=homenodes&category=integration)

    Or by hand:

    1. **Settings → Marketplace → Add from link**. The first time, the Marketplace asks you to
       accept its warning and to connect GitHub.
    2. Enter `joerdav/homenodes` and **Add**. If it asks for a type, choose **Integration**.
    3. Open **HomeNodes**, **Install**, and confirm.
    4. Restart Home Assistant.

=== "HACS (2026.10 and older)"

    Add it as a custom repository:

    1. **HACS → ⋮ → Custom repositories**.
    2. Repository `https://github.com/joerdav/homenodes`, type **Integration**.
    3. Find **HomeNodes** in HACS and download it.
    4. Restart Home Assistant.

=== "By hand"

    1. Download the latest release from GitHub.
    2. Copy its `custom_components/homenodes` folder into the `custom_components` folder next to
       your `configuration.yaml`, so you have `custom_components/homenodes/manifest.json`.
    3. Restart Home Assistant.

## 2. Add the integration

**Settings → Devices & services → Add integration → HomeNodes**, then **Submit**. Or:

[![Open your Home Assistant instance and start setting up a new integration.](https://my.home-assistant.io/badges/config_flow_start.svg)](https://my.home-assistant.io/redirect/config_flow_start/?domain=homenodes)

HomeNodes adds a **HomeNodes** panel to the sidebar. Only administrators see it. There is one
HomeNodes per Home Assistant; trying to add a second says it is already set up.

## 3. Open it

Open **HomeNodes** from the sidebar. The first load reads every automation and script, which
takes a moment on a large install; after that the map follows changes as they happen.

The map starts read-only. To run automations and switch things from it, turn on
[Allow controls](options.md#allow-controls).

## Renaming or hiding the panel

Edit the sidebar as you would for any panel: its name, icon and visibility are Home Assistant's
to change, not an option of HomeNodes.

## Updating

Update from the Marketplace (or HACS), or replace the folder by hand, then restart Home Assistant. Each release is
served from its own address, so browsers never mix files from two versions; a normal reload of
the page is enough.

## Removing

**Settings → Devices & services → HomeNodes → ⋮ → Delete** removes the panel. Then uninstall it in
the Marketplace (or HACS), or delete `custom_components/homenodes`, and restart.

Each person's layout is kept in their Home Assistant user data under the key `homenodes`, so it
is still there if you add HomeNodes back.
