<template>
  <div class="main-content">
    <MortgageCalculator @update:calculations="displayCalculations"/>
    <ResultsSection v-model:monthly-repay="monthlyRepay" v-model:total-repay="totalRepay"/>
  </div>
</template>

<script>
import MortgageCalculator from './assets/MortgageCalculator.vue';
import ResultsSection from './assets/ResultsSection.vue';

export default {
  name: "App",
  components: {
    MortgageCalculator, 
    ResultsSection
  },
  data() {
      return {
      monthlyRepay: '',
      totalRepay: '',
    }
  },
  methods: {
    displayCalculations(monthlyRepayments, totalRepay) {
      this.monthlyRepay = monthlyRepayments.toString().toLocaleString();
      this.totalRepay = totalRepay.toLocaleString();
    }
  }
};
</script>

<style>
.main-content {
    display: flex;
    flex-direction: row;
    align-items: center;
}
@media (max-width: 1000px) {
    .main-content {
        flex-direction: column;
    }
}
</style>
