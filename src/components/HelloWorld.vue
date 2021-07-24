<template>
  <v-container>
    <!-- <v-form ref="form"> -->
    <v-card elevation="6">
      <v-card-title>Personal Earnigs and Investments</v-card-title>

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
              v-model="investments"
              label="Investments"
              :rules="rules"
              clearable
            ></v-text-field>
          </v-col>
          <v-col cols="4">
            <v-text-field v-model="expenses" label="Expenses"></v-text-field>
          </v-col>
        </v-row>

        <v-row justify="space-between">
          <v-col cols="2">
            <v-text-field
              v-model="inv_exp_ratio_comp"
              label="Invest Expense Ratio"
            ></v-text-field>
          </v-col>
          <v-col cols="10">
            <v-slider
              v-model="inv_exp_ratio"
              :min="slider_min"
              :max="slider_max"
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
  name: "HelloWorld",

  data: function () {
    return {
      slider_min: 0,
      slider_max: 100,

      income: 2000,
      investments: 200,

      inv_exp_ratio: 10,
      interest_rate: 8,

      rules: [
        (value) => !!value || "Required.",
        (value) => !!parseInt(value) || "Enter Number.",
      ],
    };
  },

  computed: {
    expenses: {
      get: function () {
        return this.income - this.investments;
      },
      set: function (val) {
        this.investments = this.income - val;
      },
    },

    inv_exp_ratio_comp: {
      get: function () {
        return this.inv_exp_ratio / 10;
      },
      set: function (val) {
        this.inv_exp_ratio = val * 10;

        console.log("value=", val)
        this.investments = parseInt(
          this.income * val / (1 + val)
        );
        console.log(this.investments);
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