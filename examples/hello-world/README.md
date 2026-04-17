# Hello World

Offline keyword spotting example for this ESP-IDF component.

The app loads a bundled offline audio sample, runs a single inference, logs basic model information, and prints the classifier output.

Note: Use the helper script at [tools/wavtoh.sh](../../tools/wavtoh.sh) to convert a WAV file into a C header for bundled sample audio.

## Build and run

Note: Default build (ie. when no PRJ_BUILD_TARGET is provided) is for hardware.

If `idf.py` is unavailable, source `~/.espressif/v5.5.4/esp-idf/export.sh` first.

### Wokwi

Build:

```bash
rm -rf sdkconfig build/ && PRJ_BUILD_TARGET=wokwi idf.py build
```

Run:

```bash
wokwi-cli . --timeout 120000 --fail-text "Backtrace:" --expect-text "main_task: Returned from app_main()"
```

### Hardware (ESP32-S3)

Build:

```bash
rm -rf sdkconfig build/ && PRJ_BUILD_TARGET=esp32s3 idf.py build
```

Run:

```bash
idf.py flash monitor
```

## Example output

For Wokwi:

``` text
<removed>
I (256) main_task: Started on CPU0
I (266) main_task: Calling app_main()
W (266) hello-world: Hardware acceleration is disabled for this build. This may cause the classifier to run significantly slower than expected.
I (306) hello-world: Model: Demo: Keyword Spotting
I (306) hello-world: Labels: 3
Timing: DSP 5911 ms, inference 301 ms, anomaly 0 ms, postprocessing 55 us
#Classification predictions:
  helloworld: 0.832031
  noise: 0.000000
  unknown: 0.167969
I (6526) main_task: Returned from app_main()


Expected text found: "main_task: Returned from app_main()"
TEST PASSED.
```

   
For hardware:

``` text
I (276) main_task: Started on CPU0
I (286) main_task: Calling app_main()
I (286) hello-world: Model: Demo: Keyword Spotting
I (286) hello-world: Labels: 3
Timing: DSP 116 ms, inference 5 ms, anomaly 0 ms, postprocessing 33 us
#Classification predictions:
  helloworld: 0.832031
  noise: 0.000000
  unknown: 0.167969
I (416) main_task: Returned from app_main()

Done
```