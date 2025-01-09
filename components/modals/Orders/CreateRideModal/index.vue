<template>
  <CustomModal title="Book Ride" @close="emit('completed')">
    <CreateRideOrderForm :vendorType @completed="emit('completed')" />
  </CustomModal>
</template>

<script setup lang="ts">
defineProps({
  vendorType: {
    type: String as PropType<"vendorFoods" | "vendorItems">,
    required: true,
  },
});

const emit = defineEmits(["close", "completed"]);
</script>

<style></style>
