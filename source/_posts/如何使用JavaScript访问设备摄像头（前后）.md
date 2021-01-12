---

title: 如何使用JavaScript访问设备摄像头（前后）
date: 2020/6/9 09:08:20
tags: 前端 javascript

---

在这篇文章中，我将向您展示如何通过JavaScript在网页上访问设备的摄像头，并支持多种浏览器，而无需外部库。

<!-- more -->

#  如何使用相机API

要访问用户的相机（或麦克风），我们使用JavaScript **MediaStream API**。该API允许通过流访问这些设备捕获的视频和音频。

第一步是检查浏览器是否支持此API：

```js
if (
  "mediaDevices" in navigator &&
  "getUserMedia" in navigator.mediaDevices
) {
  // ok, 浏览器支持它
}
```

在现代浏览器中，支持是不错的（当然没有Internet Explorer）。



## 捕获视频流

要捕获由摄像机生成的视频流，我们使用 `mediaDevices` 对象的 `getUserMedia` 方法。这个方法接收一个对象，其中包含我们要请求的媒体类型（视频或音频）和一些要求。首先，我们可以通过 `{video: true}` 来获取摄像机的视频。

```js
const videoStream = await navigator.mediaDevices.getUserMedia({ video: true });

```

此调用将询问用户是否允许访问摄像机。如果用户拒绝，它将引发异常并且不返回流。因此，必须在 `try/catch` 块内完成处理这种情况。

