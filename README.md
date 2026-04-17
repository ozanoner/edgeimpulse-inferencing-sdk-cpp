[![Component Registry](https://components.espressif.com/components/ozanoner/edgeimpulse-inference-sdk/badge.svg)](https://components.espressif.com/components/ozanoner/edgeimpulse-inference-sdk)


# Edge Impulse Inference SDK for ESP32

## Overview

This repository packages the Edge Impulse C++ inferencing SDK as an ESP-IDF component for ESP32 targets.

The shared SDK is vendored under `src/edge-impulse-sdk/`, and the component builds that tree together with the Espressif port in `src/edge-impulse-sdk/porting/espressif/`.

## Current state

- The public component is published as `ozanoner/edgeimpulse-inference-sdk`.
- The component depends on `espressif/esp-dsp` and `espressif/esp-nn`.
- The repository includes two reference applications under `examples/`:
  - [examples/hello-world/README.md](examples/hello-world/README.md) — offline keyword spotting
  - [examples/hello-world-img/README.md](examples/hello-world-img/README.md) — offline image classification 
- Hardware acceleration is enabled by default, but it can be disabled with `CONFIG_EI_DISABLE_HW_ACCEL`, which controls both `EI_CLASSIFIER_TFLITE_ENABLE_ESP_NN` and `EIDSP_USE_ESP_DSP`.

## Quick start

You can use the component as follows:

1. Create a new ESP-IDF project:
```bash
# Linux
source <path_to_idf_installation>/export.sh
idf.py create-project my-tinyml-project
cd my-tinyml-project
```

2. Set the target SoC:
```bash
idf.py set-target esp32s3
```

3. Install the component from the ESP-IDF Component Registry:
```bash
idf.py add-dependency "ozanoner/edgeimpulse-inference-sdk"
```

4. Reconfigure the project to validate the setup:
```bash
idf.py reconfigure
```

5. You should see the managed components after installation:
```bash
$ ls
build  CMakeLists.txt  dependencies.lock  main  managed_components  sdkconfig

$ ls managed_components/
espressif__esp-dsp  espressif__esp-nn  ozanoner__edgeimpulse-inference-sdk
```

6. For target-specific settings, use the example `sdkconfig` defaults as a starting point. For a minimal hardware-oriented setup, you need:
```text
CONFIG_NN_ANSI_C=y
```

Registry page:
- https://components.espressif.com/components/ozanoner/edgeimpulse-inference-sdk


## Using the Component

This component packages the shared SDK only. Your application still needs the model-specific files exported from Edge Impulse, such as the generated model sources, model parameters, and application code that calls `run_classifier()`.

Typical steps for building a TinyML application with this component are:

1. Prepare your data. You can use the Edge Impulse platform (https://studio.edgeimpulse.com/login) for this purpose.
2. Design, train, and test your model on the platform.
3. Go to the deployment page of your project. Select `C++ Library` as the deployment target.
4. Build the library and download the zip package it prepares.
5. Extract the zip package and copy your model (two folders: `model-parameters` and `tflite-model`) into your project. 
6. Use the model engine for inference on input data.

The core integration point matches the local examples:

```cpp
#include "edge-impulse-sdk/classifier/ei_run_classifier.h"

signal_t signal{};
signal.total_length = EI_CLASSIFIER_RAW_SAMPLE_COUNT;
signal.get_data = &get_signal_data;

ei_impulse_result_t result{};
EI_IMPULSE_ERROR err = run_classifier(&signal, &result, false);
```

See [examples/hello-world/README.md](examples/hello-world/README.md) and [examples/hello-world-img/README.md](examples/hello-world-img/README.md) for complete ESP-IDF applications that wire the classifier into offline audio and image sample pipelines.

You can start a new project from the examples. For instance:
```bash
$ idf.py create-project-from-example "ozanoner/edgeimpulse-inference-sdk:hello-world"
Executing action: create-project-from-example
NOTICE: Example "hello-world" successfully downloaded to /home/tinymlws/projects/tmp/t2/hello-world
Done
$ cd hello-world/
$ ls
CMakeLists.txt  diagram.json  main  README.md  sdkconfig.defaults  sdkconfig.esp32s3  sdkconfig.wokwi  wokwi.toml
```

## Reporting Issues

If you find a packaging or ESP32 integration issue in this fork, open an issue at https://github.com/ozanoner/edgeimpulse-inferencing-sdk-cpp/issues.

If the issue appears to belong to the upstream shared SDK instead of this ESP-IDF packaging fork, include that context and link the upstream project when possible.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

This repository is derived from the Edge Impulse inferencing SDK and includes bundled third-party source code. See `LICENSE`, `LICENSE.3-clause-bsd-clear`, and any license files shipped with bundled dependencies for the applicable terms.
