<script setup lang='ts'>
import { onMounted, ref } from 'vue'
import { NSpin } from 'naive-ui'
import { fetchChatConfig } from '@/api'
import pkg from '@/../package.json'

interface ConfigState {
  timeoutMs?: number
  reverseProxy?: string
  apiModel?: string
  socksProxy?: string
  httpsProxy?: string
}

const loading = ref(false)

const config = ref<ConfigState>()

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
      <div class="p-2 space-y-2 rounded-md bg-neutral-100 dark:bg-neutral-700">
        <p>
          本站基于开源项目
          <a
            class="text-blue-600 dark:text-blue-500"
            href="https://github.com/Chanzhaoyu/chatgpt-web"
            target="_blank"
          >
            chatgpt-web
          </a>
          ，目前由站长Azad本人自费维护
        </p>
        <p>
          网站完全免费， <a
            class="text-blue-600 dark:text-blue-500"
            href="http://www.azad.asia/dashang/"
            target="_blank"
          >
            所得赞助
          </a>都将用于网站维护。
        </p>
				<p>
					欢迎光顾 <a
            class="text-blue-600 dark:text-blue-500"
            href="https://faka.azad.asia/"
            target="_blank"
          >
           🛒 我的小店
          </a>。
			   </p>
      </div>
      <p>{{ $t("setting.api") }}：{{ config?.apiModel ?? '-' }}</p>
      <p>{{ $t("setting.reverseProxy") }}：{{ config?.reverseProxy ?? '-' }}</p>
      <p>{{ $t("setting.timeout") }}：{{ config?.timeoutMs ?? '-' }}</p>
      <p>{{ $t("setting.socks") }}：{{ config?.socksProxy ?? '-' }}</p>
      <p>{{ $t("setting.httpsProxy") }}：{{ config?.httpsProxy ?? '-' }}</p>
    </div>
  </NSpin>
</template>
