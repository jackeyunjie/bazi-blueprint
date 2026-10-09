# BaZi Blueprint · 视频批量生产 SOP（v01 已验证管线）

> 管线：脚本 → edge-tts 配音 → HTML 文字卡 → Playwright 截图 → ffmpeg 合成。
> v01 全程约 5 分钟/条。每周批量产 7 条，放 `videos/` 目录，用户每天取 1 条上传。

## 单条生产五步法

1. **写分段脚本**：每条 4-6 句口播（对应内容包），一句一个音频段
2. **配音**（en-US-JennyNeural，如试男声换 en-US-AndrewNeural）：
   `python3 -m edge_tts --voice en-US-JennyNeural --text "..." --write-media segN.mp3`
   用 ffprobe 记录每段时长
3. **文字卡**：复制 `videos/cards.html`，改文案（一卡一屏大字，黑金品牌色）；
   `playwright screenshot --viewport-size=1080,1920 "file://...cards.html?card=N" cardN.png`
4. **合成**（音频连续、卡面按时长对齐，参考 v01 的 mkclip + concat 流程）：
   - 每卡生成无声 clip（时长=对应音频段时长，可在段中切两卡）
   - concat 视频流 + concat 音频流 → mux 出 `vNN-标题.mp4`
5. **目验**：抽 3-4 帧截图 + ffprobe 确认 aac 音轨，再放行

## 发布包格式（每条视频附带）

- 文件名：vNN-hook关键词.mp4
- 发布文案（caption）：1 句钩子复述 + CTA + 标签（统一底盘 #bazi #fourpillars #chineseastrology #astrology #spirituality + 2-3 个主题标签）
- 上传后开 TikTok 自动字幕（auto captions）

## v01 发布文案（可直接复制）

> 12 zodiac signs vs 5,000+ BaZi chart types. Your birth HOUR changes everything. Comment "BLUEPRINT" and I'll tell you your Day Master element 🌳🔥🌊
> #bazi #fourpillars #chineseastrology #astrology #spirituality #astrologytok

## 质量红线

- 不说 predict/guarantee/cure；不说「你一定会…」
- 卡面文字 ≤3 行大字，手机 1 秒可读
- 每条前 3 秒必须有钩子（数字冲击/认知冲突）
