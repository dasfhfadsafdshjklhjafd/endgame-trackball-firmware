## Endgame Trackball (firmware)

### Prerequisites
1. Git
2. Zephyr SDK
3. West

### Building

#### Locally
1. `git submodule update --init`
2. `cd zmk/` 
3. `west init -l app`
4. `west update`

Now, staying in the `zmk` directory, build with west. Always point `ZMK_CONFIG` at the in-repo module (`../endgame-trackball-config`) and request a pristine build when changing those arguments so CMake reloads the config.

```shell
EXTRA_MODULES="$(pwd)/../endgame-trackball-config;$(pwd)/../zmk-pmw3610-driver;$(pwd)/../zmk-pointer-2s-mixer;$(pwd)/../zmk-axis-clamper;$(pwd)/../zmk-report-rate-limit;$(pwd)/../zmk-ec11-ish-driver;$(pwd)/../zmk-auto-hold"
west build -s app -d build/default -p -b efogtech_trackball_0 -S studio-rpc-usb-uart -S zmk-usb-logging -- -DZMK_EXTRA_MODULES="${EXTRA_MODULES}" -DZMK_CONFIG="$(pwd)/../endgame-trackball-config" -DCONFIG_ZMK_STUDIO=y
```

Single-sensor builds (loads `config/single_sensor.{overlay,conf}`) only need an extra `-DSHIELD` selection and a unique build directory:

```shell
west build -s app -d build/single_sensor -p -b efogtech_trackball_0 -S studio-rpc-usb-uart -S zmk-usb-logging -- -DZMK_EXTRA_MODULES="${EXTRA_MODULES}" -DZMK_CONFIG="$(pwd)/../endgame-trackball-config" -DSHIELD=single_sensor -DCONFIG_ZMK_STUDIO=y
```

Look for `build/<target>/zephyr/zmk.uf2` — it's your firmware.

#### GitHub Actions
Fork the `endgame-trackball-config` repository. Any commit will trigger an action that will build the firmware for you.

### Troubleshooting

1. Flash the [debug firmware](https://github.com/efogtech/endgame-trackball-config/tree/debug).
2. [Check out your logs](https://zmk.dev/docs/development/usb-logging#viewing-logs) via USB.

### Also see
1. Parent repo: https://github.com/efogtech/endgame-trackball
2. ZMK docs: https://zmk.dev/docs/
3. Zephyr 3.5 docs: https://docs.zephyrproject.org/3.5.0/
