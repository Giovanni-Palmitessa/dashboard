<script>
import Chart from "chart.js/auto";
import axios from "axios";
export default {
  data() {
    return {
      chart: null,
      monthlyConnectionsData: {
        labels: [],
        datasets: [
          {
            label: "Monthly",
            data: [],
            backgroundColor: "rgba(75, 192, 192, 0.2)",
            borderColor: "rgba(75, 192, 192, 1)",
            borderWidth: 2,
          },
        ],
      },
      userAgeRangeData: {
        labels: [],
        datasets: [
          {
            label: "User Age Range",
            data: [],
            backgroundColor: [],
            borderColor: "rgba(255, 99, 132, 1)",
            borderWidth: 2,
          },
        ],
      },
      devicesData: {
        labels: [],
        datasets: [
          {
            data: [],
            backgroundColor: [],
          },
        ],
      },
      dataLoaded: false,
    };
  },
  async mounted() {
    await this.loadData(); // Attendere il caricamento dei dati mensili
    await this.loadDataAndCreateCharts();
  },

  methods: {
    async loadData() {
      if (this.dataLoaded) {
        return; // Esci se i dati sono già stati caricati
      }
      try {
        const response = await axios.get("../../../data.json", {
          headers: {
            "Cache-Control": "no-cache",
          },
        });
        const data = response.data;

        const monthlyConnections = data.MonthlyConnections;

        const labels = monthlyConnections.map((item) => item.month);
        const connectionsData = monthlyConnections.map(
          (item) => item.connections
        );

        // Popola l'array "data" nel tuo oggetto "monthlyConnectionsData"
        this.monthlyConnectionsData.labels = labels;
        this.monthlyConnectionsData.datasets[0].data = connectionsData;

        this.dataLoaded = true; // Imposta la variabile di stato a true quando i dati sono stati caricati
      } catch (error) {
        console.error("Errore nella richiesta dei dati JSON:", error);
      }
    },
    generateColors(numColors) {
      const predefinedColors = [
        "rgba(255, 99, 132, 0.6)",
        "rgba(54, 162, 235, 0.6)",
        "rgba(255, 206, 86, 0.6)",
        "rgba(75, 192, 192, 0.6)",
        "rgba(153, 102, 255, 0.6)",
      ];

      const colors = [];
      for (let i = 0; i < numColors; i++) {
        const colorIndex = i % predefinedColors.length;
        colors.push(predefinedColors[colorIndex]);
      }
      return colors;
    },
    async loadDataAndCreateCharts() {
      try {
        const response = await axios.get("../../../data.json", {
          headers: {
            "Cache-Control": "no-cache",
          },
        });
        const data = response.data;

        // Carica i dati mensili
        const monthlyConnections = data.MonthlyConnections;
        const monthlyLabels = monthlyConnections.map((item) => item.month);
        const monthlyData = monthlyConnections.map((item) => item.connections);

        // Carica i dati dell'età degli utenti
        const userAgeRange = data.UsersAgeRange;
        const userAgeLabels = userAgeRange.map((item) => item.range);
        const userAgeData = userAgeRange.map((item) => item.connections);

        // Carica i dati dei dispositivi
        const devices = data.Devices;
        const deviceLabels = devices.map((item) => item.os);
        const deviceData = devices.map((item) => item.connections);

        // Creazione del grafico mensile
        const monthlyCtx = document
          .getElementById("monthlyConnectionsChart")
          .getContext("2d");
        const monthlyChart = new Chart(monthlyCtx, {
          type: "line",
          data: {
            labels: monthlyLabels,
            datasets: [
              {
                label: "Monthly",
                data: monthlyData,
                backgroundColor: "rgba(75, 192, 192, 0.2)",
                borderColor: "rgba(75, 192, 192, 1)",
                borderWidth: 2,
              },
            ],
          },
          options: {
            responsive: true,
            maintainAspectRatio: false,
            scales: {
              y: {
                beginAtZero: true,
              },
            },
          },
        });

        // Creazione del grafico dell'età degli utenti
        const userAgeCtx = document
          .getElementById("userAgeRangeChart")
          .getContext("2d");
        const userAgeChart = new Chart(userAgeCtx, {
          type: "bar",
          data: {
            labels: userAgeLabels,
            datasets: [
              {
                label: "User Age Range",
                data: userAgeData,
                backgroundColor: this.generateColors(userAgeData.length),
                borderColor: "rgba(255, 99, 132, 1)",
                borderWidth: 2,
              },
            ],
          },
          options: {
            responsive: true,
            maintainAspectRatio: false,
            scales: {
              y: {
                beginAtZero: true,
              },
            },
          },
        });

        // Creazione del grafico dei dispositivi
        const deviceCtx = document
          .getElementById("devicesChart")
          .getContext("2d");
        const deviceChart = new Chart(deviceCtx, {
          type: "pie",
          data: {
            labels: deviceLabels,
            datasets: [
              {
                data: deviceData,
                backgroundColor: this.generateColors(deviceData.length),
              },
            ],
          },
          options: {
            responsive: true,
            maintainAspectRatio: false,
          },
        });
      } catch (error) {
        console.error("Errore nella richiesta dei dati JSON:", error);
      }
    },
  },
};
</script>

<template>
  <main>
    <h1 class="text-center text-2xl font-bold py-6">Monthly Connections</h1>

    <hr class="text-gray w-11/12 mx-auto" />

    <div style="max-width: 1200px; height: 250px; margin: 0 auto">
      <canvas id="monthlyConnectionsChart"></canvas>
    </div>

    <div class="charts pt-20 pb-8 flex gap-20">
      <div class="chart-left basis-5/12">
        <h2 class="text-center font-bold text-lg">User Age Range</h2>
        <div style="max-width: 600px; height: 250px; margin-left: 40px">
          <canvas id="userAgeRangeChart"></canvas>
        </div>
      </div>
      <div class="chart-right basis-5/12">
        <h2 class="text-center font-bold text-lg">Operating Systems</h2>
        <div style="max-width: 600px; height: 250px; margin-left: 40px">
          <canvas id="devicesChart"></canvas>
        </div>
      </div>
    </div>
  </main>
</template>

<style lang="scss" scoped>
main {
  width: calc(100% - 234px);
  margin-left: auto;
}
</style>
