<template>
  <div class="info-and-tool">
    <!--常用信息-->
    <weekly-quest></weekly-quest>
    <!--公告-->
    <announcement></announcement>
    <!--广告-->
    <!--发源地-->
    <tool-or-source title="饼的发源地" :list="source.list" ></tool-or-source>
    <!--快捷工具-->
    <tool-or-source title="快捷工具" :list="tool.list" ></tool-or-source>
    <!--推荐视频-->
    <video-list></video-list>
    <div class="d-flex justify-center ma-2">
      Powered By 蓝芷怡 & 洛梧藤 & 云闪
    </div>
  </div>
</template>

<script setup name="infoAndTool">
import {reactive} from "vue";
import {changeToCCT} from "@/utils/timeUtil";
import Announcement from "@/components/InfoAndTool/announcement";
import WeeklyQuest from "@/components/InfoAndTool/weeklyQuest";
import ToolOrSource from "@/components/InfoAndTool/toolOrSource";
import {sourceInfo, toolInfo} from "@/constant";
import VideoList from "@/components/InfoAndTool/videoList";

const source = reactive({
  list: sourceInfo
})

const tool = reactive({
  list: toolInfo
})

</script>

<style rel="stylesheet/scss" lang="scss">
.info-and-tool {
  user-select: none;
  min-width: 600px;
  height: 100vh;
  overflow: auto;
  padding: 10px 16px 0;
}
</style>