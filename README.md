# web-models

CORS-friendly mirror of ML model files for browser-based apps.

## Why this exists

Browser apps fetching models from `huggingface.co/.../resolve/main/...` get blocked by CORS — HF only allows the `huggingface.co` origin. GitHub's `raw.githubusercontent.com` returns `Access-Control-Allow-Origin: *`, so any web app can fetch directly.

This repo mirrors a small set of ONNX models used by Flotek browser apps that don't have a backend proxy.

## Files

| File | Size | Format | Source | License |
|------|------|--------|--------|---------|
| [`yolo26n-fp32.onnx`](./yolo26n-fp32.onnx) | 9.5 MB | ONNX FP32, 80 COCO classes, NMS-free `(1, 300, 6)` output | [`openvision/yolo26-n`](https://huggingface.co/openvision/yolo26-n) (Ultralytics export) | AGPL-3.0 |
| [`yolo26n-fp16.onnx`](./yolo26n-fp16.onnx) | 4.9 MB | Same as above but FP16. Smaller, but ORT Web's WebGPU backend lacks FP16 Resize — WASM-only in practice. Prefer the FP32 file. | same | AGPL-3.0 |
| [`mdv6-yolov9c-int8.onnx`](./mdv6-yolov9c-int8.onnx) | 26 MB | ONNX INT8, MegaDetector v6 (animal/person/vehicle) | [`flotek/megadetector-onnx`](https://huggingface.co/flotek/megadetector-onnx) | MIT |

## Fetch URL pattern

```
https://raw.githubusercontent.com/runeflo/web-models/main/<filename>
```

## Constraints

- Files >100 MB require Git LFS, which redirects to a subdomain without `*` CORS — keep large models on HF and proxy them server-side (e.g. MegaDetector v5 lives on `flotek/megadetector-onnx`).
- This repo is a mirror, not a source of truth. Update the upstream first, then re-mirror here.
