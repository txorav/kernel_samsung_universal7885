# SM-M205F (m20lte, universal7885) display / device layout

## Panel (physical, do not change)
- Resolution: 1080x2340 (19.5:9, Infinity-V TFT)
- Density: 420 dpi (`ro.sf.lcd_density=420`)
- Refresh: 60 Hz (HWC, `vsyncRate=60.00 Hz`, no VRR)
- Compositor: HWC2 (`android.hardware.graphics.composer@2.1`), 3 framebuffer surfaces

## Current GrapheneOS tuning (persists across reboots via settings)
- Render size: 720x1560 (`wm size 720x1560`)
- Density: 280 (`wm density 280`)
- Reason: Mali-T830 MP3 cannot hold 1080p Android 16 UI in 16.6 ms
  (29% janky @1080p -> 19% @720p, p50 40ms -> 22ms, measured via gfxinfo)
- Revert: `wm size reset; wm density reset`

## Pipeline notes
- GPU: ARM Mali-T830 MP3, DVFS 343-1300 MHz, governor Interactive
  (`/sys/devices/platform/11500000.mali/`), adaptive policy available
- Display: Samsung DECON (`14860000.decon_f`), DSIM, panel driver in
  `drivers/video/fbdev/exynos/dpu_7885/`
- CPU: 6x Cortex-A53 @1794 MHz max + 2x Cortex-A73 @2288 MHz max,
  governor interactive (GrapheneOS defaults: hispeed @85% load,
  target 85, 10 ms timer, 40 ms sample)
- RAM: 3 GB total, zRAM 50% (`fstab.enableswap`), swappiness 130

## SoC / board
- universal7904 (Exynos 7904), board `universal7904`, `m20lte`/`m20ltedd`
- Boot: `/dev/block/platform/13500000.dwmmc0/by-name/BOOT`
- SYSTEM partition: 3565158400 bytes (fits 3156676608-byte GSI image)
