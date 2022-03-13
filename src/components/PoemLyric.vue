<template>
  <div class="flex flex-col justify-center w-11/12 h-full">
    <div class="flex justify-between">
      <n-input
        clearable
        class="mr-8"
        v-model:value="keywords"
        type="text"
        placeholder="请输入关键词"
        :on-clear="handleClear"
      />
      <n-button-group>
        <n-button
          type="success"
          round
          :disabled="disabled"
          :loading="loading1"
          strong
          secondary
          @click="getPoemLyric('poem-')"
        >
          <template #icon>
            <n-icon><leaf-outline /></n-icon>
          </template>
          Poem
        </n-button>
        <n-button
          type="success"
          round
          strong
          secondary
          :disabled="disabled"
          :loading="loading2"
          @click="getPoemLyric('lyric-')"
        >
          <template #icon>
            <n-icon><moon-outline /></n-icon>
          </template>
          Lyric
        </n-button>
      </n-button-group>
    </div>
    <div class="flex justify-center">
      <div class="w-full h-64 p-4 my-10 font-bold leading-8 tracking-wide border border-gray-200 rounded-lg text-ellipsis">
        {{ poemLyric }}
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { LeafOutline, MoonOutline } from "@vicons/ionicons5";
import { NInput, NButton, NIcon, NButtonGroup } from "naive-ui";
import { ref } from "vue";

import axios from "axios";

const api = "api/poem_and_lyric";

const loading1 = ref<boolean>(false);
const loading2 = ref<boolean>(false);

const disabled = ref<boolean>(false);

const keywords = ref<string>("");

const poemLyric = ref<String>("");

const handleClear = () => {
  poemLyric.value = ''
}

async function getPoemLyric(prefix: String) {
  try {
    if (prefix === 'poem-') {
      loading1.value = true;
    } else {
      loading2.value = true;
    }
    disabled.value = true;
    const result: any = await axios.post(api, {
      text: prefix + keywords.value.toString(),
    });
    poemLyric.value = result.data.text;
    console.log(result.data.text);
    if (prefix === 'poem-') {
      loading1.value = false;
    } else {
      loading2.value = false;
    }
    disabled.value = false;
  } catch (err) {
    if (prefix === 'poem-') {
      loading1.value = false;
    } else {
      loading2.value = false;
    }
    disabled.value = false;
    poemLyric.value = "请重新尝试！";
    console.log(err);
  }
}
</script>
