[![Component Registry](https://components.espressif.com/components/ozanoner/edgeimpulse-inference-sdk/badge.svg)](https://components.espressif.com/components/ozanoner/edgeimpulse-inference-sdk)


# EdgeImpulse Inference SDK for ESP32

## Overview

This repository packages the Edge Impulse C++ inferencing SDK as an ESP-IDF component for ESP32 targets.

The shared SDK is vendored under `src/edge-impulse-sdk/`, and the component builds that tree together with the Espressif port in `src/edge-impulse-sdk/porting/espressif/`.

This repo is primarily a packaging fork. It is useful when your ESP-IDF project already contains model-specific Edge Impulse export files and you want to consume the shared runtime through the Espressif Component Registry.

## Current State

- The public component is published as `ozanoner/edgeimpulse-inference-sdk`.
- The component declares `espressif/esp-dsp` and `espressif/esp-nn` as dependencies.
- Hardware acceleration can be disabled with `CONFIG_EI_DISABLE_HW_ACCEL`, which controls both `EI_CLASSIFIER_TFLITE_ENABLE_ESP_NN` and `EIDSP_USE_ESP_DSP`.
- The repository includes two local example apps under `examples/` for validation and reference:
  - `examples/hello-world`: offline keyword spotting sample
  - `examples/hello-world-img`: offline image classification sample with higher memory needs


## Installation

Install the component from the ESP-IDF Component Registry:

```bash
idf.py add-dependency "ozanoner/edgeimpulse-inference-sdk"
```

Or declare it directly in your project's `idf_component.yml`:

```yaml
dependencies:
  ozanoner/edgeimpulse-inference-sdk: "^0.1.0"
```

Registry page:

- https://components.espressif.com/components/ozanoner/edgeimpulse-inference-sdk

For maintainers, stable releases should use matching Git tags such as `v0.1.0`. If you publish a prerelease for validation, use a matching prerelease tag such as `v0.1.0-rc1` and the staging registry.

## Using the Component

This component packages the shared SDK only. Your application still needs the model-specific files exported from Edge Impulse, such as the generated model sources, model parameters, and the application code that calls `run_classifier()`.

The core integration point matches the local examples:

```cpp
#include "edge-impulse-sdk/classifier/ei_run_classifier.h"

signal_t signal{};
signal.total_length = EI_CLASSIFIER_RAW_SAMPLE_COUNT;
signal.get_data = &get_signal_data;

ei_impulse_result_t result{};
EI_IMPULSE_ERROR err = run_classifier(&signal, &result, false);
```

See `examples/hello-world` and `examples/hello-world-img` for complete ESP-IDF applications that wire the classifier into offline audio and image sample pipelines.


## References

- Staging registry: https://components-staging.espressif.com/
- Production registry: https://components.espressif.com/
- Fork repository: https://github.com/ozanoner/edgeimpulse-inferencing-sdk-cpp
- Upstream SDK: https://github.com/edgeimpulse/inferencing-sdk-cpp
- ESP-IDF Getting Started: https://docs.espressif.com/projects/esp-idf/en/latest/get-started/


## Reporting Issues

If you find a packaging or ESP32 integration issue in this fork, open an issue at https://github.com/ozanoner/edgeimpulse-inferencing-sdk-cpp/issues.

If the issue appears to belong to the upstream shared SDK instead of this ESP-IDF packaging fork, include that context and link the upstream project when possible.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

This repository is derived from the Edge Impulse inferencing SDK and includes bundled third-party source code. See `LICENSE`, `LICENSE.3-clause-bsd-clear`, and any license files shipped with bundled dependencies for the applicable terms.
