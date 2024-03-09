<template>
  <input
    type="text"
    class="form-control"
    v-model.number="value"
    :id="props.id"
    :name="props.id"
    :placeholder="props.placeholder"
    :minlength="lMinlength"
    :maxlength="props.maxlength"
    :required="required"
    @keypress="isNumber" />
</template>

<script setup>
  import { defineModel, computed } from "vue";

  const props = defineProps({
    id: {
      type: String,
      required: true,
    },
    placeholder: {
      type: String,
      required: false,
      default: "",
    },
    minlength: {
      type: Number,
      required: false,
      default: null,
    },
    maxlength: {
      type: Number,
      required: false,
      default: null,
    },
    required: {
      type: Boolean,
      required: false,
      default: false,
    },
    strict: {
      type: Boolean,
      default: false,
    },
  });
  const value = defineModel({
    type: Number,
    required: false,
    default: null,
    set(value) {
      return typeof value === "string" ? null : value;
    },
  });

  const lMinlength = computed(() => {
    return props.required ? (!props.minlength ? 1 : props.minlength) : null;
  });

  const isNumber = (e) => {
    if (props.strict) {
      return /^[0-9]*$/.test(e.key) ? true : e.preventDefault();
    } else {
      return /^-?([0-9]+)?(\.[0-9]*)?$/.test(e.target.value + e.key) ? true : e.preventDefault();
    }
  };

  const validate = () => {
    if (props.required) {
      if (value.value === null) {
        return false;
      } else {
        if (!props.maxlength) {
          return value.value.toString().length > lMinlength.value - 1;
        } else {
          return (
            value.value.toString().length > lMinlength.value - 1 && value.value.toString().length < props.maxlength + 1
          );
        }
      }
    }
    return true;
  };

  defineExpose({
    validate,
  });
</script>

<style scoped></style>
