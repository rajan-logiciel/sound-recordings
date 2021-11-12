<template>
  <form @submit="addSound" class="from">
    <md-toolbar class="md-transparent" md-elevation="0">
      <span class="md-title m0">Add Sound Recording</span>
      <div class="md-toolbar-section-end">
        <span @click="closePopup()" class="close" aria-label="Close">
          <md-icon>close</md-icon>
        </span>
      </div>
    </md-toolbar>
    <md-card-content>
      <div class="md-layout md-gutter">
        <div class="md-layout-item">
          <md-field :class="this.errors.work_title ? 'md-invalid' : ''">
            <label for="title">Title</label>
            <md-input 
              name="title" 
              id="title"
              v-model="frm.work_title" 
              autocomplete="given-name"/>
            <span 
              class="md-error"
              :class="this.errors.work_title ? 'md-invalid' : ''" 
              v-if="this.errors.work_title">Title is required</span>
          </md-field>
        </div>
      </div> 
      <div class="md-layout md-gutter">
        <div class="md-layout-item">
          <md-field :class="this.errors.artist ? 'md-invalid' : ''">
            <label for="artist">Artist</label>
            <md-input 
              name="artist" 
              id="artist"
              v-model="frm.artist" 
              autocomplete="given-name"/>
            <span 
              class="md-error"
              :class="this.errors.artist ? 'md-invalid' : ''" 
              v-if="this.errors.artist">Artist is required</span>
          </md-field>
        </div>
      </div>
      <div class="md-layout-item interested-parties">
        <md-field 
          :class="[(isOpen && frm.interested_parties.length == 0) ? 'md-focused' : '', frm.interested_parties.length > 0 ? 'md-has-value':'']"
        >
          <label :class="(isOpen) || frm.interested_parties.length > 0 ? '' :'ml6'" for="interestedParties">Interested Parties</label>
          <vue-taggable-select
            v-model="frm.interested_parties"
            option-label="full_name"
            :multiple="true"
            placeholder="Search Interested Parties"
            :options="interestedParties"
            @click.native="selectList(true)">
          </vue-taggable-select>
        </md-field>
      </div>
      <div class="md-layout">
          <md-field class="md-layout-item md-size-30 mr">
            <label for="title">Days</label>
            <md-input 
              type="number"
              name="days" 
              id="days"
              v-model="frm.durations.days"/>
          </md-field>    
          <md-field 
            class="md-layout-item md-size-30 mr" 
            :class="this.errors.durations.hour ? 'md-invalid' : ''" >
            <label for="hour">Hours</label>
            <md-input 
              type="number"
              name="hour" 
              id="hour"
              v-model="frm.durations.hour"/>
            <span 
              class="md-error"
              :class="this.errors.durations.hour ? 'md-invalid' : ''" 
              v-if="this.errors.durations.hour">Enter Hours between 1 to 24.</span>
          </md-field>    
          <md-field class="md-layout-item md-size-30 mr" :class="this.errors.durations.min ? 'md-invalid' : ''">
            <label for="min">Mins</label>
            <md-input 
              type="number"
              name="min" 
              id="min"
              v-model="frm.durations.min"/>
            <span 
              class="md-error"
              :class="this.errors.durations.min ? 'md-invalid' : ''" 
              v-if="this.errors.durations.min">Enter Minutes between 1 to 60.</span>      
          </md-field>
      </div>   
    </md-card-content>
    </br>  
    <md-card-actions class="action-btn">
      <md-button type="submit" class="md-raised md-primary">Save</md-button>
      <md-button class="md-raised" @click="closePopup()">Cancel</md-button>
    </md-card-actions> 
  </form>   
</template>

<script>

  /**Import Files**/
  import {InterestedParties} from '../config.js';
  import VueTaggableSelect from "vue-taggable-select";
  
  export default {
    name: 'AddSoundRecordings',
    components: { VueTaggableSelect },
    data() {
      return {
        isOpen:false,
        frm: {durations:{}, interested_parties:[]},
        interestedParties: InterestedParties,
        errors: {
          work_title: false,
          artist: false,
          durations:{hour:'', min:''}
        }
      }
    },
    methods: {
      selectList(onFormClick) {
        
        this.isOpen = onFormClick;

        if(this.frm.interested_parties.length > 0) {
          this.isOpen = false;
        }
      },
      resetData() {
        this.frm = {durations:{}, interested_parties:[]};
        this.errors = {durations:{hour:'', min:''}};
        document.body.style.overflow = 'auto';
       
      },
      isValidation() {

        let isValid = true;
        this.errors = {durations:{hour:'', min:''}};
        if(!this.frm.work_title) {
          this.errors.work_title = true;
          isValid = false;
        }

        if(!this.frm.artist) {
          this.errors.artist = true;
          isValid = false;
        }

        if (this.frm.durations.hour && this.frm.durations.hour > 24) {
            isValid = false;
            this.errors.durations.hour = true;
        }

        if (this.frm.durations.min && this.frm.durations.min > 60) {
            isValid = false;
            this.errors.durations.min = true;
        }
        
        return isValid;
      },
      addSound(e) {

        e.preventDefault();

        if(!this.isValidation()) {return}
        const data = {...this.frm , id:Date.now(), iswc: Date.now()}

        // Send up to parent
        this.$emit('add-sound', data);
        this.resetData();
      },
      closePopup() {
        this.resetData();
        this.$emit('popup-close');
      }
    }
  }
</script>

<style hidden>
 
  .from {
    height: 970px;
    display: flex;
    flex-direction: column;
  }
  .action-btn{
    margin-top: auto;
  }
  .m0 {
    margin: 0px;
  }
  .mr {
    margin-right: 15px;
  }
  .ml6 {
    margin-left: 6px;
  }
  .md-error {
    font-size: 10px;
  }
  input[type=number] {
    -moz-appearance: textfield;
  }
  .taggable-select-wrapper {
    width:100%;
  } 
  .interested-parties ul.items-center {
    overflow: hidden !important;
    padding-bottom: 10px !important;
  }

  .interested-parties ul.items-center li:first-child {
     margin-left: 0 !important;
  }
</style>
