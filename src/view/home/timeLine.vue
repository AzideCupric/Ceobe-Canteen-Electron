<template>
  <div class="time-line">
    <v-timeline
        align="start"
        side="end"
        truncate-line="start"
    >
      <v-timeline-item
          v-for="item in home.timeLineData"
          :key="item.id"
          :left="true"
          fill-dot="fill-dot"
          dot-color="#fff"
          size="50"
      >
        <template v-slot:icon>
          <v-avatar :image="getImage(item.parent.img)"></v-avatar>
        </template>
        <component :is="component.getComponentName(item)" :id="item.id" :info="item" @openUrl="card.openUrlInThis">
          <template #default="info" v-if="card.isCopyImage && item.id == card.copyImageId">
            <div class="h-100 w-100 d-flex flex-column">
              <v-divider class="my-2"></v-divider>
              <div class="h-100 w-100 d-flex justify-space-between align-center print px-2">
                <div class="d-flex flex-column">
                  <div class="font-weight-bold title">{{ info.info.dataSource }}</div>
                  <div class="font-weight-light subtitle">{{ info.info.timeForDisplay }}</div>
                </div>
                <div class="d-flex align-center">
                  <img :src="getImage('/assets/image/logo/icon.png')" width="35">
                  <v-divider class="mx-2" vertical></v-divider>
                  <div>
                    <div class="font-weight-bold title">小刻终于吃到饼啦！</div>
                    <div class="font-weight-light subtitle">分享来自小刻食堂</div>
                  </div>
                </div>
              </div>
            </div>
          </template>
          <template #default="info" v-else>
            <span class="font-weight-bold pl-2">{{ info.info.timeForDisplay }}</span>
            <v-spacer></v-spacer>
            <v-btn size="small" icon="fas fa-copy" title="复制链接" @click.stop="card.copy(info.info.jumpUrl)"></v-btn>
            <v-btn size="small" icon="fas fa-share-nodes" title="生成卡片"
                   @click.stop="card.copyImage(item.id)"></v-btn>
            <v-btn size="small" icon="fas fa-link" title="使用浏览器打开"
                   @click.stop="card.openUrlInBrowser(info.info.jumpUrl)"></v-btn>
          </template>

        </component>
      </v-timeline-item>
    </v-timeline>
  </div>
</template>

<script setup name="timeLine">
import {getCurrentInstance, nextTick, onMounted, reactive} from "vue";
import {sourceInfo} from "@/constant"
import {getImage} from "@/utils/imageUtil"
import Music from "@/components/Card/music"
import Info from "@/components/Card/common"
import Terra from "@/components/Card/terra"
import {useRouter} from "vue-router";
import * as htmlToImage from "html-to-image";

const router = useRouter();
const {proxy} = getCurrentInstance();

// 卡片数据
const home = reactive({
  data: [],
  timeLineData: [],
  getData() {
    window.ceobeRequest.getCardList().then(res => {
      let data = res.data.data;
      // data = data["泰拉记事社官网"]
      home.timeLineData = Object.values(data).flat().sort((x, y) => y.timeForSort - x.timeForSort);
      home.timeLineData.forEach(item => {
        item.parent = sourceInfo.find(x => x.name == item.dataSource)
      })
    })
  }
});

// 卡片操作
const card = reactive({
  isCopyImage: false,// 当前是否在截图
  copyImageId: null,
  openUrlInThis(data) {
    router.push({
      path: '/home/Browser',
      query: data
    })

    // window.operate.openNotificationWindow({
    //   dataSource:home.timeLineData[0].dataSource,
    //   coverImage:home.timeLineData[0].coverImage,
    //   content:home.timeLineData[0].content,
    //   timeForDisplay:home.timeLineData[0].timeForDisplay});
  },
  copyImage(id) {
    card.copyImageId = id;
    card.isCopyImage = true;

    nextTick(() => {
      htmlToImage.toJpeg(document.getElementById(id), {quality: 0.95})
          .then(function (dataUrl) {
            card.isCopyImage = false;
            window.operate.copy({type: 'img', data: dataUrl})
          });
    })
  },
  copy(url) {
    window.operate.copy({type: 'text', data: url})
  },
  openUrlInBrowser(url) {
    window.operate.openUrlInBrowser(url)
  }
})

const component = reactive({
  getComponentName(item) {
    if (item.dataSource == "塞壬唱片网易云音乐") {
      return Music
    } else if (item.dataSource == "泰拉记事社官网") {
      return Terra
    } else {
      return Info
    }
  }
})
onMounted(() => {
  home.getData();
})
</script>

<style rel="stylesheet/scss" lang="scss">
.time-line {
  height: 100vh;
  overflow: auto;
  min-width: 500px;

  .v-timeline {
    grid-template-columns: 8px min-content 8px;

    .v-timeline-item {
      .v-timeline-item__body {
        -webkit-padding-start: 8px !important;
        padding-inline-start: 8px !important;

        .print {
          .title {
            font-size: 16px;
          }

          .subtitle {
            font-size: 12px;
            opacity: var(--v-medium-emphasis-opacity);
          }
        }

        .v-card-title {
          text-shadow: 0 0 5px black;
        }
      }
    }
  }

}
</style>
