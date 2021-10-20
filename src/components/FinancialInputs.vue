<template>
  <v-container>

    <v-card tile elevation="6">
      <v-card-title>Current Monthly Numbers</v-card-title>

      <v-card-text>
        <v-row dense justify="space-between">
          <v-col cols="6">
            <v-text-field
              v-model="income"
              @change="income_changed"
              label="Income"
              :rules="rules"
              clearable
            ></v-text-field>
          </v-col>
          <v-col cols="6">
            <v-text-field
              v-model="investments"
              @change="investments_changed"
              label="Investments"
              :rules="rules"
              clearable
            ></v-text-field>
          </v-col>
          <!--
          <v-col cols="4">
            <v-text-field
              v-model="expenses"
              @change="expenses_changed"
              label="Expenses"
              clearable
            ></v-text-field>
          </v-col>
          -->
        </v-row>

        <v-row dense justify="space-between">
          <v-col cols="4">
            <v-text-field
              v-model="savings_rate"
              @change="update_inv_exp"
              label="Savings Rate"
            ></v-text-field>
          </v-col>
          <v-col cols="8">
            <v-slider
              v-model="savings_rate"
              @change="update_inv_exp"
              :min="slider_min"
              :max="slider_max"
              label=""
            >
            </v-slider>
          </v-col>
        </v-row>

        <v-row dense justify="space-between">
          <v-col cols="6">
            <v-text-field
              v-model="income_growth_rate"
              @change="compute_trajectory"
              label="Income Growth Rate"
              clearable
            ></v-text-field>
          </v-col>
          <v-col cols="6">
            <v-text-field
              v-model="max_annual_income"
              @change="compute_trajectory"
              label="Max. Annual Income"
              clearable
            ></v-text-field>
          </v-col>
        </v-row>

        <v-row dense justify="space-between">
          <v-col cols="6">
            <v-text-field
              v-model="current_networth"
              @change="compute_trajectory"
              label="Current Networth"
              clearable
            ></v-text-field>
          </v-col>
          <v-col cols="6">
            <v-text-field
              v-model="current_age"
              @change="compute_trajectory"
              label="Current Age"
              clearable
            ></v-text-field>
          </v-col>
        </v-row>

        <v-row dense justify="space-between">
          <v-col cols="4">
            <v-text-field
              v-model="interest_rate"
              @change="compute_trajectory"
              label="Interest Rate"
            ></v-text-field>
          </v-col>
          <v-col cols="8">
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
      </v-card-text>
    </v-card>

    <v-spacer></v-spacer>

    <v-card tile elevation="6">
      <v-card-title>Coast FIRE Plan</v-card-title>
      <!--
      <v-row justify="space-between">
        <v-col cols="6">
          <v-card-title>Coast FIRE Plan</v-card-title>
        </v-col>
        <v-col cols="6">
          <v-switch v-model="switch1" label="Coast FIRE"></v-switch>
        </v-col> 
      </v-row>-->

      <v-card-text>
        <v-row dense justify="space-between">
          <v-col cols="6">
            <v-text-field
              v-model="coast_fire_expenses"
              @change="compute_trajectory"
              label="Monthly FIRE Expenses"
              clearable
            ></v-text-field>
          </v-col>

          <v-col cols="6">
            <v-text-field
              v-model="coast_fire_age"
              @change="compute_trajectory"
              label="Coast FIRE Age"
              clearable
            ></v-text-field>
          </v-col>
        </v-row>
      </v-card-text>
    </v-card>


    <v-card tile elevation="6">
      <FinancialVisualization :trajectory="trajectory" />
    </v-card>


  </v-container>
</template>


<script>
import FinancialVisualization from "./FinancialVisualization";

export default {
  name: "FinancialInput",

  components: {
    FinancialVisualization,
  },

  data: function () {
    return {
      slider_min: 0,
      slider_max: 100,

      income: 5000,
      investments: 3000,
      expenses: 2000,
      current_age: 27,
      coast_fire_age: 44,
      coast_fire_expenses: 2000,

      savings_rate: 60,
      income_growth_rate: 1,
      max_annual_income: 80000,

      interest_rate: 7,
      current_networth: 125000,

      rules: [
        (value) => !!value || "Required.",
        (value) => !!parseInt(value) || "Enter Number.",
      ],

      trajectory: [],
      year_of_freedom: 30,
    };
  },

  mounted: function () {
    this.compute_trajectory();
  },

  computed: {},

  methods: {
    update_savings_rate: function () {
      this.savings_rate = parseInt((100 * this.expenses) / this.income);
      this.compute_trajectory();
    },

    update_inv_exp: function () {
      this.investments = (this.income * this.savings_rate) / 100;
      this.expenses = this.income - this.investments;
      this.compute_trajectory();
    },

    income_changed: function () {
      this.investments = this.income - this.expenses;
      this.update_inv_exp();
    },
    investments_changed: function () {
      this.expenses = this.income - this.investments;
      this.update_savings_rate();
    },
    expenses_changed: function () {
      this.investments = this.income - this.expenses;
      this.update_savings_rate();
    },

    compute_trajectory: function () {
      this.trajectory = [];

      var annual_investments = this.investments * 12;
      var max_annual_investments =
        (this.max_annual_income * this.savings_rate) / 100;
      var interest = this.interest_rate / 100;
      var networth = this.current_networth;

      this.year_of_freedom = 30;

      for (let year = 0; year < 30; year++) {
        networth *= 1 + interest;
        networth += annual_investments;
        this.trajectory.push(parseInt(networth));

        if (
          networth * interest > this.expenses * 12 &&
          year < this.year_of_freedom
        ) {
          this.year_of_freedom = year;
        }

        //console.log(annual_investments);
        annual_investments *= 1 + this.income_growth_rate / 100;
        if (annual_investments > max_annual_investments) {
          annual_investments = max_annual_investments;
        }
      }
      console.log(this.trajectory);
      console.log(this.year_of_freedom);
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
