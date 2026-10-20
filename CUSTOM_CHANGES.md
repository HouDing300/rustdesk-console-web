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

## HTTP method override (workaround for restrictive networks)

Some corporate networks only allow `GET` / `POST` on the path between browser
and console: a firewall / web-filter / WAF resets `PUT` and `DELETE`
connections (`net::ERR_CONNECTION_RESET` in DevTools, nothing in the nginx
access log because the request never arrives). `PATCH` usually passes.

Every write action in this app (role assignment, role/user/device/strategy
editing and deletion, saving settings, ...) is one of those methods, so the
whole console appeared broken while `GET` / `POST` worked.

Workaround, in two halves that **must be deployed together**:

1. **Frontend** — `src/requestErrorConfig.ts`: a request interceptor rewrites
   `PUT` / `PATCH` / `DELETE` into `POST` and adds
   `X-HTTP-Method-Override: <original method>`.
   All 54 write call sites go through `@umijs/max`'s `request` (including the
   `actionRequest` and `webClientRequest` wrappers), so one interceptor covers
   everything.
2. **nginx** — `deploy/nginx/00-method-override.conf`: a `map` plus
   `proxy_method` that restores the real method before proxying to the backend.

Without the nginx half, write requests arrive at the backend as `POST` and
return 404. See `deploy/nginx/README-method-override.md` for the exact snippet,
install steps and a verification `curl`.

| File | Change |
| --- | --- |
| `src/requestErrorConfig.ts` | request interceptor: `PUT`/`PATCH`/`DELETE` → `POST` + `X-HTTP-Method-Override` |
| `deploy/nginx/00-method-override.conf` | nginx `map` used to restore the real method |
| `deploy/nginx/README-method-override.md` | root cause, install steps, verification command |

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
If upstream ever touches `src/requestErrorConfig.ts`, re-apply the interceptor
after the rebase — the file is small and the change is additive.
