# Switch OC Suite

Overclocking suite for Switch **(Mariko Only)** running on Atmosphere CFW. Support Horizon OS 11.0.0-13.1.0.



## Notice

### Disclaimer

**Proceed with caution!**

**I AM NOT RESPONSIBLE (NOR IS ANYONE ELSE) FOR ANYTHING THAT MIGHT HAPPEN TO YOUR CONSOLE** (bans, internal component failure, etc.) by installing OC Suite or tinkering software/hardware with any info from this repo.



## Features

- **CPU/GPU/RAM Overclock** up to **2397.0/1344.0/2131.2 MHz**
- **Auto-Boost CPU for faster game loading**
- **Fan Control Optimization** at high load
- **Modded sys-clk and ReverseNX**(-Tools and -RT)
  - **No need to change clocks manually** after toggling modes in ReverseNX
  - Profile-aware clock override for all games
- System Settings
  - Disable background services, less heat and power consumption in standby mode
  - Game recording and SysDVR streaming @ 60fps with high video bitrate
  - Option to change the threshold for chargers providing enough power
- Remove copyright watermark in screenshots/recordings, courtesy of [HookedBehemoth](https://github.com/HookedBehemoth/exefs_patches)
- **TinyMemBenchNX**: DRAM throughput and latency test based on [tinymembench](https://github.com/ssvb/tinymembench)
- **MemTesterNX**: A userspace utility for testing memory faults and stability based on [memtester](https://pyropus.ca/software/memtester/)

#### Details

- **Overclock**
  - **Safe CPU/GPU OC clock: 1963.5/1267.2 MHz**.
    - Anything above that are not in the table of official module and are all wild guess.
    - Max clock(2397.0/1344.0 MHz) may not work on some SoCs.
  - **Recommended RAM clock: 1862.4 MHz**, **1795.2 MHz for Hynix** ones.
    - **RAM clock is set permanently** via **ptm-patch**, rather than sys-clk.
    - Use Hekate to check out the brand of your RAM chips.
    - Proper EM shielding for RAM chips and testing with emuNAND before long-term usage is preferred.
    - Once RAM overvolting is available on Mariko, we may gain more stability and reach higher clock.
  - Mariko variants have much lower power consumption compared to Erista, therefore **GPU clock capping is lifted for Mariko**.
  - For more info, see [README.md](https://github.com/KazushiMe/Switch-OC-Suite/tree/master/Source/sys-clk-OC) in sys-clk-OC.
- **Auto-Boost CPU for faster game loading**
    - When a game launches or is in loading screen, sys-clk will boost CPU to 1963.5 MHz for ~10 seconds or until the loading screen ends.
    - Some games don't utilize `SetCpuBoostMode` at all, e.g. Overcooked 2, so Auto-Boost will be unavailable to these games.
    - To **disable this feature**, simply remove `boost_start.flag` and `boost.flag` in `/config/sys-clk/ `.
- **Fan Control Optimization** at high load
  - Higher tolerable temperature and smoother fan curve. Set `holdable_tskin` to 56˚C. Previously it's set to 48˚C, so by default the fan would go crazy (80~100%) easily with a slight degree of OC.
  - Replace crappy factory thermal paste is preferred.
- **Modded sys-clk and ReverseNX**(-Tools and -RT)
  - **No need to change clocks manually** after toggling modes in ReverseNX    
    - Add `/config/sys-clk/downclock_dock.flag` to use handheld clocks in Docked mode when Handheld mode is set in ReverseNX.
    - To **disable this feature**, use original version of ReverseNX-RT and delete `/config/sys-clk/ReverseNX_sync.flag`.
  - Profile-aware clock override for all games
    - Add `[A111111111111111]` title config in `/config/sys-clk/config.ini` to set frequency override globally:
      ```ini
      [A111111111111111]
      docked_cpu=
      docked_gpu=
      handheld_charging_cpu=
      handheld_charging_gpu=
      handheld_charging_usb_cpu=
      handheld_charging_usb_gpu=
      handheld_charging_official_cpu=
      handheld_charging_official_gpu=
      handheld_cpu=
      handheld_gpu=
      ```
- Disable background services, less heat and power consumption in standby mode
  - **Remove** the "Disable Background service" part in `/atmosphere/config/system_settings.ini` if you **use Nintendo Online services**.
- Game recording and SysDVR streaming @ 60fps with high video bitrate (7.5Mbps)
  - (Recommended)[dvr-patches](https://github.com/exelix11/dvr-patches): Allow recording in any games.
  - For optimal streaming experience, SysDVR via USB interface is recommended.
  - Known Issues (won't fix)
    - Game recordings may be less than 30 seconds if higher bitrate is used.
    - It has noticeable performance impacts in demanding games.
    - Video duration shown in album will be twice than the actual value, while the playback speed is not affected.
  - To **disable** this feature, simply remove the `[am.debug]` section in `system_settings.ini`.
- Option to change the threshold for chargers providing enough power
    - Find the string `enough_power_threshold_mw` in `system_settings.ini`. The default value is `0x9858` (39,000 mW).
    - To lower the threshold, you may change the value to `0x4268` (17,000 mW). Now the system and "sys-clk" will see typical Power Delivery chargers that only supply up to 18W (9V/2A) as "Official Chargers".

## Installation

- Modded `loader.kip` with embedded pcv, ptm, am-no-copyright, ValidateAcidSignature patches
- Prebuilt sys-clk-OC and ReverseNX-RT modified for OC
- `system-settings.ini` with some QoL improvements

1. **Restoring pcv backup if you have patched pcv module manually:** Launch the `patcher.te` script via TegraExplorer to restore your backup. Ptm and am-no-copyright patches can be removed.

2. Copy all the files in `SdOut` to the root of SD card.

3. Grab `x.x.x_loader_xxxx.x.kip` for your Atmosphere version and desired RAM frequency, rename it to `loader.kip` and place it in `/atmosphere/kips/`.

4. **Hekate-ipl bootloader:**

   - Rename the kip to `loader.kip` and add `kip1=atmosphere/kips/loader.kip` in `bootloader/hekate_ipl.ini`

   **Atmosphere Fusee bootloader:**

   - Fusee will load any kips in `/atmosphere/kips/` automatically.



## Build

Grab necessary patches from the repo, then compile sys-clk, ReverseNX-RT and Atmosphere with devkitpro.



## Acknowledgement

- CTCaer for [Hekate-ipl](https://github.com/CTCaer/hekate) bootloader, RE and hardware research
- [devkitPro](https://devkitpro.org/) for All-In-One homebrew toolchains
- HookedBehemoth for am_no_copyright [patch](https://github.com/HookedBehemoth/exefs_patches)
- masagrator for [ReverseNX-RT](https://github.com/masagrator/ReverseNX-RT) and [BatteryChargeInfoNX](https://github.com/masagrator/BatteryChargeInfoNX)
- RetroNX team for [sys-clk](https://github.com/retronx-team/sys-clk)
- SciresM and Reswitched Team for the state-of-the-art [Atmosphere](https://github.com/Atmosphere-NX/Atmosphere) CFW of Switch
- suchmememanyskill for [TegraExplorer](https://github.com/suchmememanyskill/TegraExplorer) and [TegraScript](https://github.com/suchmememanyskill/TegraScript)
- Switchbrew [wiki](http://switchbrew.org/wiki/) for Switch in-depth info
- ZatchyCatGames for RE and original OC loader patches for Atmosphere
