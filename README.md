# gpu-check

Checks a newly bought NVIDIA GPU before you confirm the purchase:

1. Identity: model, VRAM size, board vendor, VBIOS, power limit
2. VRAM stress test with [memtest_vulkan](https://github.com/GpuZelenograd/memtest_vulkan) (5 minutes by default)
3. Temperature, power, clocks, PCIe link and hardware throttling logged every 5 s
4. Kernel errors (NVIDIA Xid, PCIe AER) during the test

Prints PASS/FAIL per card and saves a plain-text report, telemetry CSV and memtest log to `~/gpu-checks/`.

## Usage

```sh
gpu-check            # every NVIDIA GPU, one after another
gpu-check 0 2        # only these nvidia-smi indexes
GPU_CHECK_SECONDS=900 gpu-check
```

Environment: `GPU_CHECK_SECONDS` (minimum 300), `GPU_CHECK_EXPECTED_NAME` (default `RTX 5060 Ti`),
`GPU_CHECK_MINIMUM_MEMORY_MIB` (default `16000`), `NO_COLOR`.

A card fails if the name or VRAM doesn't match, memtest_vulkan reports errors, it goes above 85 C,
it throttles on hardware (slowdown, thermal, power brake), or Xid/AER errors appear.
If the card drives your display, it asks first, since the desktop will stutter during the test.

Requires: `nvidia-smi`, `memtest_vulkan`, `script` (util-linux), `lspci`, `journalctl`.
