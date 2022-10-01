<template>
  <div
    class="flex py-3 px-6 bg-[#f8f8f8] justify-between items-center fixed bottom-8 right-2 rounded-lg opacity-90 gap-8 shadow-lg"
  >
    <p class="font-mulish text-lg font-semibold">
      {{ formattedCount }} Members
    </p>
    <p class="font-lato text-gray-600 cursor-pointer">Show All</p>
  </div>
</template>

<script>
export default {
  props: {
    count: {
      type: Number,
      default: 0,
    },
  },
  computed: {
    formattedCount() {
      const formatter = Intl.NumberFormat("en-US");
      return formatter.format(this.count);
    },
  },
};
</script>

<style></style>
