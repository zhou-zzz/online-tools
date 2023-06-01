<script setup ts>
const router = useRouter()

function back() {
  router.push('/')
}
</script>

<template>
  <nav mt-6 inline-flex gap-2 text-xl>
    <button icon-btn title="回到首页" @click="back()">
      <div i-carbon-home />
    </button>
    <button icon-btn @click="toggleDark()">
      <div i-carbon-sun dark:i-carbon-moon />
    </button>

    <a
      i-carbon-logo-github icon-btn
      rel="noreferrer"
      href="https://github.com/antfu/vitesse-lite"
      target="_blank"
      title="GitHub"
    />
  </nav>
</template>
