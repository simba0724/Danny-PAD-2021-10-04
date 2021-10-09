<template>
  <v-row>
    <v-col
    cols="12">
      <div class="text-center">
        <v-btn-toggle
        style="margin-top:12px; background-color: white; color: black;"
        mandatory
        v-model="toggle_exclusive">
          <v-btn to="/toad" active-class='active-toad'>
            Toad
          </v-btn>
          <v-btn to="/pad">
            Pad
          </v-btn>
        </v-btn-toggle>
        <div style="margin-top: 62px;" class="pagelogo">
          <img src="/toad.svg" />
          <h1>Toad Farms</h1>
          <h5>Stake tokens to earn Toad and LP tokens.</h5>
        </div>
      </div>
    </v-col>
    <filterlayout>
      <template slot="front">
        <v-card class="farm-filter">
          <div style="display: flex;">
            <v-switch
              v-model="stake_flag"
              inset
              label="Staked"
              dense
              style="margin-left: 10px;"
              class="filter-switch"
            ></v-switch>
          </div>
          <div class="filter-right">
            <div class="filter-right-item">
              <p style="margin-right: 10px; margin-top: 10px; margin-bottom: 0px;">Sortby:</p>
              <v-select
                :items="sortby"
                class="filter-select"
              ></v-select>
            </div>
            <div class="filter-right-item">
              <p style="margin-right: 10px; margin-top: 10px; margin-bottom: 0px;">Search</p>
              <v-text-field class="filter-search" style="width: 250px;" label="Enter Token Name"></v-text-field>
            </div>
          </div>
        </v-card>
      </template>
    </filterlayout>

    <flipcard>
      <template slot="front">
        <v-card
          class="farm-card flip-card-front"
          elevation="8">
          <h3 class="title">Regular Farms</h3>
          <v-expansion-panels>
            <farmitem v-for="(item, i) in famr_list" :item="item" :key="i"></farmitem>
          </v-expansion-panels>
        </v-card>
      </template>
    </flipcard>
  </v-row>

</template>

