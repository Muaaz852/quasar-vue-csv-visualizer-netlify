<template>
  <q-page padding>
    <div class="q-pa-md">
      <div class="column">
        <div class="col">
          <q-input v-model="csvFileURL" type="url" class="q-mb-lg" label="CSV File Link" style="width: 350px"></q-input>
        </div>
        <div class="col">
          <q-btn color="primary" label="Load Data" @click="fetchData"></q-btn>
        </div>
        <div class="col q-mt-lg">
          <q-table title="CSV Data" :columns="csvHeaders" :data="csvItems" row-key="name" auto-width="true">
          </q-table>
        </div>
      </div>
    </div>
  </q-page>
</template>

<style>
</style>

<script>
import Papa from 'papaparse'
export default {
  name: 'PageIndex',
  data () {
    return {
      csvFileURL: null,
      csvContent: null,
      csvHeaders: [],
      csvItems: []
    }
  },
  methods: {
    fetchData: function () {
      this.csvContent = null
      this.csvHeaders = []
      this.csvItems = []
      Papa.parse(this.csvFileURL, {
        delimiter: '', // auto-detect
        newline: '', // auto-detect
        quoteChar: '"',
        escapeChar: '"',
        header: false,
        transformHeader: undefined,
        dynamicTyping: true,
        preview: 0,
        encoding: '',
        worker: false,
        comments: false,
        step: undefined,
        complete: (results) => {
          this.csvContent = results.data
          this.extractHeadersAndData()
          // console.log(this.csvContent);
          // console.log(Object.values(this.csvContent));
        },
        error: undefined,
        download: true,
        downloadRequestHeaders: undefined,
        skipEmptyLines: false,
        chunk: undefined,
        fastMode: undefined,
        beforeFirstChunk: undefined,
        withCredentials: undefined,
        transform: undefined,
        delimitersToGuess: [',', '\t', '|', ';', Papa.RECORD_SEP, Papa.UNIT_SEP]
      })
    },
    extractHeadersAndData () {
      const csvDataArray = Object.values(this.csvContent)

      for (const index in csvDataArray[0]) {
        this.csvHeaders.push(
          { 'name': csvDataArray[0][index], 'label': csvDataArray[0][index], 'field': csvDataArray[0][index], 'sortable': true }
        )
      }

      csvDataArray.shift()
      csvDataArray.pop()

      console.log(this.csvHeaders)
      console.log(this.csvContent[0])
      console.log(csvDataArray)

      for (const index in csvDataArray) {
        let dict = {}
        this.csvContent[0].forEach((key, i) => (dict[key] = csvDataArray[index][i]))
        // console.log(dict);
        this.csvItems.push(dict)
      }

      // console.log(this.csvContent);
      // console.log(this.csvHeaders);
      console.log(this.csvItems)
    }
  }
}
</script>
