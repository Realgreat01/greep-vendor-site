<template>
  <div>
    <div class="my-5 flex items-center justify-between">
      <div class="flex text-sm">
        {{ gpDates.formatDateTime(realTimeDate) }}
      </div>
      <div class="flex flex-col items-end">
        <div class="flex items-center space-x-2">
          <Switch class="data-[state=checked]:bg-[#10BB76]" id="availablity" />
          <Label for="availablity" class="text-sm font-normal leading-[20px]"
            >Enable Delivery Charge</Label
          >
        </div>
        <h2 class="text-xs font-medium text-[#10BB76]">
          {{ gpNumbers.formatCurrency(50, "TRY") }} Delivery Charges
        </h2>
      </div>
    </div>
    <form class="space-y-4" @submit="onSubmit">
      <FormField v-slot="{ componentField }" name="recepientName">
        <FormItem>
          <FormLabel class="-mb-2 font-normal">
            Customers Name
            <span class="font-normal text-red-400">*</span>
          </FormLabel>
          <FormControl>
            <Input placeholder="John Doe" v-bind="componentField" />
          </FormControl>
          <FormMessage />
        </FormItem>
      </FormField>

      <FormField v-slot="{ componentField }" name="to">
        <FormItem>
          <FormLabel class="-mb-2 font-normal"
            >Customer Address
            <span class="text-sm text-[#FF5656]">*</span></FormLabel
          >
          <FormControl>
            <LocationPicker
              type="text"
              placeholder="Hamiltkoy"
              class="text-sm"
              v-bind="componentField"
              v-model="location"
            />
          </FormControl>
          <FormMessage />
        </FormItem>
      </FormField>

      <FormField v-slot="{ componentField }" name="recepientPhoneNumber">
        <FormItem>
          <FormLabel class="-mb-2 font-normal">
            Customers Phone Number
            <span class="font-normal text-red-400">*</span>
          </FormLabel>
          <FormControl>
            <PhoneNumber v-bind="componentField" />
          </FormControl>
          <FormMessage />
        </FormItem>
      </FormField>

      <FormField v-slot="{ componentField }" name="streetName">
        <FormItem>
          <FormLabel class="-mb-2 font-normal">
            Street Name
            <span class="font-normal text-red-400">*</span>
          </FormLabel>
          <FormControl>
            <Input placeholder="Lafkasa Street" v-bind="componentField" />
          </FormControl>
          <FormMessage />
        </FormItem>
      </FormField>

      <div class="grid grid-cols-2 gap-4">
        <FormField v-slot="{ componentField }" name="buildingName">
          <FormItem>
            <FormLabel class="-mb-2 font-normal">
              Building Name
              <span class="font-normal text-red-400">*</span>
            </FormLabel>
            <FormControl>
              <Input placeholder="Lirakj Home" v-bind="componentField" />
            </FormControl>
            <FormMessage />
          </FormItem>
        </FormField>
        <FormField v-slot="{ componentField }" name="cityName">
          <FormItem>
            <FormLabel class="-mb-2 font-normal">
              City
              <span class="font-normal text-red-400">*</span>
            </FormLabel>
            <FormControl>
              <Input placeholder="Hamiltoy" v-bind="componentField" />
            </FormControl>
            <FormMessage />
          </FormItem>
        </FormField>
        <FormField v-slot="{ componentField }" name="flat">
          <FormItem>
            <FormLabel class="-mb-2 font-normal">
              Flat
              <span class="font-normal text-red-400">*</span>
            </FormLabel>
            <FormControl>
              <Input placeholder="" v-bind="componentField" />
            </FormControl>
            <FormMessage />
          </FormItem>
        </FormField>
        <FormField v-slot="{ componentField }" name="doorNo">
          <FormItem>
            <FormLabel class="-mb-2 font-normal">
              Door No
              <span class="font-normal text-red-400">*</span>
            </FormLabel>
            <FormControl>
              <Input placeholder="42" v-bind="componentField" />
            </FormControl>
            <FormMessage />
          </FormItem>
        </FormField>
        <FormField v-slot="{ componentField }" name="amount">
          <FormItem>
            <FormLabel class="-mb-2 font-normal">
              Amount
              <span class="font-normal text-red-400">*</span>
            </FormLabel>
            <FormControl>
              <CurrencyPicker placeholder="4000" v-bind="componentField" />
            </FormControl>
            <FormMessage />
          </FormItem>
        </FormField>
        <FormField v-slot="{ componentField }" name="paymentMethod">
          <FormItem>
            <FormLabel class="-mb-2 font-normal">
              Payment Method
              <span class="font-normal text-red-400">*</span>
            </FormLabel>
            <FormControl>
              <Input placeholder="Enter Text Here..." v-bind="componentField" />
            </FormControl>
            <FormMessage />
          </FormItem>
        </FormField>
        <FormField v-slot="{ componentField }" name="deliveryType">
          <FormItem>
            <FormLabel class="-mb-2 font-normal">
              Delivery Type
              <span class="font-normal text-red-400">*</span>
            </FormLabel>
            <FormControl>
              <Input placeholder="4000" v-bind="componentField" />
            </FormControl>
            <FormMessage />
          </FormItem>
        </FormField>
        <FormField v-slot="{ componentField }" name="size">
          <FormItem>
            <FormLabel class="-mb-2 font-normal">
              Size
              <span class="font-normal text-red-400">*</span>
            </FormLabel>
            <FormControl>
              <Input placeholder="1kg" v-bind="componentField" />
            </FormControl>
            <FormMessage />
          </FormItem>
        </FormField>
      </div>
      <FormField v-slot="{ componentField }" name="dropOffNote">
        <FormItem>
          <FormLabel class="-mb-2 font-normal"> Message (optional) </FormLabel>
          <FormControl>
            <Textarea
              placeholder="Enter Text Here..."
              v-bind="componentField"
            />
          </FormControl>
          <FormMessage />
        </FormItem>
      </FormField>

      <Button block class="flex h-12 w-full items-center gap-x-2">
        <img src="/images/icons/rider.svg" alt="" /> Book a Ride</Button
      >
    </form>
  </div>
