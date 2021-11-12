<template>
    <div>
        <md-table>
            <md-table-toolbar>
                <div class="md-toolbar-section-start"></div>
                <md-field md-clearable class="md-toolbar-section-end">
                <md-input placeholder="Search by title..." v-model="search" @input="$emit('search-sound-recording', search)" />
                </md-field>
            </md-table-toolbar>
            <md-table-row>
                <md-table-head>#</md-table-head>
                <md-table-head>Title</md-table-head>
                <md-table-head>Artist</md-table-head>
                <md-table-head>Interested Parties</md-table-head>
                <md-table-head>Durations</md-table-head>
                <md-table-head>Action</md-table-head>
            </md-table-row>
            <md-table-row 
              :key="index" 
              v-for="(item,index) in soundRecords" 
              v-if="soundRecords.length > 0">
              <md-table-cell >
                {{index+1}}
              </md-table-cell>
              <md-table-cell>
                {{item.work_title}}
              </md-table-cell>
              <md-table-cell>
                {{item.artist || '--'}}
              </md-table-cell>
              <md-table-cell>
                {{GetArrayElementValues(item.interested_parties, 'full_name').join(', ') || '--'}}
              </md-table-cell>
              <md-table-cell>
                {{getDurations(item.durations)}}
              </md-table-cell>
              <md-table-cell>
                <md-button @click="$emit('del-sound-recording', item.id)" >
                  <md-icon>delete</md-icon>
                </md-button>
              </md-table-cell>
            </md-table-row>
            <md-table-row  v-if="soundRecords.length == 0" >
                <md-table-cell colspan="6" class="text-align"> 
                Record Not Found 
                </md-table-cell>
            </md-table-row>
        </md-table>
    </div> 
</template>

<script>
  export default {
    name: 'SoundRecordingList',
    props: ["soundRecords"],
    data() {
      return {
        search: null
      }
    },
    methods: {
      getDurations(duration) {    
        let dur = "";

        if(!duration || duration && !duration.days && !duration.hour && !duration.min) {
          return '--';
        }

        if(duration && duration.days) {
          dur += duration.days +'Day(s) ';
        }

        if(duration && duration.hour) {
          dur += duration.hour +'Hour(s) ';
        }

        if(duration && duration.min) {
          dur += duration.min +'Minute(s)';
        }

        return dur;
      },
      GetArrayElementValues(list, key) {

        if (typeof list == "undefined" || list == null || !Array.isArray(list) || !key) {
          return [];
        }

        let keyValues = [];

        list.map((item) => {
            if(item[key]) {
                keyValues.push(item[key]);
            }
        });
        return keyValues;
      }
    }  
  }
</script>

<style scoped>
  .md-table-head {
    text-align: left;
  }
  .md-table-cell {
    text-align: left;
  }
  .text-align {
    text-align: center
  }
</style>
