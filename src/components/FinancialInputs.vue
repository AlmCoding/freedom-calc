<template>
  <v-container>
    <!-- <v-form ref="form"> -->
    <v-card elevation="6">
      <v-card-title>Monthly Income and Investments</v-card-title>

      <v-card-text>
        <v-row justify="space-between">
          <v-col cols="4">
            <v-text-field
              v-model="income"
              label="Income"
              :rules="rules"
              clearable
            ></v-text-field>
          </v-col>
          <v-col cols="4">
            <v-text-field
              v-model="investments_comp"
              label="Investments"
              :rules="rules"
              clearable
            ></v-text-field>
          </v-col>
          <v-col cols="4">
            <v-text-field
              v-model="expenses_comp"
              label="Expenses"
              clearable
            ></v-text-field>
          </v-col>
        </v-row>

        <v-row justify="space-between">
          <v-col cols="2">
            <v-text-field
              v-model="inv_exp_ratio"
              label="Invest Expense Ratio"
            ></v-text-field>
          </v-col>
          <v-col cols="10">
            <v-slider
              v-model="inv_exp_ratio_slider_comp"
              :min="slider_min"
              :max="slider_max_ratio"
              label=""
            >
            </v-slider>
          </v-col>
        </v-row>

        <v-row justify="space-between">
          <v-col cols="2">
            <v-text-field
              v-model="interest_rate"
              label="Interest Rate"
            ></v-text-field>
          </v-col>

          <v-col cols="10">
            <v-slider
              v-model="interest_rate"
              :min="slider_min"
              :max="slider_max"
              label=""
            >
            </v-slider>
          </v-col>
        </v-row>
      </v-card-text>
    </v-card>
    <!-- </v-form> -->
  </v-container>
</template>


<script>
export default {
  name: "FinancialInputs",

  data: function () {
    return {
      slider_min: 0,
      slider_max: 100,
      slider_max_ratio: 100000,

      income: 2000,
      inv_exp_ratio: 1,

      interest_rate: 8,

      rules: [
        (value) => !!value || "Required.",
        (value) => !!parseInt(value) || "Enter Number.",
      ],
    };
  },

  computed: {
    investments_comp: {
      get: function () {
        return parseInt(
          (this.income * parseFloat(this.inv_exp_ratio)) /
            (1 + parseFloat(this.inv_exp_ratio))
        );
      },
      set: function (val) {
        val = parseInt(val);
        var expenses = this.income - val;
        this.inv_exp_ratio = val / expenses;
      },
    },

    expenses_comp: {
      get: function () {
        return parseInt(this.income / (1 + parseFloat(this.inv_exp_ratio)));
      },
      set: function (val) {
        var investments = this.income - val;
        this.inv_exp_ratio = investments / val;
      },
    },

    inv_exp_ratio_slider_comp: {
      get: function () {
        return this.inv_exp_ratio * 10000;
      },
      set: function (val) {
        val = parseFloat(val);
        this.inv_exp_ratio = val / 10000;
      },
    },
  },
  
  methods: {},
};
</script>

<!--
<style scoped>
  .container {
    border: 1px solid green;
  }
  .row {
    border: 1px solid red;
  }
  .col {
    border: 1px solid blue;
  }
</style>
-->