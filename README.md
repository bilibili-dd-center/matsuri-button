# Natsuiro Matsuri button

Matsuri button [Click here to visit https://natsuiromatsuri.moe](https://natsuiromatsuri.moe)

Related Links:

* [Natsuiro Matsuri's Youtube channel](https://www.youtube.com/channel/UCQ0UDLQCjY0rmuxCDE38FGg)

* [Natsuiro Matsuri's Twitter](https://twitter.com/natsuiromatsuri)

## 参与完善本项目

请发issue，附上要提交的音频与音频分类

### 添加或修改音频

请各位以文件夹的形式分类，并将音频文件以 中文_日文.mp3 的格式命名后，将文件下载地址贴至issue里

音频一律为mp3格式。

添加的新音频请先进行音量标准化。可以使用MP3GainGUI之类的软件，目前音量全部标准化为80。

### 参与翻译

请帮助进行日语的翻译！

主程序翻译在 [src/locales](src/locales) 中的以语言名命名的js文件中。

语音的翻译在 [public/voices.json](public/voices.json) 中。

### 参与网页开发

请参考[部署本地开发环境](#部署本地开发环境)一节。

## 部署本地开发环境

本站使用 Vue + jQuery + Bootstrap 3 开发。

要部署本地开发环境，请先安装最新版的node。然后按照如下步骤操作：

1. Clone代码到本地。

2. 进入代码目录，运行`npm install`。

3. 运行本地开发服务器`npm run serve`，在代码修改过程中，本地开发服务器可以即时反映修改的结果。

4. 要编译可供部署的文件，请运行`npm run build`，这会生成`dist`目录。本站为全静态，将整个`dist`目录部署即可。

若要为本项目贡献你的代码，你不必在本地编译。在开发服务器中测试通过并推送至Github后，直接给本项目提交Pull Request即可。

## LICENCE

程序部分： MIT

音频部分按照hololive二次创作条例进行

本项目为爱好者作品，和hololive官方没有关联

## Special Thanks

This project is supported by MeowSound Idols.

This project is modified based on the [Meamea button](https://github.com/zyzsdy/meamea-button).