<script>
import FlipCard from '../components/FlipCard.vue'
import FilterLayout from '../components/FilterLayout.vue'
import FarmItem from '../components/ToadFarmItem.vue'
import { toadFarmList } from '../farms_config.json'
export default {
  layout: 'toad',
  data () {
    return {
      famr_list: [],
      stake_flag: false,
      toggle_exclusive: 0,
      sortby: ['Hot', 'APY', 'Daily ROI', 'Earned', 'Staked']
    }
  },
  components: {
    flipcard: FlipCard,
    filterlayout: FilterLayout,
    farmitem: FarmItem
  },
  created () {
    this.famr_list = toadFarmList
  }
}
</script>
<style scoped>
  .v-item-group {
    border-radius: 25px !important;
  }

  .v-btn--is-elevated {
    padding: 13px 25px !important;
    border-radius: 25px !important;
    background-color: transparent !important;
    font-size: 14px !important;
    height: auto !important;
    color: black !important;
  }

  .v-btn--active {
    border-radius: 25px !important;
  }

  .active-toad {
    background-color: #00fc4c !important;
    color: black !important;
  }

  .active-pad {
    background-color: #F99DF3 !important;
    color: #920087 !important;
  }

  .theme--dark.v-btn--active:hover::before, .theme--dark.v-btn--active::before {
    border-radius: 25px !important;
    margin: 0px !important;
  }

  .pagelogo {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .pagelogo h1 {
    font-family: Roboto, sans-serif;
    font-style: normal;
    font-weight: 500;
    font-size: 36px;
    line-height: 44px;
    margin-top: 20px;
    letter-spacing: 0.25px;
  }

  .pagelogo h5 {
    font-family: Roboto, sans-serif;
    font-style: normal;
    font-weight: 500;
    font-size: 14px;
    line-height: 19px;
    margin-top: 10px;
  }

  .filter {
    max-width: 1200px;
    width: 100%;
  }

  .farm-filter {
    display: flex;
    justify-content: space-between;
    box-shadow: none !important;
  }

  .farm-filter .filter-toggle {
    background-color: #292D38;
    border: 3px solid #292D38 !important;
  }

  .farm-filter .filter-toggle .v-btn {
    color: white !important;
    padding: 10px 20px !important;
  }

  .farm-filter .filter-toggle .v-btn--active {
    background-color: white !important;
    color: black !important;
  }

  .farm-filter .v-input {
    margin-top: 0;
    padding-top: 0;
  }

  .farm-filter .v-input .v-input__control .v-input__slot {
    margin-bottom: 0;
    border: 0 !important;
  }

  .farm-filter .filter-switch .v-messages {
    display: none !important;
  }

  .v-input__control .v-text-field__details {
    display: none !important;
    width: 140px !important;
  }

  .v-application--is-ltr .v-messages {
    display: none !important;
  }

  .filter-select {
    margin-right: 30px;
    width: 170px !important;
  }

  .farm-title {
    font-size: 30px;
    padding-top: 25px;
    margin-bottom: 20px;
    font-weight: 500;
    letter-spacing: 2px;
  }
  .farm-card{
    border-radius: 11px;
    padding-bottom: 30px;
    padding: 10px;
    background-color: #0d0d0d;
  }
  .farm-card .title{
    color: #00fc4c;
    font-size: 16px !important;
    line-height: 16px !important;
    font-weight: 500;
    margin: 0;
    margin-left: 20px;
    margin-bottom: 10px;
  }
  .farm-rewards{
    font-size: 33px;
    font-weight: 600;
  }
  .farm-rewards-caption{
    margin-top: 5px;
    display: block;
    font-size: 15px;
    font-weight: normal;
    color: rgb(185, 181, 180);
    margin-bottom:30px;
  }
  .stats-line{
    margin: 7px 15px 7px;
    font-size: 17px;
    font-weight: 500;
    display: block;
    text-align: left;
    line-height: 40px;
  }
  .stats-line > img {
    margin-right: 4px;
    line-height: 40px;
    vertical-align: middle;
  }
  .stats-line-info{
    font-weight: bold;
    float: right;
    padding: 0px;
    line-height: 40px;
  }
  .theme--light.v-card > .text-center{
    color: #a3a09f !important;
  }
  @media all and (max-width: 959px) {
    .v-input {
      max-width: 96% !important;
      margin: auto;
    }
    .v-btn-toggle:not(.v-btn-toggle--dense) .v-btn.v-btn.v-size--default {
      height: 48px;
      min-height: 0;
      max-width: 32vw;
    }
    .add-remove-btns{
      width:96%;
      margin: auto;
    }
    .v-item-group > .v-btn.v-size--default {
      font-size: 0.75rem;
    }
  }
  .cool-btn{
    width: calc(100% - 20px) !important;
    margin-left: 10px;
    margin-right: 10px;
    border-radius: 11px;
    transition: all .5s ease 0s;
    background-position: left center;
    background-image: linear-gradient(51deg,rgb(159 68 228)0,rgb(75 205 255) 51%,rgb(159 68 228));
    background-size: 250%;
  }
  .cool-btn:hover{
    background-position: right center;
  }
  .theme--dark.farm-filter {
    background-color:#181D26;
  }
  .theme--dark.v-btn:hover::before {
    opacity: 0;
  }
  .v-btn-toggle {
      border-radius: 16px;
  }
  .v-btn-toggle > .v-btn.v-btn {border: none}
  .theme--dark.v-btn.v-btn--has-bg {
      background-color: #212429;
  }
  .more-info{
    width: 20px;
    float: right;
    margin-right: 20px;
    margin-top: 20px;
    margin-left: -40px;
    cursor: pointer;
  }
  .more-info:hover{
    opacity:0.5
  }
  .v-icon:focus::after {
      opacity: 0
  }
  .theme--light.farm-card > .text-center > svg { background-color: #f7f7f7 !important}
  .v-icon.v-icon.v-icon--link {
      z-index: 2;
  }
  .flip-container {
    max-width: 1200px;
    width: 100%;
  }
  .filter-right {
    display: flex;
  }
  .filter-right-item {
    display: flex;
  }
  @media screen and (max-width: 850px) {
    .filter {
      height: auto;
    }
    .farm-filter {
      flex-direction: column;
      display: flex;
    }
    .farm-filter .filter-right {
      flex-direction: column;
    }
    .filter-right-item {
      margin-bottom: 10px;
    }
  }
</style>
