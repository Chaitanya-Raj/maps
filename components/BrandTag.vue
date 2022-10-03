<template>
  <div
    class="bg-offWhite opacity-90 flex items-center px-3 py-1 rounded-lg shadow-lg"
  >
    <p class="font-lora text-gray-600">Powered By</p>
    <a href="https://magic.app/">
      <magik-logo class="invert h-5 -mx-3" />
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
