# Roles

The server keeps one whitelist: `journal_roles.json` maps a record `kind` to the agents allowed to write it. There are six kinds, 60 distinct agents and 139 assignments. The file is read at start; `JOURNAL_ROLES` overrides its path. An agent outside the list gets `ValueError` naming the field.

One agent may hold several kinds. One rule holds everywhere: the agent that writes a verdict does not write `fix` or `done` for the same work.

## Kinds

| kind | who may write | agents |
|---|---|---|
| `verdict` | reviewers only | 17 |
| `finding` | researchers, scouts, critics | 15 |
| `fix` | makers | 21 |
| `done` | makers | 21 |
| `state` | every agent in the roster | 54 |
| `decision` | mode leads | 11 |

`fix` and `done` share the same list. `gate=1` or `gate=2` records never collapse into a duplicate, even inside the 900 s window.

## Passport roles per mode

A passport belongs to one mode. Writing (`passport_create`, `passport_set_section`, `passport_set_sections`, the export file) is limited to that mode's writer; gating (`passport_gate`) to that mode's lead. Reading a passport (`passport_get`, `passport_status`, `passport_diff`, `passport_list`, `passport_export` markdown) is open to any agent. Nine modes, 17 distinct roles (`passport_store.py:72-90`).

| mode | writer | lead |
|---|---|---|
| `mode-factory` | `modewriter` | `modeteamlead` |
| `appteam` | `appdesigner` | `appteamlead` |
| `siteteam` | `sitedesigner` | `siteteamlead` |
| `tenders` | `tenderwriter` | `tenderlead`, `tenderbid` |
| `hunt` | `hunter` | `huntlead` |
| `biteam` | `bitresearch` | `biteamlead` |
| `free` | `freedeveloper` | `freeteamlead` |
| `hy4` | `mimodeveloper` | `hy4teamlead` |
| `gitshow` | `modewriter` | `modeteamlead` |

A passport created before the `mode` column existed is treated as `mode-factory`.

## Agent to kind

| agent | verdict | finding | fix | done | state | decision |
|---|---|---|---|---|---|---|
| `appcodereviewer` | yes | · | · | · | yes | · |
| `appcopywriter` | · | · | yes | yes | yes | · |
| `appdesigner` | · | · | yes | yes | yes | · |
| `appdeveloper` | · | · | yes | yes | yes | · |
| `apporacle` | · | · | · | · | yes | · |
| `appresearcher` | · | yes | · | · | yes | · |
| `appreviewer` | yes | · | · | · | yes | · |
| `appscoutreviewer` | yes | · | · | · | yes | · |
| `appteamlead` | · | · | · | · | yes | yes |
| `bitai` | · | · | yes | yes | yes | · |
| `bitapp` | · | · | yes | yes | yes | · |
| `bitarch` | · | · | yes | yes | yes | · |
| `bitcritic` | yes | · | · | · | yes | · |
| `bitdesign` | · | · | yes | yes | yes | · |
| `biteamlead` | · | · | · | · | yes | yes |
| `bitengineer` | · | · | yes | yes | yes | · |
| `bitresearch` | · | yes | yes | yes | yes | · |
| `bitreview` | yes | · | · | · | yes | · |
| `bitverify` | yes | · | · | · | yes | · |
| `dokanalyst` | · | yes | · | · | · | · |
| `doklead` | yes | · | · | · | yes | yes |
| `dokreader` | · | yes | · | · | · | · |
| `dokverifier` | yes | · | · | · | · | · |
| `dokwriter` | · | · | yes | yes | · | · |
| `freedeveloper` | · | · | yes | yes | yes | · |
| `freereviewer` | yes | · | · | · | yes | · |
| `freeteamlead` | · | · | · | · | yes | yes |
| `gitshowlead` | · | · | · | · | yes | yes |
| `hunter` | · | yes | yes | yes | yes | · |
| `huntlead` | · | · | · | · | yes | yes |
| `huntreviewer` | yes | · | · | · | yes | · |
| `hunttriage` | · | yes | · | · | yes | · |
| `hy4teamlead` | · | · | · | · | yes | yes |
| `mimodeveloper` | · | · | yes | yes | yes | · |
| `mm3reviewer` | yes | · | · | · | yes | · |
| `modecritic` | yes | yes | · | · | yes | · |
| `moderesearcher` | · | yes | · | · | yes | · |
| `modeteamlead` | · | · | · | · | yes | yes |
| `modeverifier` | yes | · | · | · | yes | · |
| `modewriter` | · | · | yes | yes | yes | · |
| `readmewriter` | · | · | yes | yes | yes | · |
| `reporecon` | · | yes | · | · | yes | · |
| `showcasedesigner` | · | · | yes | yes | yes | · |
| `showcasereviewer` | yes | · | · | · | yes | · |
| `sitecopywriter` | · | · | yes | yes | yes | · |
| `sitedesigner` | · | · | yes | yes | yes | · |
| `sitedeveloper` | · | · | yes | yes | yes | · |
| `sitemodulescout` | · | yes | · | · | yes | · |
| `siteresearcher` | · | yes | · | · | yes | · |
| `sitereviewer` | yes | · | · | · | yes | · |
| `siteteamlead` | · | · | · | · | yes | yes |
| `specreviewer` | yes | · | · | · | yes | · |
| `tenderanalyst` | · | yes | · | · | · | · |
| `tenderassessor` | · | yes | · | · | yes | · |
| `tenderbid` | · | · | yes | yes | yes | yes |
| `tenderlead` | · | · | · | · | yes | yes |
| `tenderpricer` | · | yes | · | · | · | · |
| `tenderreviewer` | yes | · | · | · | yes | · |
| `tenderscout` | · | yes | · | · | yes | · |
| `tenderwriter` | · | · | yes | yes | yes | · |

## Document index roles

The index uses the same whitelist. `owner` in `doc_register`, `agent` in `doc_link` and `doc_set_status` must be a member of the matching kind. `review → approved` additionally requires an agent from the `verdict` list. If `journal_roles.json` cannot be read, role-bound calls are refused and the role-free calls (`doc_list`, `doc_status`, `doc_verify`) keep working.

Roles are declared by the caller. The server checks membership, not identity.
