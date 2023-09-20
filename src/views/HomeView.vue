<template>
  <div class="home">
    <CalculationForm/>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import CalculationForm from "@/components/CalculationForm.vue"; // @ is an alias to /src

export default defineComponent({
  name: 'HomeView',
  components: {
    CalculationForm,
  },
});
</script>
