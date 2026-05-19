# llama-cpp-turboquant-tq3-merge

Runtime fork of TurboQuant GGUFs `llama.cpp`.\
With the following 2 forks merged together: [llama-cpp-turboquant](https://github.com/QuinsZouls/llama-cpp-turboquant) & [llama.cpp-tq3](https://github.com/daniel-eder/llama.cpp-tq3).

This fork brings together TurboQuant's `turbo2`, `turbo3`, `turbo4` KV-cache types, with tq3's `TQ3_1S` and `TQ4_1S` weights, and tq3's `tq3_0` KV-cache type.

Due to type conflicts, this is not compatible with TurboQuant's `TQ3_1S` and `TQ4_1S` weighted models:
- Disabled TurboQuant's `GGML_TYPE_TQ3_1S` weight with ID `45`
- Disabled TurboQuant's `GGML_TYPE_TQ4_1S` weight with ID `46`
- Changed TurboQuant's `GGML_TYPE_TURBO4_0` to ID `198`, since it's used for KV-cache only, and ID `44` is needed by tq3's `TQ3_1S`
- Using tq3's `GGML_TYPE_TQ3_1S` weight with ID `44`

**Notes:**
- If you need TurboQuant's `TQ3_1S` and `TQ4_1S` weights, just use the [llama-cpp-turboquant](https://github.com/TheTom/llama-cpp-turboquant) fork itself.
- This repo is runtime only, focused on GGUF with CUDA.
- This is my ([@Jashepp](https://github.com/Jashepp)) first time with the llama.cpp codebase, and I barely understand it, so some things may be broken (`TQ3_1S` weighted models are currently not working).
- This repo should not be used in a production/commercial situation. There are only [several tq3 weighted models](https://huggingface.co/YTan2000), which can only be quantized using [turbo-tan](https://github.com/turbo-tan)'s private llama fork.\
If you want to contribute meaningful changes/improvements, consider forking this repo, or merging turboquant & tq3 together in a more professional way. Or use and contribute to the [llama-cpp-turboquant](https://github.com/TheTom/llama-cpp-turboquant) fork itself.

## What can this do?

This allows TurboQuant's KV-cache types to be used on tq3's weighted models (or any gguf model).

For example: TurboQuant's `turbo4` with tq3's `tq3_0`
```
llama-server --cache-type-k turbo4 --cache-type-v tq3_0
```

## Why?

After experimenting with many different small models to run locally, the fastest I ([@Jashepp](https://github.com/Jashepp)) could get any model working on my RTX 3060 12gb & AMD 5900X, is `YTan2000/Qwen3.6-35B-A3B-TQ3_4S` which can only be used by `llama.cpp-tq3`.\
However I wanted to use `turbo4` from `llama-cpp-turboquant` for the k-cache, to squeeze out some more performance, so that led me down the rabbit hole of merging them together.

## Status

I ([@Jashepp](https://github.com/Jashepp)) have only tested this with the following models, with GGUF on CUDA:
- [YTan2000\Qwen3.5-27B-TQ3_1S](https://huggingface.co/YTan2000/Qwen3.5-27B-TQ3_1S) - ❌ not working.
- [YTan2000\Qwen3.6-35B-A3B-TQ3_4S](https://huggingface.co/YTan2000/Qwen3.6-35B-A3B-TQ3_4S) - ✅ correctly uses TurboQuant's `turbo4` with tq3's `TQ3_4S` weights, I recommend this model.


## More Details

For more details, view the [llama-cpp-turboquant](https://github.com/QuinsZouls/llama-cpp-turboquant) README & [llama.cpp-tq3](https://github.com/daniel-eder/llama.cpp-tq3) README.


## Credits

- `TheTom/turboquant_plus` - TurboQuant llama fork with `turbo2`, `turbo3`, `turbo4`
  - https://github.com/TheTom/llama-cpp-turboquant
- `QuinsZouls/llama-cpp-turboquant` - fork of `TheTom/turboquant_plus`, used for this repo.
  - https://github.com/QuinsZouls/llama-cpp-turboquant

- `turbo-tan/llama.cpp-tq3` - TurboQuant llama fork with `tq3_0`, `TQ3_1S`, `TQ3_4S`
  - https://github.com/turbo-tan/llama.cpp-tq3
- `daniel-eder/llama.cpp-tq3` - fork of `turbo-tan/llama.cpp-tq3`, used for this repo.
  - https://github.com/daniel-eder/llama.cpp-tq3

- Google Research for the TurboQuant compression algorithm and technical paper.
  - https://research.google/blog/turboquant-redefining-ai-efficiency-with-extreme-compression/

- Two Minute Papers YouTube video - how I ([@Jashepp](https://github.com/Jashepp)) found out about turboquant.
  - https://www.youtube.com/watch?v=7YVrb3-ABYE
