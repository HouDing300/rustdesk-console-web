# Custom changes (fork of databk/rustdesk-console-web)

Branch: `feat/device-last-ip-and-account`
Upstream: https://github.com/databk/rustdesk-console-web

Companion to the backend fork `HouDing300/rustdesk-console`; both branches must
be deployed together.

## Feature

The device table (device management and the device select table) now shows
three separate columns instead of the combined `username@device_name` cell:

- Device Name — `info.device_name`
- Login Account — `info.username`
- Last IP — `info.ip`

All three are searchable, and the device list search form renders expanded
(`defaultCollapsed: false`) instead of collapsed behind "Expand".

## Changed files

| File | Change |
| --- | --- |
| `src/components/DeviceSelectTable/columns.tsx` | splits the combined Device cell into Device Name / Login Account / Last IP columns, each with its own search field |
| `src/components/DeviceSelectTable/index.tsx` | forwards `device_name` / `device_username` / `ip` to `/api/peers` |
| `src/pages/devices/index.tsx` | `search={{ defaultCollapsed: false }}`, forwards the new params, adds the three values to the CSV export |
| `src/services/rustdesk-console/typings.d.ts` | `ip` (and the device fields) on `DeviceListParams` / `AdminDeviceListParams` |
| `src/locales/zh-CN/pages.ts` | new `pages.devices.*` labels |
| `src/locales/en-US/pages.ts` | new `pages.devices.*` labels |

## Syncing with upstream without losing these changes

```bash
git remote add upstream https://github.com/databk/rustdesk-console-web.git
git fetch upstream

git switch main
git merge --ff-only upstream/main      # or: gh repo sync
git push origin main

git switch feat/device-last-ip-and-account
git rebase main
git push --force-with-lease origin feat/device-last-ip-and-account
```

Keep `main` as a pure upstream mirror and your work on the feature branch.
