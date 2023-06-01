<script setup lang="ts" generic="T extends any, O extends any">
defineOptions({
  name: 'IndexPage',
})
</script>

<template>
  <div class="mx-auto max-w-300">
    <div class="mx-auto max-w-500 w-max py-2" grid="~ cols-1 gap-10">
      <RouterLink to="/tools/ToType" flex="~  items-center gap-4">
        <div i-file-icons-test-typescript text-3xl />
        <div flex-auto text-left>
          <div>ToType</div>
          <div text-sm opacity-50>
            Convert Java Swagger Models To TypeScript type
          </div>
        </div>
      </RouterLink>
    </div>
  </div>
</template>
