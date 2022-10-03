<template>
  <div
    class="bg-offWhite opacity-90 flex flex-row-reverse items-center justify-between gap-6 px-4 py-2 rounded-lg shadow-lg"
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
