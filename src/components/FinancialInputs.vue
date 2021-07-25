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
              @change="income_changed"
              label="Income"
              :rules="rules"
              clearable
            ></v-text-field>
          </v-col>
          <v-col cols="4">
            <v-text-field
              v-model="investments"
              @change="investments_changed"
              label="Investments"
              :rules="rules"
              clearable
            ></v-text-field>
          </v-col>
          <v-col cols="4">
            <v-text-field
              v-model="expenses"
              @change="expenses_changed"
              label="Expenses"
              clearable
            ></v-text-field>
          </v-col>
        </v-row>

        <v-row justify="space-between">
          <v-col cols="2">
            <v-text-field
              v-model="inv_exp_ratio"
              @change="inv_exp_ratio_changed"
              label="Invest Expense Ratio"
            ></v-text-field>
          </v-col>
          <v-col cols="10">
            <v-slider
              v-model="inv_exp_ratio_10"
              @change="inv_exp_ratio_10_changed"
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
              @change="compute_trajectory"
              label="Interest Rate"
            ></v-text-field>
          </v-col>

          <v-col cols="10">
            <v-slider
              v-model="interest_rate"
              @change="compute_trajectory"
              :min="slider_min"
              :max="slider_max"
              label=""
            >
            </v-slider>
          </v-col>
        </v-row>

        <v-row justify="space-between">
          <v-col cols="12">
            <v-text-field
              v-model="current_networth"
              @change="compute_trajectory"
              label="Current Networth"
              clearable
            ></v-text-field>
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

      income: 5000,
      investments: 3000,
      expenses: 2000,

      inv_exp_ratio: 1.5,
      inv_exp_ratio_10: 15,

      interest_rate: 5,
      current_networth: 85000,

      rules: [
        (value) => !!value || "Required.",
        (value) => !!parseInt(value) || "Enter Number.",
      ],

      trajectory: [],
    };
  },

  computed: {},

  methods: {
    update_inv_exp_ratio: function () {
      this.inv_exp_ratio = parseFloat(this.investments / this.expenses).toFixed(
        2
      );
      this.inv_exp_ratio_10 = this.inv_exp_ratio * 10;
      this.compute_trajectory();
    },

    update_inv_exp: function () {
      this.investments = parseInt(
        (this.income * parseFloat(this.inv_exp_ratio)) /
          (1 + parseFloat(this.inv_exp_ratio))
      );
      this.expenses = this.income - this.investments;
      this.compute_trajectory();
    },

    income_changed: function () {
      this.investments = this.income - this.expenses;
      this.update_inv_exp_ratio();
    },
    investments_changed: function () {
      this.expenses = this.income - this.investments;
      this.update_inv_exp_ratio();
    },
    expenses_changed: function () {
      this.investments = this.income - this.expenses;
      this.update_inv_exp_ratio();
    },

    inv_exp_ratio_changed: function () {
      this.inv_exp_ratio_10 = this.inv_exp_ratio * 10;
      this.update_inv_exp();
    },
    inv_exp_ratio_10_changed: function () {
      this.inv_exp_ratio = this.inv_exp_ratio_10 / 10;
      this.update_inv_exp();
    },

    compute_trajectory: function () {
      this.trajectory = [];

      var annual_investments = this.investments * 12;
      var interest = this.interest_rate / 100;
      var networth = this.current_networth;

      for (let year = 0; year < 50; year++) {
        networth = networth * (1 + interest);
        networth += annual_investments;
        this.trajectory.push(networth);
      }
      console.log(this.trajectory);
    },
  },
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
