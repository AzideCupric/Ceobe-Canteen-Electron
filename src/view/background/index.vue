<template>
    <div class="background">
    </div>
</template>

<script setup name="index">
import { onMounted, onBeforeMount } from "vue";
import { backgroundInit } from "@/utils/backgroundUtil"

onBeforeMount(() => {
    backgroundInit().then(() => {})
})

// onMounted(() => {
//     window.newestTimeline.getTimeline((_, args) => {
//         // 接收到的是详细的饼列表
//         let cookies_len = args.length;
//         let count = 0;
//         args.forEach(data => {
//             count++;
//             window.ongotpointercapture.openNotificationWindow(data);
//             if (count !== cookies_len) {
//                 setTimeout(() => {
//                     window.notification.closeWindow();
//                 }, 10000)
//             }
//         });
//     })
// })
</script>