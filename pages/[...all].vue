<script setup lang="ts">
import { callWithNuxt } from '#app';

const config = useAppConfig();
const app = useNuxtApp();

const { page, layout } = useContent();

function getSEOTitle(): string {
  const baseTitle = page?.value?.title || config?.site?.name || "Missing Title";
  const route = useRoute();
  const path = route.path;

  // For directory entries, add compelling suffix
  if (path.startsWith('/dir/') && page?.value?.title) {
    return `${baseTitle} Reviews & Alternatives`;
  }

  return baseTitle;
}

function getSEODescription(): string {
  return page?.value?.description || config.site.description || "Missing Description"
}

callWithNuxt(app, useSeoMeta, [{ title: getSEOTitle(), description: getSEODescription() }]);

if (page.value) {
  defineOgImage({
    component: page.value.ogImageStyle || 'Custom',
    cover: page.value.cover || page.value.card_image,
    ...(page.value.ogImage || {}),
  });
}
</script>

<template>
  <div class="document-driven-page">
    <ContentRenderer v-if="page" :key="(page as any)._id" :value="page">
      <template #empty="{ value }">
        <DocumentEmpty :value="value" />
      </template>
    </ContentRenderer>
    <DocumentNotFound v-else />
  </div>
</template>