![](http://myimgcloud.oss-cn-hangzhou.aliyuncs.com/202006/cameras-with-javascript/2.png)





请注意，它返回一个Promise，因此您必须使用 `async/await` 或 `then` 块。在Mac OS系统上还会弹出授权

![](http://myimgcloud.oss-cn-hangzhou.aliyuncs.com/202006/cameras-with-javascript/3.png)

点击“好”，就可以访问电脑摄像头了，控制台输出的 `videoStream` 对象如下

![](http://myimgcloud.oss-cn-hangzhou.aliyuncs.com/202006/cameras-with-javascript/4.png)

## 视频规格（requirements）

我们可以通过传递有关所需分辨率以及最小和最大限制的信息来改善视频的要求：

```js
const constraints = {
  video: {
    width: {
      min: 1280,
      ideal: 1920,
      max: 2560,
    },
    height: {
      min: 720,
      ideal: 1080,
      max: 1440,
    },
  },
};

const videoStream = await navigator.mediaDevices.getUserMedia(constraints);
```

这样，流以正确的宽度和高度比例进入，如果它是处于纵向模式的手机，则需要进行尺寸反转。

## 在页面上显示视频

既然有了流，我们该如何处理？

我们可以在页面上的 `video` 元素中显示视频：

```js
// 页面中有一个 <video autoplay id="video"></video> 标签
const video = document.querySelector("#video");
const videoStream = await navigator.mediaDevices.getUserMedia(constraints);
video.srcObject = videoStream;
```

请注意 `video` 标签中的自动播放属性 `autoplay`，没有它，你需要调用 `video.play()` 才能真正开始显示图像。

![](http://myimgcloud.oss-cn-hangzhou.aliyuncs.com/202006/cameras-with-javascript/5.png)

## 访问手机的前后摄像头

默认情况下，`getUserMedia` 将使用系统默认的视频录制设备。如果是有两个摄像头的手机，它使用前置摄像头。

要访问后置摄像头，我们必须在视频规格中包括 `faceModeMode:"environment"`：

```js
const constraints = {
  video: {
    width: { ... },
    height: { ... },
    facingMode: "environment"
  },
};
```

默认值为 `faceingMode:"user"`，即前置摄像头。

需要注意的是，如果你想在已经播放视频的情况下更换摄像机，你需要先停止当前的视频流，然后再将其替换成另一台摄像机的视频流。

```js
videoStream.getTracks().forEach((track) => {
  track.stop();
});
```

## 截屏

你可以做的另一件很酷的事情是捕获视频的图像（屏幕快照）。

你可以在canvas上绘制当前视频帧，例如：

```js
// 页面中有一个 <canvas id="canvas"></canvas> 标签
const canvas = document.querySelector("#canvas");
canvas.width = video.videoWidth;
canvas.height = video.videoHeight;
canvas.getContext("2d").drawImage(video, 0, 0);
```

你还可以在 `img` 元素中显示画布内容。

在本教程创建的示例中，我添加了一个按钮，该按钮可从画布动态创建图像并将其添加到页面：

```js
const img = document.createElement("img");
img.src = canvas.toDataURL("image/png");
screenshotsContainer.prepend(img);
```

## 完整示例和代码

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8" />
    <title>摄像头</title>
    <style type="text/css">
        #video {
          width: 100%;
        }

        .is-hidden {
          display: none;
        }

        .btns {
          margin-bottom: 10px;
        }

        button {
          font-size: 22px;
          padding: 8px 10px;
          border: 2px solid #ccc;
          border-radius: 10px;
        }

        .video-screenshot {
          display: grid;
          grid-template-columns: 1fr 1fr;
          grid-column-gap: 10px;
        }
    </style>
</head>
<body>
<h1>如何使用JavaScript访问设备摄像头（前后）</h1>
<div class="btns">
  <button class="button is-hidden" id="btnPlay">
    播放
  </button>
  <button class="button" id="btnPause">
    暂停
  </button>
  <button class="button" id="btnScreenshot">
    截屏
  </button>
  <button class="button" id="btnChangeCamera" style="padding: 6px 10px;">
    切换摄像头
  </button>
</div>
<div class="video-screenshot">
  <video autoplay id="video"></video>
  <div>
    <div id="screenshotsContainer">
      <canvas id="canvas" class="is-hidden"></canvas>
    </div>
  </div>
</div>
<script type="text/javascript">
// https://segmentfault.com/a/1190000022867561
window.onload = async function () {
  if (
    !"mediaDevices" in navigator ||
    !"getUserMedia" in navigator.mediaDevices
  ) {
    document.write('当前浏览器不支持Camera API，请更新浏览器。')
    return;
  }

  const video = document.querySelector("#video");
  const canvas = document.querySelector("#canvas");
  const screenshotsContainer = document.querySelector("#screenshotsContainer");
  let videoStream = null
  let useFrontCamera = true; // 前置摄像头
  const constraints = {
    video: {
      width: {
        min: 1280,
        ideal: 1920,
        max: 2560,
      },
      height: {
        min: 720,
        ideal: 1080,
        max: 1440,
      }
    },
  };

  // play
  btnPlay.addEventListener("click", function () {
    video.play();
    btnPlay.classList.add("is-hidden");
    btnPause.classList.remove("is-hidden");
  });

  // pause
  btnPause.addEventListener("click", function () {
    video.pause();
    btnPause.classList.add("is-hidden");
    btnPlay.classList.remove("is-hidden");
  });

  // 切换摄像头
  btnChangeCamera.addEventListener("click", function () {
    useFrontCamera = !useFrontCamera;
    init();
  });


  // 停止视频流
  function stopVideoStream() {
    if (videoStream) {
      videoStream.getTracks().forEach((track) => {
        track.stop();
      });
    }
  }

  // 截图
  btnScreenshot.addEventListener("click", function () {
    let img = document.getElementById('screenshot');
    if (!img) {
      img = document.createElement("img");
      img.id = 'screenshot';
      img.style.width = '100%';
    }
    canvas.width = video.videoWidth;
    canvas.height = video.videoHeight;
    canvas.getContext("2d").drawImage(video, 0, 0);
    img.src = canvas.toDataURL("image/png");
    screenshotsContainer.prepend(img);
  });

  async function init() {
    stopVideoStream();
    constraints.video.facingMode = useFrontCamera ? "user" : "environment";
    try {
      videoStream = await navigator.mediaDevices.getUserMedia(constraints);
      video.srcObject = videoStream;
    } catch (error) {
      console.log(error)
    }
  }
  init();
}
</script>
</body>
</html>
```

