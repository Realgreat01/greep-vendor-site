<template>
  <vue-tel-input
    v-bind="{ ...telProps }"
    class="!focus-visible:outline-none !focus-visible:ring-2 !focus-within:border-[#000] !focus-visible:ring-ring !focus-visible:ring-offset-2 !rounded-md !border !border-input py-1 !shadow-none !ring-offset-background"
    @validate="showOptions"
  ></vue-tel-input>
</template>

<script setup lang="ts">
const phoneOptions = defineModel<any>({ default: null });

const showOptions = (opts: any) => {
  phoneOptions.value = opts;
};

const telProps = {
  preferredCountries: ["TR", "NG"],
  placeholder: "Enter your phone number",
  mode: "international",
  inputOptions: {
    showDialCode: true,
  },
  //   autoDefaultCountry: true,
  dropdownOptions: {
    showSearchBox: true,
    showFlags: true,
    searchBoxPlaceholder: "Search for country",
  },
};
</script>

<style scoped></style>
