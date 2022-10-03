<template>
  <div
    class="md:right-12 md:mx-0 mx-4 top-6 p-7 max-h-[700px] overflow-y-auto fixed flex flex-col max-w-lg bg-offWhite rounded-xl shadow-lg opacity-90"
  >
    <div class="flex gap-5">
      <div>
        <!-- :src="`https://ui-avatars.com/api/?background=000&color=fff&name=${
            user.properties.title.split(' ')[0]
          }+${user.properties.title.split(' ')[1]}`" -->
        <img
          :src="user.photo"
          alt=""
          class="w-20 rounded-full cursor-pointer"
        />
      </div>
      <div class="flex flex-col">
        <span class="font-mulish text-xl font-semibold cursor-pointer">{{
          user.fullName
        }}</span>
        <span class="font-lato"
          >{{ user.designation }} at {{ user.companyName }}</span
        >
        <div class="flex items-center gap-2 mt-1 cursor-pointer">
          <div class="flex items-center w-6">
            <img
              src="https://cdn-icons-png.flaticon.com/128/2113/2113178.png"
              alt=""
              class="w-full"
            />
          </div>

          <span class="font-lato text-sm"
            >{{ user.location.city }}, {{ user.location.country }}</span
          >
        </div>
      </div>
    </div>
    <div class="flex flex-col mt-5">
      <span class="font-mulish text-lg font-semibold">About Me</span
      ><span class="font-lato mt-2"
        >Travel fanatic. Creator. Troublemaker. Friend of animals everywhere.
        Food expert. Falls down a lot.</span
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
