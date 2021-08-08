<template>
  <v-container>
    <div>
      <apexchart
        type="line"
        :options="options"
        :series="series"
      ></apexchart>
    </div>

  </v-container>
</template>

<script>

export default {
  name: "FinancialVisualization",

  props: ["data_array"],

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
          data: this.data_array,
        },
      ],
    };
  },

  computed: {},
  methods: {},
};
</script>
