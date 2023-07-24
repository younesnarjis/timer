<template>
  <div class="header bg-gray-700 rounded-lg p-4 flex items-center">
    <TimerComponent :time="elapsedTime" />
    <ButtonComponent v-if="!isTimeRunning" :text="'Entrar'" @click="clockIn" />
    <ButtonComponent v-if="isTimeRunning && !isTimePaused" :text="'Pausar'" @click="pauseTime" />
    <ButtonComponent v-if="isTimeRunning && isTimePaused" :text="'Reanudar'" @click="resumeTime" />
    <ButtonComponent v-if="isTimeRunning" :text="'Salir'" @click="clockOut" />
    <AvatarComponent :status="workEntry.employee.workStatus" :src="workEntry.employee.imageProfileURL" />
    <div class="text-white ml-4 cursor-pointer" @click="toggleDropdown">
      {{ workEntry.employee.firstName }} {{ workEntry.employee.lastName }} {{ showDropdown ? '▲' : '▼' }}
    </div>
    <DropDownComponent v-if="showDropdown" />
  </div>
</template>

<script>
import axios from 'axios'
import TimerComponent from '@/components/TimerComponent.vue'
import ButtonComponent from '@/components/ButtonComponent.vue'
import AvatarComponent from '@/components/AvatarComponent.vue'
import DropDownComponent from '@/components/DropDownComponent.vue'

export default {
  name: 'EmployeeComponent',
  components: {
    TimerComponent,
    ButtonComponent,
    AvatarComponent,
    DropDownComponent,
  },
  props: {
    workEntry: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      elapsedTime: '00:00:00',
      isTimeRunning: false,
      isTimePaused: false,
      showDropdown: false,
      intervalId: null
    };
  },
  beforeUpdate() {
    this.getData();
  },
  methods: {
    getData() {
      this.isTimeRunning = this.workEntry.employee.workStatus === 'offline' ? false : true;
      this.elapsedTime = this.isTimeRunning ? this.formatElapsedTime(this.workEntry.workEntryIn.date) : this.formatTime(this.workEntry.workedSeconds);
    },
    async clockIn() {
      try {
        if (this.workEntry.employee.id && !this.isTimeRunning) {
          const response = await axios.post('https://api-test.sesametime.com/schedule/v1/work-entries/clock-in',
          {
            employeeId: this.workEntry.employee.id,
            workEntryIn: {
              coordinates: {
                latitude: null,
                longitude: null
              }
            }
          },
          {
            headers: {
              Authorization: 'Bearer 16e2f0694a311151c01aa0a131b94a5a3ad7f110e12c2d8f459fcbb158214f5f'
            },
          });
          this.isTimeRunning = true;
          this.elapsedTime = this.formatTime(response.data.data.workEntryIn.date)
          this.$emit('fetchData');

          this.intervalId = setInterval(() => {
            if (!this.isTimePaused) {
              this.elapsedTime = this.formatElapsedTime(this.workEntry.workEntryIn.date);
            }
          }, 1000);
        }
      } catch (error) {
        console.error(error);
      }
    },

    async clockOut() {
      try {
        if (this.workEntry.employee.id && this.isTimeRunning) {
          const response = await axios.post('https://api-test.sesametime.com/schedule/v1/work-entries/clock-out',
          {
            employeeId: this.workEntry.employee.id,
            workEntryOut: {
              coordinates: {
                latitude: null,
                longitude: null
              }
            }
          },
          {
            headers: {
              Authorization: 'Bearer 16e2f0694a311151c01aa0a131b94a5a3ad7f110e12c2d8f459fcbb158214f5f'
            },
          });
          this.isTimeRunning = false;
          this.elapsedTime = this.formatTime(response.data.data.workedSeconds);
          this.$emit('fetchData');
        }
      } catch (error) {
        console.error(error);
      }
    },
    pauseTime() {
      this.isTimePaused = true;
      clearInterval(this.intervalId);
    },

    resumeTime() {
      this.isTimePaused = false;
      this.intervalId = setInterval(() => {
        if (!this.isTimePaused) {
          this.elapsedTime = this.formatElapsedTime(this.workEntry.workEntryIn.date);
        }
      }, 1000);
    },

    formatElapsedTime(startTime) {
      const currentTime = new Date().getTime();
      const elapsedMilliseconds = currentTime - new Date(startTime).getTime();

      if (isNaN(elapsedMilliseconds)) {
        return '00:00:00';
      }

      const elapsedSeconds = Math.floor(elapsedMilliseconds / 1000);
      const hours = Math.floor(elapsedSeconds / 3600);
      const minutes = Math.floor((elapsedSeconds % 3600) / 60);
      const seconds = elapsedSeconds % 60;

      return `${String(hours).padStart(2, '0')}:${String(minutes).padStart(2, '0')}:${String(seconds).padStart(2, '0')}`;
    },


    formatTime(timeInSeconds) {
      const hours = Math.floor(timeInSeconds / 3600);
      const minutes = Math.floor((timeInSeconds % 3600) / 60);
      const seconds = timeInSeconds % 60;

      return `${hours}:${String(minutes).padStart(2, '0')}:${String(seconds).padStart(2, '0')}`;
    },

    toggleDropdown() {
      this.showDropdown = !this.showDropdown;
    }
  },
};
</script>

<style>
  .bg-gray-700 {
    background-color: #4a5568;
  }

  .text-white {
    color: #fff;
  }

  .ml-4 {
    margin-left: 1rem;
  }

  .p-4 {
    padding: 1rem;
  }

  .rounded-lg {
    border-radius: 0.5rem;
  }

  .flex {
    display: flex;
  }

  .items-center {
    align-items: center;
  }
</style>
