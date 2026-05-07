# Changelog

## v1.1.7 — 2026-05-07

Stability + Linux v4l2 backend release. End-to-end RGBD validated on UTM
VM (Humble + ros2 humble + vxl_camera_node) and bench-tested with the
SDK reference programs.

### Fixed

- **`realpath()` target buffers sized to `PATH_MAX`** (was 1024 in 3
  places under `src/backend/`). glibc's `_FORTIFY_SOURCE=2` generates a
  `__realpath_chk` call that aborts at runtime with
  `*** buffer overflow detected ***` when the destination is smaller —
  even when the actual resolved path fits. Symptom: every Release build
  binary linking the SDK (test_vxl_context, device_info, rgbd_fusion,
  any vxlros2 node) aborted on `vxl_context_create()`. Workaround was
  to compile with `-D_FORTIFY_SOURCE=0`; no longer needed.
- **VXL6X5 v4l2 backend: Y16 profile mapped to `Z16`** for the DEPTH
  sensor (parity with libuvc backend; consumers expecting Z16 used to
  get Y16 silently and decode wrong).
- **Pipeline.start streams in `enableStream` order** (was iterating the
  internal `std::map` which gave COLOR-first ordering by enum value).
  On VXL6X5 the firmware needs the Y16 (depth/IR) stream open before
  MJPEG bulk transfers will deliver frames; the wrong order left the
  Color stream stuck at 0 frames in dual-stream mode.
- **Auto-set `NIR_DEPTH` XU on RGB stream open path** in the v4l2
  backend (mode required by VXL6X5 firmware for MJPEG bulk to flow).

### Added

- **Linux v4l2 backend** for VXL6X5 (`-DVXL6X5_BACKEND=v4l2`, opt-in,
  default still libuvc). Useful where libuvc dual-stream is unstable —
  notably USB-passthrough VMs. Single-binary supports both backends at
  build time.

### Notes

- This release ships Linux x86_64 only. macOS / Windows builds still
  use the v1.1.6 lib and continue to work; rerun
  `package_sdk.sh` on each platform to regenerate.
- See git log between v1.1.6..v1.1.7 for full commit history.
