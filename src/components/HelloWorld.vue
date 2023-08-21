<template>
  <NLayout content-style="min-height: 100vh">
    <NLayoutHeader bordered>
      <div class="p-4">
        <div class="flex items-center">
          <div>
            <img alt="Naive logo" src="../assets/logo.svg" width="25" height="25" />
          </div>
          <div class="ml-4">
            <NGradientText size="20"> 一个简单的 Naive UI + TailwindCSS 的模板 </NGradientText>
          </div>
        </div>
      </div>
    </NLayoutHeader>
    <NLayoutContent content-style="display: flex;justify-content: center;">
      <div id="container" class="flex justify-center flex-col p-2">
        <NButton @click="handleDemo">demo</NButton>
        <div id="demo">
          <NGradientText size="30"> 一个简单的 Naive UI + TailwindCSS 的模板 </NGradientText>
          <NH2 prefix="bar">
            <NText type="primary"> 说明 </NText>
          </NH2>
          <NP>
            一个本人很喜欢的技术栈，Naive UI + TailwindCSS，这个模板可以让你快速开始一个项目。
            <br />
            使用unplugin-auto-import + unplugin-vue-components技术加持，自动导入不费力。
            <br />
            <NText delete> PS：只是方便自己不用每次都搭建一次而已。 </NText>
          </NP>
          <div id="vs"></div>
        </div>
      </div>
    </NLayoutContent>
    <NLayoutFooter position="absolute">
      <div class="flex justify-center">
        <NButton
          text
          tag="a"
          href="https://github.com/lvzhenbo/naive-tailwind-template"
          target="_blank"
        >
          GitHub
        </NButton>
      </div>
    </NLayoutFooter>
  </NLayout>
</template>

<script setup lang="ts">
  import Player from 'xgplayer';
  import 'xgplayer/dist/index.min.css';
  onMounted(() => {
    new Player({
      id: 'vs',
      url: 'http://s2.pstatp.com/cdn/expire-1-M/byted-player-videos/1.0.0/xgplayer-demo.mp4',
      lang: 'zh-cn',
    });
  });
  const handleDemo = async () => {
    if (documentPictureInPicture.window) {
      documentPictureInPicture.window.close();
      return;
    }

    const pipWindow = await documentPictureInPicture.requestWindow();
    [...document.styleSheets].forEach((styleSheet) => {
      try {
        const cssRules = [...styleSheet.cssRules].map((rule) => rule.cssText).join('');
        const style = document.createElement('style');

        style.textContent = cssRules;
        pipWindow.document.head.appendChild(style);
      } catch (e) {
        const link = document.createElement('link');

        link.rel = 'stylesheet';
        link.type = styleSheet.type;
        link.media = styleSheet.media;
        link.href = styleSheet.href;
        pipWindow.document.head.appendChild(link);
      }
    });
    const player = document.querySelector('#demo');

    pipWindow.document.body.append(player);
    pipWindow.addEventListener('pagehide', (event) => {
      const container = document.querySelector('#container');
      const pipVideo = event.target.querySelector('#demo');
      pipVideo.classList.toggle('fullpage', false);
      container?.append(pipVideo);
    });
  };
</script>

<style scoped></style>
