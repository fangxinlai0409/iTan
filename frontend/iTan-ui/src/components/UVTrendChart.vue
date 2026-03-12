<template>
  <div class="chart-wrap">
    <Line :data="chartData" :options="chartOptions" />
  </div>
</template>

<script setup>
import { computed } from 'vue'
import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  LineElement,
  PointElement,
  CategoryScale,
  LinearScale,
  Filler,
} from 'chart.js'
import { Line } from 'vue-chartjs'

ChartJS.register(
  Title,
  Tooltip,
  Legend,
  LineElement,
  PointElement,
  CategoryScale,
  LinearScale,
  Filler
)

const props = defineProps({
  trend: {
    type: Array,
    default: () => [],
  },
})

const formatHour = (isoString) => {
  if (!isoString) return '--'
  const date = new Date(isoString)
  return date.toLocaleTimeString([], {
    hour: '2-digit',
    minute: '2-digit',
  })
}

const chartData = computed(() => ({
  labels: props.trend.map((point) => formatHour(point.time)),
  datasets: [
    {
      label: 'UV Index',
      data: props.trend.map((point) => Number(point.uv_index) || 0),
      borderColor: '#2563eb',
      backgroundColor: (context) => {
        const chart = context.chart
        const { ctx, chartArea, scales } = chart

        if (!chartArea) return null

        const gradient = ctx.createLinearGradient(0, chartArea.bottom, 0, chartArea.top)

        const y = scales.y
        const max = y.max || 12
        const stop = (value) => value / max

        gradient.addColorStop(stop(0), 'rgba(34,197,94,0.25)')   // green
        gradient.addColorStop(stop(3), 'rgba(234,179,8,0.25)')   // yellow
        gradient.addColorStop(stop(6), 'rgba(249,115,22,0.25)')  // orange
        gradient.addColorStop(stop(8), 'rgba(239,68,68,0.25)')   // red
        gradient.addColorStop(stop(11), 'rgba(168,85,247,0.25)') // purple

        return gradient
      },
      fill: true,
      tension: 0.35,
      pointRadius: 3,
      pointHoverRadius: 6,
      pointBackgroundColor: props.trend.map((point) => {
        const uv = Number(point.uv_index) || 0
        if (uv < 3) return '#22c55e'
        if (uv < 6) return '#eab308'
        if (uv < 8) return '#f97316'
        if (uv < 11) return '#ef4444'
        return '#a855f7'
      }),
      pointBorderColor: props.trend.map((point) => {
        const uv = Number(point.uv_index) || 0
        if (uv < 3) return '#22c55e'
        if (uv < 6) return '#eab308'
        if (uv < 8) return '#f97316'
        if (uv < 11) return '#ef4444'
        return '#a855f7'
      }),
      borderWidth: 2.5,
      segment: {
        borderColor: (ctx) => {
            const y = ctx.p1.parsed.y

            if (y < 3) return '#22c55e'   // Low - green
            if (y < 6) return '#eab308'   // Moderate - yellow
            if (y < 8) return '#f97316'   // High - orange
            if (y < 11) return '#ef4444'  // Very High - red
            return '#a855f7'              // Extreme - purple
        },
      },
    },
  ],
}))

const chartOptions = computed(() => ({
  responsive: true,
  maintainAspectRatio: false,
  plugins: {
    legend: {
      display: false,
    },
    tooltip: {
      callbacks: {
        label: (context) => `UV ${context.parsed.y}`,
      },
    },
  },
  scales: {
    x: {
      grid: {
        display: false,
      },
      ticks: {
        color: '#6b7280',
        maxRotation: 0,
        autoSkip: true,
      },
      border: {
        display: false,
      },
    },
    y: {
        min: 0,
        max: 12,
        ticks: {
            stepSize: 3,
            color: '#6b7280',
      },
      grid: {
        color: '#e5e7eb',
      },
      border: {
        display: false,
      },
      title: {
        display: true,
        text: 'UV',
        color: '#6b7280',
      },
    },
  },
}))


</script>

<style scoped>
.chart-wrap {
  width: 100%;
  height: 260px;
  background: #ffffff;
  border-radius: 16px;
  padding: 1rem 1rem 0.75rem;
}
</style>