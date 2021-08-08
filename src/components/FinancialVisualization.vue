<template>
  <v-container>
    <div>
      <apexchart type="line" :options="options" :series="series"></apexchart>

      <v-btn @click="button_clicked" elevation="2">Test</v-btn>
    </div>
  </v-container>
</template>

<script>
export default {
  name: "FinancialVisualization",

  props: {
    trajectory: [],
  },

  watch: {
    trajectory: function () {
      this.series = [
        {
          data: this.trajectory,
        },
      ];
    },
  },

  data: function () {
    return {
      options: {
        chart: {
          id: "vuechart-example",
        },
        //xaxis: {
        //  categories: [1991, 1992, 1993, 1994, 1995, 1996, 1997, 1998],
        //},
      },
      series: [
        {
          name: "Networth",
          data: this.trajectory,
        },
      ],
    };
  },

  computed: {
  },

  methods: {
    button_clicked() {
      console.log("button clicked");
      console.log(this.trajectory);
    },
  },
};
</script>
