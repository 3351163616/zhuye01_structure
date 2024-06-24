<template>
  <div class="more-content">
    <div v-for="item in upData.new" :key="item.url" class="content-item">
      <span class="description" v-html="item.description"></span>
      <a :href="item.url" target="_blank" class="link">{{ item.url }}</a>
    </div>
    <div v-for="item in upData.fix" :key="item.url" class="content-item">
      <span class="description">{{ item.description }}</span>
      <a :href="item.url" target="_blank" class="link">{{ item.url }}</a>
    </div>
  </div>
</template>

<script setup>
import { reactive } from 'vue';

const upData = reactive({
  new: [
    { description: "壁纸由以下Api接口提供" },
    { description: "栗次元随机动漫", url: "https://t.alcy.cc/ycy" },
    { description: "微软必应每日", url: "https://api.dujin.org/bing/1920.php" },
    { description: "随机风景", url: "https://api.vvhan.com/api/wallpaper/views" },
    { description: "天气由以下Api接口提供" },
    { description: "韩小韩天气Api接口", url: "https://api.vvhan.com/api/weather" },
    { description: "音乐接口为作者自行部署", url: "https://3351163616-meting-api-34.deno.dev/" },
    { description: "音乐接口调用前联系作者Q <a href='https://qm.qq.com/q/qdQNoh7ewM' class='link' style='color: blue;'>3351163616</a>" }
  ],
  fix: [
    // 你可以在这里添加你的修复条目
  ]
});
</script>

<style lang="scss" scoped>
.more-content {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  margin-top: 20px;
  width: 100%;
  height: 100%;
  overflow: auto;

  .content-item {
    margin-bottom: 20px;
    padding: 10px;
    width: 90%;

    .link {
      color: blue;
      text-decoration: none;

      &:hover {
        text-decoration: underline;
      }
    }

    .description {
      margin-right: 10px;

      .link {
        color: blue;
        text-decoration: none;

        &:hover {
          text-decoration: underline;
        }
      }
    }
  }
}
</style>
