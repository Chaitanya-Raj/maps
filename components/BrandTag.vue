<template>
  <div
    class="py-1 px-3 bg-[#f8f8f8] flex items-center rounded-lg opacity-90 shadow-lg"
  >
    <p class="font-lora text-gray-600">Powered By</p>
    <a href="https://magic.app/">
      <magik-logo class="invert h-5 -mx-4" />
    </a>
  </div>
</template>

<script>
import MagikLogo from "./MagikLogo.vue";
export default {
  components: { MagikLogo },
};
</script>

<style></style>
