# Resurrected Remix OTA repo
In order for a device to be officially supported by Resurrected Remix, OTA information needs to be added.
Please refer to the following "Readme" to get started

## 1. Introduction ##
In order for a device to be OTA compliant, there are a few things to know.

Each device has two files in this repo:
```
device/<device>.json      device fields and the latest release
changelog/<device>.txt    changelog
```

### 1.1 JSON structure ###
```
{
  "response": [
    {
      "maintainer": "maintainer",
      "oem": "manufacturer",
      "device": "device",
      "filename": "ResurrectedRemix-<date>-<build type>-<device>.zip",
      "download": "https://dl.resurrectedremix.com/<device>/ResurrectedRemix-<date>-<build type>-<device>/ResurrectedRemix-<date>-<build type>-<device>.zip",
      "timestamp": 0000000000,
      "md5": "abcdefg123456",
      "sha256": "abcdefg123456",
      "size": 123456789,
      "version": "",
      "buildtype": "OFFICIAL/UNOFFICIAL",
      "buildvariant": "vanilla/gapps",
      "os_sdk_level": 37,
      "os_patch_level": "YYYY-MM-DD",
      "changelog": [],
      "forum": "https://forum link",
      "gapps": "https://gapps link",
      "firmware": "https://firmware link",
      "modem": "https://modem link",
      "bootloader": "https://bootloader link",
      "recovery": "https://recovery link",
      "paypal": "https://donation link",
      "telegram": "https://telegram link",
      "dt": "https://github.com/<devices org>/device_<oem>_<device>",
      "common-dt": "https://github.com/<devices org>/device_<oem>_<SOC>-common",
      "kernel": "https://github.com/<devices org>/kernel_<oem>_<device/SOC>"
    }
  ]
}
```

The build writes `<device>.json` to the OUT dir with `createjson.py`. It fills in the file
fields, `os_sdk_level` (from `ro.build.version.sdk`), `os_patch_level` (from
`ro.build.version.security_patch`) and `changelog`, and copies the device fields (`oem`, the
links, `dt`, `common-dt`, `kernel`) from this repo's `device/<device>.json`. The updater
uses the levels to decide whether a build is installable, so do not edit them by hand.
Builds don't have version numbers, so `version` stays empty, but keep the key: Updaters on older
builds refuse a json without it.

### 1.2 changelog.txt structure ###
```
Device: Device name (<device codename>)
Device maintainer: Name (nickname)
Required firmware: (optional)

===== <date> =====
- change 1
- change 2
- change 3
```

## 2 Guidelines ##
* Check if manufacturer is already existing
* Check if published link is official
* Check if JSON is intact with help of online validator tools like [https://jsonformatter.curiousconcept.com](https://jsonformatter.curiousconcept.com) or [https://jsonformatter.org](https://jsonformatter.org)
* Check if no extra / missing spaces

## 3. How to ##
For following below description, replace *codename* with your device codename.
### 3.1 Initial support ###
1. Fork this repo to your own GitHub.
2. A file named *codename*.json is created in OUT dir after you built.
3. Copy it to the `device` folder where this repo was cloned.
4. Open the file and modify needed entries (see 1.1 for mandatory entries).
5. Create a file named *codename*.txt in the `changelog` folder based on changelog structure from point 1.2, and add your changelog in it.
6. Submit a pull request to this repo (this way we validate that you understood the requirements and if all is good you'll be granted direct push access to this repo)

### 3.2 Update build ###
The Updater reads `https://dl.resurrectedremix.com/ota/<codename>.json` and
`changelog_<codename>.txt` next to it, not this repo.
1. Make changes to *codename*.txt in the `changelog` folder.
2. Run the website's release script. It uploads the zip, its flash images and SHA256SUMS to
   `https://dl.resurrectedremix.com/<codename>/<build>/`, then the json and changelog to `ota/`,
   and commits *codename*.json to the `device` folder here.
```
python3 ~/Pictures/workspace/rr-website/tools/release.py <codename>
```
3. Push this repo.
