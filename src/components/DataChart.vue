<template>
  <div>
    <h1 style="text-align: center;">Kasus Covid-19 yang Terkonfirmasi di 5 Provinsi di Pulau Jawa</h1>
    <BarChart :label="label" :charData="charData"> </BarChart>
  </div>
</template>

<script>
//API
const axios = require("axios");
// Jika Dummy Comment pada baris di atas

import BarChart from "./BarChart.vue";
import Chart from "chart.js";

export default {
  components: {
    BarChart,
  },
  data() {
    return {
      //API (uncomment baris 23-24)
      label: [],
      charData: [],

      // Dummy (uncomment baris 27-28)
      // label: ["DKI JAKARTA", "JAWA BARAT", "JAWA TENGAH", "JAWA TIMUR", "DAERAH ISTIMEWA YOGYAKARTA"],
      // charData: [150, 200, 300, 400, 250],
    };
  },
  methods: {
    panggilChart() {
      const barc = document.getElementById("barChart");
      new Chart(barc, {
        type: "bar",
        data: {
          labels: this.label,
          datasets: [
            {
              label: "Kasus",
              backgroundColor: "rgba(144,238,144 , 0.9 )",
              data: this.charData,
            },
          ],
        },
        options: {
          scales: {
            yAxes: [
              {
                ticks: {
                  beginAtZero: true,
                },
              },
            ],
          },
        },
      });
    },
  },

  // Dummy (uncomment baris 62-64)
  // mounted() {
  //   this.panggilChart();
  // },

  // API (uncomment baris 67-78)
  async created() {
    const { data } = await axios.get("https://apicovid19indonesia-v2.vercel.app/api/indonesia/provinsi/more");
    console.log(data);
    for (var i = 0; i < 34; i++) {
      if (data[i].provinsi == "JAWA TENGAH" || data[i].provinsi == "JAWA TIMUR" || data[i].provinsi == "JAWA BARAT" || data[i].provinsi == "DKI JAKARTA" || data[i].provinsi == "DAERAH ISTIMEWA YOGYAKARTA") {
        this.label.push(data[i].provinsi);
        this.charData.push(data[i].kasus);
      }
    }
    console.log(this.label, this.charData);
    this.panggilChart();
  },
};
</script>

<style></style>
