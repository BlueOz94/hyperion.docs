<!-- Idea from: https://github.com/radix-vue/radix-vue/blob/main/docs/.vitepress/components/Annoucement.vue -->

<template>  
  <a
    v-if="frontmatter.hero.prelink && newVersionAvailable"
    :href="frontmatter.hero.prelink.link"
    :target="frontmatter.hero.prelink.target"
    class="annoucement"
  >
    {{ frontmatter.hero.prelink.title }}&nbsp;
    <span class="version_number">
      v{{ newVersionTag }}
    </span>
  </a>
</template>

<script setup lang="ts">
import { useData } from 'vitepress';
import { computed, onMounted, ref } from 'vue';

const { frontmatter } = useData();
const newVersionAvailable = ref<boolean>(false);
const newVersionTag = ref<string>('');
const expiringDays = computed<number>(() => +(frontmatter.value.hero.prelink.days || -1))

async function initialize() {
  const latestReleaseInfo = await fetchLatestReleaseInfo();
  if (!latestReleaseInfo) {
    return;
  }

  // @ts-expect-error
  const publishedDate = latestReleaseInfo.published_at;
  if (typeof publishedDate !== 'undefined') {
    const published = new Date(publishedDate);
    const now = new Date();

    const millisecondsInDay = 1000 * 60 * 60 * 24;
    const diffInDays = Math.floor((now.getTime() - published.getTime()) / millisecondsInDay);

    if (diffInDays <= expiringDays.value) {
      // @ts-expect-error
      newVersionTag.value = latestReleaseInfo.tag_name;
      newVersionAvailable.value = true;
    }
  }
}

onMounted(() => {
  initialize();
});

function fetchLatestReleaseInfo() {
  return new Promise((resolve, reject) => {
    fetch('https://api.github.com/repos/hyperion-project/hyperion.ng/releases/latest', {
      method: 'GET',
      headers: {
        'Accept': 'application/json',
      },
    })
      .then(response => {
        if (response.ok) {
          return response.json();
        }
      })
      .then(response => resolve(response))
      .catch(error => reject(error));
  });
};
</script>

<style>
.annoucement {
  display: inline-flex;
  font-weight: 600;
  line-height: 100%;
  padding: .5rem 1rem .5rem 1rem;
  margin-bottom: 0.25rem;
  border-radius: 0.5rem;
  background-color: var(--vp-sidebar-bg-color);
}

.version_number {
  vertical-align: top;
  line-height: 100%;
  font-size: 8pt;
}
</style>
