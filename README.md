# -> [Demo Site](https://storage.googleapis.com/birme-sd-variant/index.html?target_width=512&target_height=512) <-

# Birme Variant for Stable Diffusion
在训练 Stable Diffusion (或其他生成图像模型) 我们需要高质量且裁剪成512x512的训练图像。Birme是快速完成此任务的最佳工具，并借助 [smartcrop.js](https://github.com/jwagner/smartcrop.js/)的帮助，它确实是一个强大的批量裁剪图像工具。

## 本地安装
克隆该仓库并在您喜欢的浏览器（不包括Firefox）中打开index.html。随时添加书签！
```bash
git clone https://github.com/luoyuu77/birme-sd-variant.git
cd birme-sd-variant
python -m webbrowser index.html   # 或者直接打开index.html文件
```

## 使用Docker-Compose运行
```bash
git clone https://github.com/luoyuu77/birme-sd-variant.git
cd birme-sd-variant
docker-compose up -d
#  打开浏览器访问 => http://<HOST_IP>:8080
```

## Problem
Birme限制了用户选择应用的平滑处理方式，这可能导致裁剪后的图像质量较低。

在Birme代码中，注意到con.imageSmoothingQuality = "medium";这一行硬编码了裁剪图像时的平滑质量。
```js
process_image(img, file) {
    ...
    let canvas = document.createElement("canvas");
    canvas.width = tw;
    canvas.height = th;
    let con = canvas.getContext("2d");
    con.imageSmoothingEnabled = true;
    con.imageSmoothingQuality = "medium";
    ...
}
```
(sourced on 10-14-22: [line #627](https://www.birme.net/static/js/scripts-323dd.js?953e6bb6))

## 解决方案
在“图像格式/质量”设置中选择所需的平滑质量
![Image of the Quality Preset dropdown box in the "Image Format / Quality settings](https://i.imgur.com/j2Uh1KJ.png)

## 结果
待办事项：显示“中”、“高”和“赫米特”质量预设的比较结果
通常，高质量更适合风景/主体，而中等质量更适合平滑处理近距离文本。

## 限制
- 🦊 不支持FIREFOX - [支持的浏览器](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/imageSmoothingQuality#browser_compatibility)

## 作者
- [Birme Author, support them](https://www.birme.net/)
- Small feature written by me

## 扩展
The Hermite quality option uses the [Hermite resize library](https://github.com/viliusle/Hermite-resize) so you can experiment with what gives you the best quality image for your source images.
