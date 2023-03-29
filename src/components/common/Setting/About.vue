<script setup lang='ts'>
import { computed, onMounted, ref } from 'vue'
import { NSpin } from 'naive-ui'
import { fetchChatConfig } from '@/api'
import pkg from '@/../package.json'
import { useAuthStore } from '@/store'

interface ConfigState {
  timeoutMs?: number
  apiModel?: string
  reverseProxy?: string

}

const authStore = useAuthStore()

const loading = ref(false)

const config = ref<ConfigState>()

const isChatGPTAPI = computed<boolean>(() => !!authStore.isChatGPTAPI)

async function fetchConfig() {
  try {
    loading.value = true
    const { data } = await fetchChatConfig<ConfigState>()
    config.value = data
  }
  finally {
    loading.value = false
  }
}

onMounted(() => {
  fetchConfig()
})
</script>

<template>
  <NSpin :show="loading">
    <div class="p-4 space-y-4">
      <h2 class="text-xl font-bold">
      
			Version - {{ pkg.version }}
      </h2>
		<h2 class="text-xl font-bold">
      
		客服微信：wxid_4ta1x11bv57v12
      </h2>
			
      <div class="p-2 space-y-2 rounded-md bg-neutral-100 dark:bg-neutral-700">
        <p>
          超级一休-言之有理，内测阶段限时免费中，PC端和手机端自适应响应！
        </p>
        <p>
          超级一休，让你轻松享受AI科技带来的智能体验！不仅可以帮你快速准确写出论文、律师函、直播话术、作文、代码，还可以秒写演讲稿、工作报告、入职申请等，让你的工作变得更加轻松高效！
					它将彻底改变我们的生活习惯，让你的生活更加轻松，让你的工作变得更加高效！让我们一起感受超级一休给我们带来的新鲜体验吧！
      </p>
 </div>
      <p>{{ $t("setting.api") }}：{{ config?.apiModel ?? '-' }}</p>
      <p v-if="!isChatGPTAPI">
        {{ $t("setting.reverseProxy") }}：{{ config?.reverseProxy ?? '-' }}
      </p>
    </div>
  </NSpin>
</template>
