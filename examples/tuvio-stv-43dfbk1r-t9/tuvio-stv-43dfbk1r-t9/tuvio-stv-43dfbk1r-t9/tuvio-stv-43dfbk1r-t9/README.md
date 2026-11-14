# TUVIO STV-43DFBK1R-T9 (Yandex TV / Яндекс ТВ, Android TV 11)

TUVIO `STV-43DFBK1R-T9` (`STV-43DFBK1R-T9.ST4251B052V`), MediaTek, Android TV 11 (SDK 30), build
`RP1A.200720.011` (dev-keys). Yandex TV platform with Yandex Home (`com.yandex.tv.home`), Wi-Fi, IR remote.

Tested on one set in a single session (no HDMI/AV devices connected). After a reboot and a full power-cycle,
Home stayed on `com.gathin.tvhome` and ADB reconnected on the same network address.

## Debloat

```bash
TV_HOST=<tv ip> tools/disable.sh examples/tuvio-stv-43dfbk1r-t9/debloat.txt
The debloat list disables 8 packages (Yandex telemetry/leftovers, Alice, Browser, Music, and Yandex Home last).
com.yandex.devices.appstore could not be disabled (protected package) and is left enabled; in this setup its
dock tile is repurposed to open Live TV. com.mediatek.androidbox (WFDSink/Miracast) and firmware updater/factory
packages were left untouched.
Launcher
com.gathin.tvhome was installed, set as Home, and the TV rebooted before disabling com.yandex.tv.home (last line
of the list). On this set, selecting HDMI/AV in the Inputs menu previously routed to Live TV/channels; the shipped
config.json works around this by:
- opening the Yandex source picker directly from the Inputs pill (input_action / source_panel_service =
com.yandex.tv.live/.input.SourceActivity);
- adding a «ТВ каналы» dock tile (renamed/redirected com.yandex.devices.appstore) that switches to the Efir/Live
TV input.
Physical HDMI/AV ports were not tested with connected devices after this change; verify with your own sources.
The config also includes the session's visual overlay (Sweden wallpapers, dock glass, Wi-Fi status colors). Dock apps
in config.json assume the listed apps are installed.
Re-enable ADB / debugging (if needed)
If ADB/debugging is turned off later and you need it back on this TV, one reported method is:
1. Connect a USB keyboard to the TV and switch to HDMI 1.
2. On the TV remote press Menu (three lines).
3. On the keyboard type 1147 to open the engineering/service menu.
4. Open Debug and enable ADB there. Do not change other settings in the engineering menu.
After enabling, reconnect with adb connect <tv ip>:5555 (or the port shown) and verify with tools/tv.py --probe.
Undo
TV_HOST=<tv ip> tools/restore.sh examples/tuvio-stv-43dfbk1r-t9/debloat.txt com.yandex.tv.home
