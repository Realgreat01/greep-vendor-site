<template>
  <div class="-mt-2 grid grid-cols-3 gap-2">
    <FormField name="currency" class="col-span-1">
      <FormItem class="col-span-1 flex flex-col">
        <!-- <FormLabel class="mb-[6px] hidden"
          >Currency <span class="text-[#FF5656]">*</span></FormLabel
        > -->
        <Popover>
          <PopoverTrigger>
            <FormControl>
              <Button
                variant="outline"
                role="combobox"
                :class="
                  cn(
                    'w-full justify-between',
                    !values.currency && 'text-muted-foreground',
                  )
                "
              >
                {{
                  values.currency
                    ? currencies.find(
                        (currency) => currency.value === values.currency,
                      )?.label
                    : "Select currency..."
                }}
                <ChevronsUpDown class="ml-2 h-4 w-4 shrink-0 opacity-50" />
              </Button>
            </FormControl>
          </PopoverTrigger>
          <PopoverContent class="w-full p-0">
            <Command>
              <CommandInput placeholder="Search language..." />
              <CommandEmpty>Nothing found.</CommandEmpty>
              <CommandList>
                <CommandGroup>
                  <CommandItem
                    v-for="currency in currencies"
                    :key="currency.value"
                    :value="currency.label"
                    @select="
                      () => {
                        setFieldValue('currency', currency.value);
                      }
                    "
                  >
                    <Check
                      :class="
                        cn(
                          'mr-2 h-4 w-4',
                          currency.value === values.currency
                            ? 'opacity-100'
                            : 'opacity-0',
                        )
                      "
                    />
                    {{ currency.label }}
                  </CommandItem>
                </CommandGroup>
              </CommandList>
            </Command>
          </PopoverContent>
        </Popover>
        <FormMessage />
      </FormItem>
    </FormField>
    <FormField v-slot="{ componentField }" name="amount" class="col-span-2">
      <FormItem class="col-span-2">
        <FormControl>
          <Input type="number" placeholder="1" v-bind="componentField" />
        </FormControl>
        <FormMessage />
      </FormItem>
    </FormField>
  </div>
</template>

<script setup lang="ts">
import * as z from "zod";
import { Check, ChevronsUpDown } from "lucide-vue-next";
import { useForm } from "vee-validate";
import { cn } from "@/lib/utils";

const props = defineProps({
  mode: {
    type: String as PropType<"create" | "edit">,
    default: "create",
  },
  selectedProduct: {
    type: Object,
    default: () => {},
  },
});

const emits = defineEmits(["completed"]);
const openMainModal = ref<boolean>(true);

const currencies = [
  { label: "NGN", value: "NGN" },
  { label: "TRY", value: "TRY" },
];

const formSchema = ref({
  currency: z.string({
    message: "Currency is required",
  }),
  amount: z.string({
    message: "Amount is required",
  }),
});

const { handleSubmit, resetForm, setFieldValue, errors, values } = useForm({
  validationSchema: formSchema,
  initialValues: {
    currency: "TRY",
  },
});
</script>

<style scoped></style>
