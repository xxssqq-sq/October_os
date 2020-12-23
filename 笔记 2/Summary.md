 

- Upload Images

February 22, 2020 by typora.io

- 简介
  - 注意事项
- Configuration
  - [Validate Image Uploader](https://support.typora.io/Upload-Image/#validate-image-uploader)
  - Image Uploaders
    - [iPic (macOS, Freemium)](https://support.typora.io/Upload-Image/#ipic-macos-freemium)
    - [uPic (macOS, OpenSource)](https://support.typora.io/Upload-Image/#upic-macos-opensource)
    - PicGo-Core (Command line) (OpenSource)
      - [Install prebuilt binary of PicGo-Core (Linux / Windows)](https://support.typora.io/Upload-Image/#install-prebuilt-binary-of-picgo-core-linux--windows)
      - [Install PicGo-Core via node package manager (Requires NodeJS runtime)](https://support.typora.io/Upload-Image/#install-picgo-core-via-node-package-manager-requires-nodejs-runtime)
      - Config PicGo-Core
        - [Option 1: Edit the config file](https://support.typora.io/Upload-Image/#option-1-edit-the-config-file)
        - [Option 2: Config via CLI](https://support.typora.io/Upload-Image/#option-2-config-via-cli)
        - [Use Plugins](https://support.typora.io/Upload-Image/#use-plugins)
    - PicGo.app (Chinese Language Only)
      - [Difference between PicGo.app and PicGo-Core (command line)](https://support.typora.io/Upload-Image/#difference-between-picgoapp-and-picgo-core-command-line)
    - Custom
      - [Use current filename / filepath in custom commands](https://support.typora.io/Upload-Image/#use-current-filename--filepath-in-custom-commands)
  - Upload Automatically When Insert Images
    - [Global Setting](https://support.typora.io/Upload-Image/#global-setting)
    - [Per-document Setting](https://support.typora.io/Upload-Image/#per-document-setting)
- Usage
  - [Upload Selected Image](https://support.typora.io/Upload-Image/#upload-selected-image)
  - [Upload All Local Images](https://support.typora.io/Upload-Image/#upload-all-local-images)
- [Troubleshooting](https://support.typora.io/Upload-Image/#troubleshooting)
- [Migration from older Typora version](https://support.typora.io/Upload-Image/#migration-from-older-typora-version)

# Summary

In newer version of Typora (≥ 0.9.9.32 on macOS or 0.9.84 on Windows / Linux), we added a “upload image” function to upload images to a cloud image storage via 3rd apps or scripts.

Its motivation is that, since markdown files is just plain text files, when you embed images, the markdown files does not “own” those images, but just keep a weak reference to used external image files. When you move or share markdown files, those images should also be moved or shared, which brings maintaince costs. But if those images are hosted online, you can move or share markdown files freely without maintaining the reference between text plain and the images it used.

Typora now supports apps like iPic, uPic, PicGo, etc, which is able to upload your images into [Imgur](http://imgur.com/), [Flickr](https://www.flickr.com/),[ Amazon S3](https://aws.amazon.com/s3/), [Github](https://support.typora.io/Upload-Image/www.github.com), or other image hosting services.

## Warning!

> Before you use this feature, please notice that Typora does not own those image uploader apps or cloud storage services, they are all 3rd services, so **please take care about their license, user agreement, privacy policy, service stability and reliability by yourself**.
>
> By using this feature after enabled in preferences panel, Typora will pass the path of used images to those 3rd tools, then they will upload your images to a 3rd (or 4th) cloud image storage, Typora has no control or guarantee over this process, so please pick up a reliable service to ensure your data security.
>
> Besides the privacy and reliability issues, some service may have an expiration time for hosting your images, some service may not provide a “delete” function even after you uploaded sensitive images by accident, and service follows laws that is not in your country, banning certain type of images or users in certain country, etc.
>
> In short, please read their manual then choose the app and service for uploading images.

# Configuration配置

You can enable（使用 ） and config（配置） image uploader in preferences panel（偏好面板）, we provide integrations（整合、集成、积分） with following apps / tools:

我们提供了以下工具的集成：

- iPic (macOS)

- uPic (macOS)

- PicGo.app (macOS / Windows / Linux, Simplified Chinese language only)

- PicGo (Command Line) (Windows / Linux)

- Custom (macOS / Windows / Linux)

  

After select one option, you may need to download and installed the required app, do simple configuration of which cloud storage you want to host your images on that app, then click “Test Uploader” to test if Typora can upload local images using current service and configuration.

## Validate Image Uploader

By clicking the “Test Uploader” button, Typora will try to upload test images using current configuration, you can see the details, error messages, and final url result after upload on following “Validation” dialog:

![validation result](https://support.typora.io/media/image-upload/validation%20result.png)

If it shows “Validation Failed”, you need to check the reason from the raw output.

## Image Uploaders

### iPic (macOS, Freemium)

![ipic](https://support.typora.io/media/image-upload/ipic.jpg)

[iPic][ttps://itunes.apple.com/app/id1101244278] is a **freemium** app which allows you to upload local images into various cloud service, including [Imgur](http://imgur.com/), [Flickr](https://www.flickr.com/),[ Amazon S3](https://aws.amazon.com/s3/), etc, and return you a web url of the uploaded image for public access.

[Download](https://itunes.apple.com/app/id1101244278)

[Document](https://github.com/toolinbox/iPic) [中文文档](https://toolinbox.net/iPic/)

To use iPic as the image uploader service, you just need to install iPic app from [MAS](https://itunes.apple.com/app/id1101244278) or from their website, then config its image host following instructions [here](https://github.com/toolinbox/iPic#image-hosts). And select “iPic” as “image uploader” in preferences panel of Typora, then you can upload images to preferred cloud image storage using iPic.

### uPic (macOS, OpenSource)

![upic](https://raw.githubusercontent.com/gee1k/oss/master/screenshot/uPic/logo.png)

[uPic](https://github.com/gee1k/uPic) is a native, powerful, beautiful and simple picture and file upload tool for macOS.

You could install via Homebrew:

```
brew cask install upic
```

Or Download from github [release](https://github.com/gee1k/uPic/releases).

To use uPic as the image uploader service, you just need to install iPic app from the instructions above, then config its image host following instructions [here](https://blog.svend.cc/upic/en/). And select “uPic” as “image uploader” in preferences panel of Typora, then you can upload images to preferred cloud image storage using uPic.

uPic also has a detailed English [Document](https://blog.svend.cc/upic/en/).

### PicGo-Core (Command line) (OpenSource)

[PicGo-Core](https://github.com/PicGo/PicGo-Core) is an open source node module that supports picture uploading. It also supports plugins to extend its functionality.

Since it is a node module, we provide two ways to install and use it.

#### Install prebuilt binary of PicGo-Core (Linux / Windows)

Typora provide a prebuilt binary of PicGo using nexe to packing the node module into one-file binary. You could just click the “Download” button to install the PicGo binary, and Typora will start downloading and put it under Typora’s support folder directly.

<details style="display: block;"><summary style="display: block;"><a name="picgo-location" style="background-color: transparent; color: rgb(70, 63, 92); text-decoration: underline;"></a>Location of downloaded PicGo-Core binary</summary></details>

Due to the security rule of macOS Catalina, the prebuilt binary is not functionally on Catalina, so you may try the following option:

#### Install PicGo-Core via node package manager (Requires NodeJS runtime)

If you have node or yarn installed, you cloud run following commands in terminal.

```
npm install picgo -g

# or

yarn global add picgo
```

Then you can input “which picgo” in terminal to get its actual installed location, then, select “Custom Command” as the “Image Uploader” function and input “[your node path] [your picgo-core path] upload” as the command. If you have “node” and “picgo” installed in system PATH directly, you could also fill “picgo upload” as the custom command directly.

![install-picgo](https://support.typora.io/media/image-upload/install-picgo.png)

![custom](https://support.typora.io/media/image-upload/custom.png)

#### Config PicGo-Core

##### Option 1: Edit the config file

Please edit `config.json` at following location

- Linux / macOS → `~/.picgo/config.json`.
- Windows → `C:\Users\[your user name]\.picgo\config.json`.

For details, please refer to [this document (Chinese Only)](https://picgo.github.io/PicGo-Core-Doc/zh/guide/config.html#默认配置文件).

##### Option 2: Config via CLI

If you installed pico-core via node, please input `picgo -h` to show help of picgo, and do configuration follow its help.

If you used the prebuilt binary of PicGo-Core installed from Typora’s preferences panel, please find the binary from the folder (see instructions [here](https://support.typora.io/Upload-Image/#picgo-location)), then open the folder in terminal, and do configurations via (`./picgo -h`).

You can find its CLI usage in https://github.com/PicGo/PicGo-Core#use-in-cli.

##### Use Plugins

You can use the cli interface to install plugins of PicGo-Core.

### PicGo.app (Chinese Language Only)

![picgo](https://raw.githubusercontent.com/Molunerfinn/test/master/picgo/New%20LOGO-150.png)

[PicGo](https://picgo.github.io/PicGo-Doc/zh/) is an GUI app built from PicGo-Core. Since it only provides a Chinese user interface, you will find this option only when your Typora is using Simplified Chinese language.

To use with Typora, we require PicGo ≥ 2.2.0 with [PicGo-Server](https://picgo.github.io/PicGo-Doc/zh/guide/config.html#picgo-server设置) enabled and use port in default value (36677). And when Typora upload images using PicGo, PicGo will be launched and kept running.

For Linux / Windows users, please set the path of PicGo binary in Typora’s preferences setting after install it.

![picgo](https://support.typora.io/media/image-upload/picgo.PNG)

#### Difference between PicGo.app and PicGo-Core (command line)

- Currently PicGo (app) is Chinese only.
- PicGo.app provides a GUI, and therefore more easy to setup compared with the CLI version.
- Upload using PicGo-Core (command line) costs less computing resources, because the process will only be running during the uploading process, and will exit after uploading succeed or failed. While uploading via PicGo.app, PicGo.app will be always kept running can not exit automatically. Also, PicGo.app is an electron app, which eats more computing resources.
- PicGo.app and PicGo-Core use different config files, but you can copy the json object under `picBed` key in the config file of PicGo.app into PicGo’s config file.
- PicGo.app provides other features, like upload history, auto rename, etc.

### Custom

You could config a custom command to upload images, using tools that is not listed in above options, or event write your own tools / scripts. Typora will append all images that needs to be uploaded after the custom command you filled.

Then, Typora will fetch image urls from the last N lines of the standard output of your custom command. (N is the number of images to upload).

For example, if you write a tool `upload-image.sh`, then you can input `[some path]/upload-image.sh` in the command filed. Typora will call `[some path]/upload-image.sh "image-path-1" "image-path-2"` to upload two images located in `image-path-1` and `image-path-2`. Then the command may return something like:

```
Upload Success:
http://remote-image-1.png
http://remote-image-2.png
```

Then Typora will get the two remote image url from the output, and replace the original local images used in the Markdown document.

You could click the “Test Uploader” button to verify your custom commands.

#### Use current filename / filepath in custom commands

You can use `${filename}` and `${filepath}` in your custom commands, they will be replace as the current markdown file name and current file path. For “untitled” files that have not been saved on your disk, they will be empty strings.

## Upload Automatically When Insert Images

#### Global Setting

User can tell Typora to upload images automatically when insert (including insert image from menu, Touch Bar, via copy & paste or drag & drop). To enable this feature, please select “Upload Image” under “When Insert…” options like following screenshot suggests.

Then, if you want the “auto upload” only applies to local images, please only check `Apply above rules to local images`, if you also want to re-upload images that is already hosting on remote website, you can also check `Apply above rules to online images`.

![auto-upload](https://support.typora.io/media/image-upload/auto-upload.png)

#### Per-document Setting

Firstly, please enable `Allow upload images automatically based on YAML settings` in preferences panel.

Then if your Markdown file contains following config in YAML front matter, Typora will use this option to upload images automatically when insert image:

```
typora-copy-images-to: upload
```

You could also click menu → `Format` → `Image` → `When insert local image…` → `Upload Image` to insert this YAML front matter and enable this behavior.

![menu](https://support.typora.io/media/image-upload/menu.png)

# Usage

## Upload Selected Image

You could right click on an image, and click “Upload Image” to upload selected image using the app configured in preferences panel.

![upload](https://support.typora.io/media/image-upload/upload.gif)

## Upload All Local Images

If your Markdown files contains lots of local images, and you want to upload all of them in one click, you could click menu → `Format` → `Image` → `Upload All Local Images` to upload all local images.

# Troubleshooting

- **“Please set an image uploader in Preferences Panel before using this function.”**

  In order to make “image upload” functional, you must set an image uploader in Preferences Panel following the [configuration](https://support.typora.io/Upload-Image/#configuration) section,

- **Image Upload Failed**

  When upload image failed, Typora will print the error message when calling the 3rd app you configured as image upload provider. You could find the cause from the error message.

  If you cannot know the cause, you could try validate the image upload option by clicking the “Test Uploader” button in preferences panel, and if it success or not. Also, you may try if you upload images using those app without Typora, and check if it can succeed or not.

  If you find those app cannot upload images successfully, you could contact to their maintainer for further support.

- **Garbled console output on Windows**

  If you use, custom command, and after clicking “Test Uploader” button in preferences panel, and its console output is Garbled characters, you may try to force the process to use UTF8 encoding, by prepending `@chcp 65001 >nul & cmd /d/s/c `before your custom command.

- **[PicGo ERROR]: Error: API v1 is deprecated, please refer to https://doc.sm.ms/ for v2 API documentation.**

  It is caused by PicGo’s support issue of its default image hosting service: sm.ms, please refer [PicGo/PicGo-Core#30](https://github.com/PicGo/PicGo-Core/issues/30), or use other image service other than the default one

- **Fail to parse result image path from**

  Like [#3309](https://github.com/typora/typora-issues/issues/3309), Typora requires the return url of an image to start with `http://`, `https://`, or `file://`. Other protocols are not supported. Also, in HTML / Markdown, image `src` without protocols, are treated like relative path.

- **EPERM: operation not permitted mkdir “\**/upload”**

  This should be fixed in 0.9.87, please upgrade Typora.

# Migration from older Typora version

In older version of Typora on macOS, we only provide the integration with “ipic”. So if your older Markdown document contains

```
typora-copy-images-to: ipic
```

Typora will still use `ipic` to upload images in this file, you could update the behavior by changing into `typora-copy-images-to: upload`.

hosted on [Github](https://github.com/typora/wiki-website).