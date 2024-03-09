<template>
  <input
    type="text"
    class="form-control"
    v-model="value"
    :id="props.id"
    :name="props.id"
    :placeholder="props.placeholder"
    :minlength="lMinlength"
    :maxlength="props.maxlength"
    @keypress="txtStrict"
    :required="required" />
</template>

<script setup>
  import { defineModel, defineProps, computed, defineExpose } from "vue";
  //const value = ref("");
  /* const props = defineProps({
    value: {
      type: String,
      default: "",
    },
  });
  const emit = defineEmits(["update"]);
  const lValue = computed({
    get: () => props.value,
    set: (newValue) => {
      emit("update", newValue);
    },
  }); */
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
    type: String,
    required: false,
    default: "",
  });

  const lMinlength = computed(() => {
    return props.required ? (!props.minlength ? 1 : props.minlength) : null;
  });

  const txtStrict = (e) => {
    return props.strict ? isAlpha(e) : true;
  };

  const isAlpha = (e) => {
    return /^[a-záéíóúA-ZÁÉÍÓÚñ ]+$/.test(e.key) ? true : e.preventDefault();
  };

  const validate = () => {
    if (props.required) {
      if (!props.maxlength) {
        return value.value.length > lMinlength.value - 1;
      } else {
        return value.value.length > lMinlength.value - 1 && value.value.length < props.maxlength + 1;
      }
    }
    return true;
  };

  defineExpose({
    validate,
  });
</script>
<style scoped></style>
