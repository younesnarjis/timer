<template>
  <EmployeeComponent
    :work-entry="workEntry"
    @fetchData="fetchData"
  />
</template>

<script>
import axios from 'axios'
import EmployeeComponent from './components/EmployeeComponent.vue'

export default {
  name: 'App',
  components: {
    EmployeeComponent
  },
  data() {
    return {
      workEntry: {
        id: null,
        employee: {
          id: null,
          firstName: null,
          lastName: null,
          workStatus: null
        },
        workEntryIn: {
          date: null
        },
        workedSeconds: null
      },
    }
  },
  mounted() {
    this.fetchData()
  },
  methods: {
    async fetchData() {
      try {
        const response = await axios.get('https://api-test.sesametime.com/schedule/v1/work-entries', {
          headers: {
            Authorization: 'Bearer 16e2f0694a311151c01aa0a131b94a5a3ad7f110e12c2d8f459fcbb158214f5f'
          }
        });

        const workEntries = response.data.data;

        if (workEntries.length > 0) {
          this.workEntry = workEntries[0];
        }
      } catch (error) {
        console.error(error);
      }
    }
  }
}
</script>