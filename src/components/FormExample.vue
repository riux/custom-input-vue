<template>
  <div class="container">
    <h3>Form ...</h3>
    <form :class="{ 'was-validated': isValid }">
      <div class="form-group">
        <label for="nombre">Nombre: </label>
        <TextInput
          ref="refNombre"
          v-model="nombre"
          id="nombre"
          placeholder="Nombres..."
          :strict="true"
          :required="true"
          :minlength="3" />

        <div class="invalid-feedback">
          Este campo es necesario y debe contener por lo menos 3 caracteres alfabéticos.
        </div>
        <div class="valid-feedback">Campo correcto</div>
      </div>

      <div class="form-group">
        <label for="edad">Edad: {{ typeof edad }} </label>
        <NumberInput ref="refEdad" v-model.number="edad" id="edad" placeholder="Edad..." :required="true" />
        <div class="invalid-feedback">Este campo es necesario.</div>
      </div>

      <div class="form-group">
        <label for="dni">DNI: {{ typeof dni }} </label>
        <NumberInput
          ref="refDni"
          v-model.number="dni"
          id="dni"
          placeholder="DNI..."
          :required="true"
          :minlength="8"
          :maxlength="8"
          :strict="true" />
        <div class="invalid-feedback">Este campo es necesario.</div>
      </div>
      <br />
      <button type="submit" class="btn btn-primary" @click.prevent="submit">Submit</button>
    </form>
  </div>
</template>

<script setup>
  import { ref } from "vue";
  import TextInput from "./inputs/TextInput.vue";
  import NumberInput from "./inputs/NumberInput.vue";

  const isValid = ref(false);

  const nombre = ref("");
  const edad = ref();
  const dni = ref();

  const refNombre = ref();
  const refEdad = ref();
  const refDni = ref();

  const validate = () => {
    return refNombre.value.validate() && refEdad.value.validate() && refDni.value.validate();
  };

  const submit = () => {
    if (!validate()) {
      console.error("ERROR!!!...");
      isValid.value = true;
    } else {
      console.log("Formulario correcto...");
    }
  };
</script>

<style scoped></style>
