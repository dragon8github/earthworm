<template>
  <div class="mt-8 flex w-full justify-between">
    <!-- 左侧头像区域 -->
    <div class="mr-16 hidden w-72 md:block">
      <div
        class="mx-auto h-56 w-56 overflow-hidden rounded-full border-2 border-gray-300 bg-gray-300 dark:border-gray-700 dark:bg-gray-700"
      >
        <!-- 通过给定高度来自适应拉伸图片，如果图片不存在或者加载失败则显示外层的背景色（没有宽度） -->
        <img
          class="h-full object-cover"
          :src="userStore.user?.avatar"
        />
      </div>
      <div class="mt-4 truncate">
        <div class="flex gap-2">
          <div class="text-3xl font-medium">{{ userStore.user?.username }}</div>
          <span
            v-if="userStore.isFounderMembership()"
            class="i-ph-crown-simple-fill relative overflow-hidden bg-yellow-400"
            title="尊贵的创始会员,感谢您对 Earthworm 的大力支持！"
          >
            <div class="glimmer"></div>
          </span>
        </div>
        <div class="text-md text-gray-400">
          {{ userStore.user?.name }}
        </div>
      </div>
      <hr class="my-5 dark:border-gray-700" />
      <!-- TODO: 等后续勋章制作完成再放出来 -->
      <!-- <div class="text-lg font-medium">勋章</div>
      <div class="mt-2 grid grid-cols-4 gap-2">
        <div
          v-for="i in 6"
          class="h-16 w-16 rounded-full bg-gray-200 dark:bg-gray-700"
        ></div>
      </div> -->
    </div>

    <!-- 右侧课程包区域 -->
    <div class="min-w-0 flex-1">
      <div class="mb-4 flex justify-between border-b pb-2 dark:border-gray-700">
        <div class="text-xl font-medium">最近使用的课程包</div>
        <NuxtLink
          href="/course-pack"
          class="link text-blue-500 no-underline hover:opacity-75"
          >更多课程包
        </NuxtLink>
      </div>
      <HomeRecentCoursePack />
      <HomeCalendarGraph
        class="mt-10"
        :data="learnRecord.list"
        :totalCount="learnRecord.totalCount"
        @toggleYear="toggleYear"
      />
    </div>
  </div>
</template>
<script setup lang="ts">
import { ref } from "vue";

import { useLearnRecord } from "~/composables/learnRecord";
import { type CalendarData } from "~/composables/user/calendarGraph";
import { useUserStore } from "~/store/user";

const userStore = useUserStore();
const { learnRecord, setupLearnRecord, setQueryYear } = useLearnRecord();
const { toggleYear } = useCalendarGraph();

function useCalendarGraph() {
  const data = ref<CalendarData[]>([]);
  const totalCount = ref<number>(0);

  async function toggleYear(year?: number) {
    setQueryYear(year);
    setupLearnRecord();
  }

  return {
    data,
    totalCount,
    toggleYear,
  };
}
</script>

<style scoped>
.glimmer {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(
    90deg,
    rgba(255, 255, 255, 0) 0%,
    rgba(139, 0, 0, 0.5) 50%,
    rgba(255, 255, 255, 0) 100%
  );
  animation: glimmer 2s infinite;
}

@keyframes glimmer {
  0% {
    transform: translateX(-100%);
  }
  100% {
    transform: translateX(100%);
  }
}

/* 添加浏览器前缀 做浏览器的兼容 */
@-webkit-keyframes glimmer {
  0% {
    -webkit-transform: translateX(-100%);
  }
  100% {
    -webkit-transform: translateX(100%);
  }
}

.glimmer {
  -webkit-animation: glimmer 2s infinite;
}
</style>
