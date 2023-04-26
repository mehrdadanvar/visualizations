<template>
    <div>
        <h1>Visualizations</h1>
        <div class="file_reader container border border-slate-800mx-6 my-6 px-6 py-6">
            <p>Read your file</p>
            <input type="file" @change="loadfile">
            <p>{{ csv_contents }}</p>
            <pre v-for="x in csv_contents" :key="x">{{ x }}</pre>
        </div>
    </div>
</template>

<script setup>
import Papa from 'papaparse'
let csv_contents = ref('')
let loadfile = (event) => {
    let file = event.target.files[0]
    let reader = new FileReader()
    reader.onload = (ev) => {
        let file_contents = ev.target.result
        csv_contents.value = file_contents
        console.log(typeof (file_contents))
        const csvData = Papa.parse(file_contents, { header: true }).data;
        console.log(csvData)

    }
    reader.readAsText(file)

}



</script>

<style scoped></style>