<template>
  <v-expansion-panel>
    <v-expansion-panel-header>
      <div class="header-body">
        <div class="token-pair">
          <img src='/bnb-busd.svg'/>
          <p>BNB-BUSD</p>
        </div>
        <div class="percent">
          <p>YEARLY APY</p>
          <h2>417.04%</h2>
        </div>
        <div class="percent">
          <p>DAILY APY</p>
          <h2>0.45%</h2>
        </div>
        <div class="Earned">
          <p>PAD EARNED</p>
          <h2>0.45%</h2>
          <p class="tag">Earned Value: <span>$11.3122</span></p>
        </div>
        <v-btn
          style="background-color: #00FC4C; border-radius: 20px; margin: auto 20px; color: black; font-size: 16px; font-weight: 600; height: 40px; letter-spacing: 0px;     text-transform: none;"
          >
          Enable
        </v-btn>
      </div>
    </v-expansion-panel-header>
    <v-expansion-panel-content>
      <div class="token-info">
        <p class="token-info-title">GET BNB-BUSD</p>
        <div class="token-info-item">
          <p style="color: #979CA5; width: 150px;">PAD-BNB BALANCE:</p>
          <p style="color: #FFF;">102.9179</p>
        </div>
        <div class="token-info-item">
          <p style="color: #979CA5; width: 150px;">POOL SIZE:</p>
          <p style="color: #FFF;">200.468M PAD</p>
        </div>
        <div class="token-info-item">
          <p style="color: #979CA5; width: 150px;">PAD-BNB STAKED:</p>
          <p style="color: #FFF;">3283.949974</p>
        </div>
        <div class="token-info-item">
          <p style="color: #979CA5; width: 150px;">POOL VALUE:</p>
          <p style="color: #FFF;">$2.60K</p>
        </div>
        <div class="token-info-item">
          <p style="color: #979CA5; width: 150px;">STAKED VALUE:</p>
          <p style="color: #FB53EF;">$470.91</p>
        </div>
        <div class="token-info-item">
          <p style="color: #979CA5; width: 150px;">LP/TOKEN PRICE:</p>
          <p style="color: #FFF;">$0.1434</p>
        </div>
        <div class="token-info-item">
          <p style="color: #979CA5; width: 150px;">EARNED VALUE:</p>
          <p style="color: #FB53EF;">$11.3122</p>
        </div>
      </div>
      <div class="token-form">
        <div class="token-form-body">
          <div style="display: flex; margin-bottom: 5px;">
            <p style="margin-bottom: 0px; margin-right: 10px;">Deposit</p>
            <p style="margin-bottom: 0px;">Withdraw</p>
          </div>
          <div style="display: flex;">
            <div class="token-form-input">
              <input />
              <button>Max</button>
            </div>
            <v-btn
              style="background-color: #00FC4C; border-radius: 20px; margin: auto 20px; color: black; font-size: 16px; font-weight: 600;"
              >
              Deposit
            </v-btn>
          </div>
          <p style="margin-bottom: 0; font-size: 12px; margin-top: 5px;">BNB-BUSD BALANCE: <span style="color: white">102.9179</span></p>
        </div>
        <div class="token-form-tag">
          <v-icon style="margin-right:5px;font-size: 14px;">fas fa-exclamation-circle</v-icon>
          <p style="margin-bottom: 0">V2 Farms: 1% fee when staking and unstaking, this fee goes to the PadSwap vault.</p>
        </div>
      </div>
    </v-expansion-panel-content>
  </v-expansion-panel>
</template>

<script>
import '@fortawesome/fontawesome-free/css/all.css'

export default {
  name: 'FarmItem',
  methods: {
    flip () { this.flipped = !this.flipped }
  },
  data () {
    return {
      flipped: false
    }
  }
}
</script>

<style>
.v-expansion-panel:not(:first-child)::after {
  border-top: none !important;
}
.v-expansion-panel {
  background-color: #181D26 !important;
  margin-bottom: 10px;
  margin-top: 0px !important;
  border-radius: 15px !important;
}
.v-expansion-panel-header {
  padding: 15px 30px;
}
.v-expansion-panel-content {
  background-color: #292D38 !important;
  min-height: 210px;
  padding: 30px;
  display: flex;
  border-radius: 0 0 15px 15px;
}
.v-expansion-panel-content__wrap{
  padding: 0px;
  display: flex;
}
.v-expansion-panels > *:last-child {
  border-radius: 15px;
}
.v-expansion-panels:not(.v-expansion-panels--accordion):not(.v-expansion-panels--tile) > .v-expansion-panel--active{
  border-radius: 15px;
}
.v-expansion-panel-header__icon .v-icon {
  color: #00FC4C !important;
}

.header-body {
  display: flex;
  justify-content: space-between;;
}
.token-pair {
  display: flex;
  align-items: center;
}
.token-pair img {
  width: 55px;
  margin-right: 15px;
}
.token-pair p{
  margin: 0px;
  color: #FB53EF;
  width: 100px;
}

.percent {
  display: flex;
  flex-direction: column;
  width: 180px;
  margin: auto 0;
}
.percent p {
  margin-bottom: 10px;
  font-size: 12px;
  color: #71767F;
  font-weight: 500;
}
.percent h2 {
  font-size: 24px;
}
.Earned {
  margin: auto 0;
  display: flex;
  flex-direction: column;
  width: 180px;
}
.Earned p {
  margin-bottom: 10px;
  font-size: 12px;
  color: #71767F;
  font-weight: 500;
}
.Earned h2 {
  font-size: 24px;
}
.Earned .tag {
  font-size: 12px;
  color: #71767F;
  font-weight: 400;
  margin-top: 5px;
}
.Earned .tag span {
  color: #FB53EF;
}

.token-info {
  width: 50%;
  font-size: 14px;
  font-weight: 500;
  display: grid;
  grid-template-columns: repeat(2,minmax(0,1fr));
}
.token-info-title {
  color: #FB53EF;
  width: 50%;
}
.token-info-item {
  display: flex;
}
.token-form {
  width: 50%;
  color: #B3B8C1;
}
.token-form-body {
  border: 1px solid #595E67;
  border-radius: 15px;
  height: 110px;
  padding: 10px 20px;
  display: flex;
  flex-direction: column;
}
.token-form-input {
  width: calc(100% - 125px);
  margin-right: 15px;
  background: #71767F;
  border: 1px solid #979CA5;
  box-sizing: border-box;
  border-radius: 8px;
  display: flex;
}
.token-form-input input {
  height: 40px;
  width: calc(100% - 50px);
  padding: 5px;
}
.token-form-input input:focus-visible {
  outline: 0px;
}
.token-form-input button {
  border-radius: 8px;
  padding: 4px 8px;
  background-color: #595E67;
  border: 1px solid #979CA5;
  font-size: 12px;
  color: white;
  height: 30px;
  margin: auto 0;
}
.token-form-tag {
  display: flex;
  margin-top: 5px;
  align-items: baseline;
}
</style>
