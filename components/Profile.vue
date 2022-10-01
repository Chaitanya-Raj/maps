<template>
  <div
    class="right-12 top-6 p-7 max-h-[700px] overflow-y-auto fixed flex flex-col max-w-lg bg-[#f8f8f8] rounded-xl shadow-lg"
  >
    <div class="flex gap-5">
      <div>
        <!-- :src="`https://ui-avatars.com/api/?background=000&color=fff&name=${
            user.properties.title.split(' ')[0]
          }+${user.properties.title.split(' ')[1]}`" -->
        <img :src="user.properties.logo" alt="" class="w-20 rounded-full" />
      </div>
      <div class="flex flex-col">
        <span class="font-mulish text-xl font-semibold">{{
          user.properties.name
        }}</span>
        <span class="font-lato"
          >{{ user.properties.designation }} at
          {{ user.properties.company }}</span
        >
        <span class="font-lato mt-2 text-sm">{{
          user.properties.location
        }}</span>
      </div>
    </div>
    <div class="flex flex-col mt-5">
      <span class="font-mulish text-lg font-semibold">About Me</span
      ><span class="font-lato mt-2"
        >Lorem ipsum dolor sit amet consectetur adipisicing elit. Excepturi
        saepe veritatis velit cum, error porro blanditiis. Amet sapiente fugit
        nihil laboriosam earum modi nobis ipsa incidunt eum culpa? Sunt,
        ea?</span
      >
    </div>
  </div>
</template>

<script>
export default {
  props: {
    user: {
      type: Object,
      default: () => {},
    },
  },
};
</script>

<style></style>
