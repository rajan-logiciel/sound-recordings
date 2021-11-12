<template>
  <div class="page-container">
    <md-toolbar class="md-primary">
      <span class="md-title">Sound Recordings</span>
      <div class="md-toolbar-section-end">
        <md-button @click="openPopup()">Add</md-button>
      </div>
    </md-toolbar>

    <!--Add Recording Popup-->
    <md-drawer class="md-right" :md-active.sync="showSidepanel">
      <AddSoundRecordings 
        v-on:add-sound="addSoundRecordings"
        v-on:popup-close="popupClose"/>
    </md-drawer>
    
    <!--List Of recording-->
    <md-content>
      <SoundRecordingList 
        v-bind:soundRecords="soundRecords" 
        v-on:search-sound-recording="searchSoundRecording" 
        v-on:del-sound-recording="deleteSoundRecordings"/>
    </md-content>
  </div>
</template>

<script>

  /**Import Files**/
  import AddSoundRecordings from './AddSoundRecordings.vue';
  import SoundRecordingList from './SoundRecordingList.vue';
  import {GetSoundRecordingList} from '../config.js';

  /**
  *set lower text
  */
  const toLower = text => {
    return text.toString().toLowerCase()
  }

  export default {
    name: 'SoundRecordings',
    components: {
      AddSoundRecordings,
      SoundRecordingList
    },
    data: () => ({
      showSidepanel: false,
      soundRecords: GetSoundRecordingList,
      oldSoundRecord: GetSoundRecordingList
    }),
    methods: {
      openPopup() {
        this.showSidepanel = true;
        document.body.style.overflow = 'hidden';
      },
      addSoundRecordings(newTodo) {    
        this.soundRecords.unshift(newTodo);
        this.oldSoundRecord = this.soundRecords;
        this.showSidepanel = false;
      },
      deleteSoundRecordings(id){
        this.soundRecords = this.soundRecords.filter((data) => data.id !== id);
        this.oldSoundRecord = this.soundRecords;
      },
      popupClose() {
        this.showSidepanel = false;
      },
      searchSoundRecording(search) {

        if(!search) {
          this.soundRecords = this.oldSoundRecord;
          return
        }
        
        this.soundRecords = this.oldSoundRecord.filter(item => toLower(item.work_title).includes(toLower(search)))
      } 
    }
  }
</script>

<style scoped>
  .drawer - content {
    height: 800px;
    width: 300 px;
    background: orange;
    overflow - y: auto;
  }
  .page-container {
    min-height: 970px;
    overflow: hidden;
    position: relative;
    border: 1px solid rgba(#000, .12);
  }
  .md-drawer {
    width: 550px;
    max-width: calc(100vw - 125px);
  }
  .md-content {
    padding: 16px;
    min-height: 900px;
  }
</style>
