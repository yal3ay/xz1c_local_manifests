# XZ1C LineageOS 22.2 — local manifests

`repo` local manifests for building the unofficial LineageOS 22.2 (Android 15)
ROM for the Sony Xperia XZ1 Compact SO-02K (`lilac_dcm`).

The manifests pin the public source repositories used by the ROM:

- `lilac.xml` — device tree, shared Yoshino device tree, and kernel.
- `sony-camera.xml` — **placeholder only**; the Sony camera vendor tree is
  proprietary and is not published.
- `xz1c-timekeep.xml` — public LineageOS `hardware/sony/timekeep` project.

`vendor/sony/**` (Sony proprietary files and extracted blobs) is deliberately
not listed. Extract it locally from a device or firmware image that you legally
own.

## Usage

```bash
mkdir -p ~/lineageos && cd ~/lineageos
repo init -u https://github.com/LineageOS/android.git -b lineage-22.2
mkdir -p .repo/local_manifests
cp /path/to/xz1c_local_manifests/*.xml .repo/local_manifests/
repo sync -c -j8

source build/envsetup.sh
breakfast lilac_dcm

# Extract proprietary files from your own device/firmware.
cd device/sony/lilac
./extract-files.sh /path/to/dump
cd -

mka bacon
```

If `repo` complains about a missing `vendor/sony` project, run the extraction
step first. The device tree's `extract-files.sh` creates the vendor trees
locally; they are not part of this repository.

## Notes

- The manifests point at `yal3ay` repositories. If you fork them, change the
  project names and keep the paths.
- `revision="lineage-22.2"` matches the published branches of the device and
  kernel repositories.
- `repo sync --force-sync` may be needed after changing local manifests.
- No signing keys are included or referenced. Use your own keys.

## License

The manifest files and documentation are Apache-2.0; see `LICENSE`.

## 中文說明

這是非官方 LineageOS 22.2（Android 15）Sony Xperia XZ1 Compact 日版 SO-02K
（`lilac_dcm`）的 `repo` local manifests。

`lilac.xml` 釘住公開的裝置樹、共用 Yoshino 裝置樹與 kernel；`sony-camera.xml`
只有佔位說明，Sony 相機閉源樹不公開；`xz1c-timekeep.xml` 指向公開的
LineageOS timekeep 專案。`vendor/sony/**` 刻意不列為 project，請用裝置樹的
`extract-files.sh` 從你合法持有的裝置或固件自行提取，不要散布提取出來的檔案。

使用方式：`repo init` 後把三個 XML 放進 `.repo/local_manifests/`，再
`repo sync -c -j8`、`breakfast lilac_dcm`、提取 blob、`mka bacon`。本 repo
不含任何簽名金鑰。
