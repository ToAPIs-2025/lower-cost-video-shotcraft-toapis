# 更低成本的 Video Shotcraft 图片与镜头方案

<!-- TOAPIS-MODEL-COST-UPDATE-20261016 -->
## 模型类型、测试档位与花费

更新：2026-10-16。下表为已验证案例的历史档位与价格；本段补充不改变下方原批次记录。

| 模型（API ID） | 类型 | 测试档位 | 单次报价 | 已确认单次消费 | 数量 |
|---|---|---|---:|---:|---:|
| gpt-image-2(VIP) (`gpt-image-2-vip`) | 图片 | low / 1K / n=1 | $0.0019/image | $0.0019 | 1 |
| gpt-image-2.5-flare | 图片 | high / 1K / n=1 | $0.0225/image | $0.0225 | 1 |
| Nano Banana 2.1 (`gemini-nano-banana-2.1`) | 图片 | 1K / n=1 | $0.015/image | $0.015 | 1 |

原图片/视频案例已核对消费小计：**$0.0394**。

独立图片素材已验证；原Remotion组件、完整成片和视频模型接入未验收。

gpt-image-2(VIP) **$0.0019/image 起**；本表 low/1K 实测档位为 **$0.0019/image**。不同模型、质量和分辨率不代表同质效果。
<!-- /TOAPIS-MODEL-COST-UPDATE-20261016 -->

[English](README.md)

[ToAPIs · 对应模型价格与接入](https://go2v.toapis.com/sh15gc)

**2026-10-15：3 张真实输出，图片实扣合计 $0.0394（7.88积分），后台截图已确认。**

独立 API 图片素材准备与镜头素材清单；Remotion 原组件与完整品牌宣传片未验收。

## 演示

![真实输出与实扣](screenshots/cn/01-models.png)

## 本次对应模型价格

| Model | Requested preset | Actual pixels | Quote / request | Actual charge |
|---|---|---|---:|---|
| [`gpt-image-2-vip`](https://go2v.toapis.com/sh15v) | low / 1K | 1024×1024 | $0.0019 | $0.0019 / 0.38 credits |
| [`gemini-nano-banana-2.1`](https://x.toapis.com/sh15n) | 1K | 1024×1024 | $0.0150 | $0.0150 / 3 credits |
| [`gpt-image-2.5-flare`](https://im25.toapis.com/sh15f) | high / 1K | 1024×1024 | $0.0225 | $0.0225 / 4.5 credits |

本次图片实扣合计： **$0.0394**. 本次图片实扣已由后台截图确认；1积分=$0.005。除 global 为用户提供价表，其余为当天实时 `/v1/pricing`。不同模型/画质不是等效质量比较。

## 复现

环境：Python 3.11及以上；Node/TS原生入口验收另需Node 22及以上。

```bash
python -m pip install Pillow
python generate_case.py --model gpt-image-2-vip --prompt "An original product photograph, no text"
# 默认演练不联网。实测前在本机私下配置 TOAPIS_API_KEY，再显式加 --live。
python generate_case.py --model gpt-image-2-vip --prompt "An original product photograph, no text" --run-id my-unique-case --output result.png --live
```

`TOAPIS_BASE_URL`可填`https://toapis.com`或`https://toapis.cn`，只补一次`/v1`。`TOAPIS_STATE_DIR`保存本地私有任务状态；`TOAPIS_MAX_IMAGE_USD`默认单图报价上限为0.10。不要提交Key或私有状态目录。

### 原生入口与范围

`shot-assets.json`把原创产品图片整理为上游`spotlight-hero-card`镜头的素材清单。原始镜头卡与固定版本源码入口见`SHOT_REUSE.md`。本轮验收到API素材准备，未运行原始Remotion组件或完整宣传片。

### 接口映射

`POST /v1/images/generations`提交一次并保存返回任务ID，再通过`GET /v1/images/generations/{id}`查询结果。下载CDN图片时不转发API Key；Bearer仅发给允许的API域名。没有自动POST重试，未确认是否受理的提交会保留并阻止重复生成。

适配器将像素提示转换为画幅比例，并按模型发送分辨率字段。JPEG结果保留原始字节，另存PNG供展示；真实尺寸均写入记录。请求1K不保证返回恰好1024×1024。

## 证据与截图教程

- [逐任务记录](evidence/)
- [原始输出](examples/)
- [模型对比](screenshots/cn/01-models.png)
- [API调用步骤](screenshots/cn/02-api.png)
- [复用与验收边界](screenshots/cn/03-reuse.png)

Upstream: [Vincentwei1021/video-shotcraft](https://github.com/Vincentwei1021/video-shotcraft/tree/13c74a707786872a18e3317e48c5c94011027620), pinned `13c74a707786872a18e3317e48c5c94011027620`. 社区独立示例，不代表作者背书或已有合作。[署名与许可](NOTICE.md)。

[后台确认的逐项图片消费](evidence/actual-charges.json)。未取得任务ID的视频提交不计入图片合计，仍单独待核。
