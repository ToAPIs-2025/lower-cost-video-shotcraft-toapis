# 更低成本的 Video Shotcraft 图片与镜头方案

[English](README.md)

[ToAPIs · 对应模型价格与接入](https://go2v.toapis.com/sh15gc)

**2026-10-15：3 张真实输出，图片实扣合计 $0.0394（7.88积分），后台截图已确认。**

独立 API 图片素材准备与镜头素材清单；Remotion 原组件与完整品牌宣传片未验收。

## 演示

![真实输出与报价](screenshots/cn/01-models.png)

## 本次对应模型价格

| Model | Requested preset | Actual pixels | Quote / request | Actual charge |
|---|---|---|---:|---|
| [`gpt-image-2-vip`](https://go2v.toapis.com/sh15v) | low / 1K | 1024×1024 | $0.0019 | $0.0019 / 0.38 credits |
| [`gemini-nano-banana-2.1`](https://x.toapis.com/sh15n) | 1K | 1024×1024 | $0.0150 | $0.0150 / 3 credits |
| [`gpt-image-2.5-flare`](https://im25.toapis.com/sh15f) | high / 1K | 1024×1024 | $0.0225 | $0.0225 / 4.5 credits |

报价合计: **$0.0394**. 本次图片实扣已由后台截图确认；1积分=$0.005。除 global 为用户提供价表，其余为当天实时 `/v1/pricing`。不同模型/画质不是等效质量比较。

## 复现

Python 3.11+; Node 22+ for Node/TS native-entry checks.

```bash
python -m pip install Pillow
python generate_case.py --model gpt-image-2-vip --prompt "An original product photograph, no text"
# Dry-run: no network call. Set TOAPIS_API_KEY privately before --live.
python generate_case.py --model gpt-image-2-vip --prompt "An original product photograph, no text" --run-id my-unique-case --output result.png --live
```

`TOAPIS_BASE_URL=https://toapis.com` or `https://toapis.cn`; `/v1` is appended once. `TOAPIS_STATE_DIR` stores private durable task state. `TOAPIS_MAX_IMAGE_USD` defaults to 0.10. Never commit the key or private state.

### 原生入口与范围

Use `shot-assets.json` to map generated original product images to the upstream `spotlight-hero-card` shot idea. The upstream recipe and fixed source links are included in `SHOT_REUSE.md`. This example validates API asset preparation, not the original Remotion component or full film.

### API mapping

`POST /v1/images/generations` → persist returned task ID → `GET /v1/images/generations/{id}` → download result without forwarding the API key to the CDN. Bearer authentication is used only for the API origin. No automatic POST retries. Uncertain submissions remain blocked until reconciled.

The adapter maps explicit pixel hints to aspect ratio and sends model-specific resolution fields. JPEG responses are preserved as original JPEG and separately converted to PNG for display; real pixel sizes are recorded. A requested 1K tier is not a guarantee of exactly 1024×1024 pixels.

## 证据与截图教程

- [Per-task records](evidence/)
- [Original outputs](examples/)
- [Model comparison](screenshots/cn/01-models.png)
- [API steps](screenshots/cn/02-api.png)
- [Reuse and limits](screenshots/cn/03-reuse.png)

Upstream: [Vincentwei1021/video-shotcraft](https://github.com/Vincentwei1021/video-shotcraft/tree/13c74a707786872a18e3317e48c5c94011027620), pinned `13c74a707786872a18e3317e48c5c94011027620`. Community work; no author endorsement or existing partnership is implied. [Attribution](NOTICE.md).

[Dashboard-confirmed itemized image charges](evidence/actual-charges.json). Video submissions without task IDs are excluded and remain unreconciled.
