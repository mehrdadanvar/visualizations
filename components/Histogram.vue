<template>
  <div class="box">
    <div>
      <input type="range" v-model="width" min="500" max="2000" step="50" />
      <svg :width="width" :height="height">
        <!-- prettier-ignore -->
        <Rect class="rect" v-for="(rect, index) in rects" :key="index" :height="rect.freq * 5" :width="15"
          :x="((16 * rect.id) + (-300))" :y="20" :rx="3" :fill="'green'" :transform="'translate(500,500) rotate(180)'" />
        <path d="M 10 10 H 100 V 90 H 10 L 20 20" />
        <!-- <Rect v-for="(rect, index) in rects" :key="index" :height="rect.freq * 5" :width="9" :x="10 * rect.id + 11"
          :y="20" :fill="'red'" :transform="'translate(600,500) rotate(170)'" :fill-opacity="0.3" /> -->
        <!-- <line x1="0" x2="500" y1="450" y2="450" stroke="blue" stroke-width="1" /> -->
        <!-- <line x1="0" x2="0" y1="1" y2="450" stroke="navy" stroke-width="10" /> -->
        <text class="chart-title" x="50" y="15">Normally Distributed Ramdom Numbers</text>
        <Horaxis :x1="1" :y1="1" :x2="1" :y2="450" :stroke="'red'" :stroke-width="0.5" />
      </svg>
    </div>
    <button @click="binz">draw</button>
    <input type="range" v-model="opacity" min="0" max="1" step="0.02" />
    <input type="color" v-model="value" />
    <p>{{ opacity }}</p>
  </div>
  <div>
    <!-- <h3>this is the arrau of data</h3> -->
    <!-- <ul v-for="rect in rects" v-bind:key="rect">
      <li>{{ rect }}</li>
    </ul> -->
  </div>
</template>
<script setup>
let rect = ref([])
function randn(n) {
  let u = 1 - Math.random(); //Converting [0,1) to (0,1)
  let v = Math.random();
  function generate() {
    return Math.sqrt(-2.0 * Math.log(1 - Math.random())) * Math.cos(2.0 * Math.PI * Math.random());
  }
  let rands = [];
  for (let i = 0; i < n; i++) {
    rands.push(generate());
  }
  return rands;
}
function hist(data, number_bins) {
  let sorted = data.sort((a, b) => b - a);
  let minimum = Math.min(...sorted);
  let maximum = Math.max(...sorted);
  let range = maximum - minimum;
  let determind_bins = number_bins;
  let len_data = sorted.length;
  let n_bins = Math.round(len_data / determind_bins);
  let dist = range / n_bins;
  cl(minimum, maximum, range, len_data, n_bins, dist);
  let r = {
    minimum: minimum,
    maximum: maximum,
    range: range,
    len: len_data,
    ideal_bins: n_bins,
    dist: dist,
  };
  let quads = [];
  for (let i in [...Array(r.ideal_bins).keys()]) {
    // prettier-ignore
    let lower = r.minimum + (r.dist * i);
    // prettier-ignore
    let upper = lower + r.dist;
    let inter = sorted.filter((x) => {
      let y = x >= lower && x < upper;
      return y;
    });
    let freq = inter.length;
    quads.push({
      lower: parseFloat(lower),
      upper: parseFloat(upper),
      freq: parseFloat(freq),
      id: parseInt(i),
    });
  }
  console.log(quads.length);
  return quads;
}
rects.value = hist(randn(1000), 50)

</script>
<style></style>