</template>

<script setup lang="ts">
import { toTypedSchema } from "@vee-validate/zod";
import { useForm } from "vee-validate";
import * as z from "zod";

const props = defineProps({
  vendorType: {
    type: String as PropType<"vendorFoods" | "vendorItems">,
    required: true,
  },
});

const emit = defineEmits(["completed", "close"]);

const realTimeDate = ref(new Date());

watch(realTimeDate, (currentTime) => {
  realTimeDate.value = currentTime;
});

interface Location {
  name: string;
  city: string;
  state: string;
  country: string;
  latitude: number;
  longitude: number;
}

const location = ref<Location>();

const isDialogOpen = ref(false);

const closeModal = () => emit("completed");

const formSchema = toTypedSchema(
  z.object({
    recepientName: z.string({ message: "Customer name is required" }),
    recepientPhone: z.string({ message: "Customer phone number is required" }),
    streetName: z.string({ message: "Customer street name is required" }),
    buildingName: z.string({ message: "Customer building name is required" }),
    cityName: z.string({ message: "Customer city name is required" }),
    flat: z.string({ message: "Customer flat name is required" }),
    doorNo: z.number({ message: "Customer door number is required" }),
    amount: z.number({ message: "Order amount is required" }),
    paymentMethod: z.string({ message: "Payment method is required" }),
    dropOffNote: z.string().optional(),
    to: z.any({
      message: "Customer address cannot be empty",
    }),
  }),
);

const { handleSubmit, setFieldValue, resetForm } = useForm({
  validationSchema: formSchema,
  initialValues: { dropOffNote: "Drop in front of my house" },
});

const onSubmit = handleSubmit(async (values: any) => {
  const payload = {
    ...values,
  };
});
</script>

<style scoped></style>
