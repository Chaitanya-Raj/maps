<template>
  <div
    class="py-2 px-4 bg-[#f8f8f8] flex justify-between items-center rounded-lg opacity-90 gap-6 shadow-lg flex-row-reverse"
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
