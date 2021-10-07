<template>
  <v-expansion-panel>
    <v-expansion-panel-header>
      <div class="header-body">
        <div class="token-pair">
          <img src='/bnb-busd.svg'/>
          <p>{{item.symbol}}</p>
        </div>
        <div class="percent">
          <p>YEARLY APY</p>
          <h2>{{apy}}%</h2>
        </div>
        <div class="percent">
          <p>DAILY APY</p>
          <h2>0%</h2>
        </div>
        <div class="Earned">
          <p>PAD EARNED</p>
          <h2>0%</h2>
          <p class="tag">Earned Value: <span>$0</span></p>
        </div>
        <v-btn
          style="background-color: #00FC4C; border-radius: 20px; margin: auto 20px; color: black; font-size: 16px; font-weight: 600; height: 40px; letter-spacing: 0px;  padding: 10px 30px; text-transform: none;"
          >
          Enable
        </v-btn>
      </div>
    </v-expansion-panel-header>
    <v-expansion-panel-content>
      <div class="token-info">
        <p class="token-info-title">GET BNB-BUSD</p>
        <div class="token-info-item">
          <p class="token-info-title">{{item.symbol}} BALANCE:</p>
          <p>{{token_balance}}</p>
        </div>
        <div class="token-info-item">
          <p class="token-info-title">POOL SIZE:</p>
          <p>{{pool_size}} TOAD</p>
        </div>
        <div class="token-info-item">
          <p class="token-info-title">{{item.symbol}} STAKED:</p>
          <p>{{staked_tokens}}</p>
        </div>
        <div class="token-info-item">
          <p class="token-info-title">POOL VALUE:</p>
          <p>${{pool_value}}</p>
        </div>
        <div class="token-info-item">
          <p class="token-info-title">STAKED VALUE:</p>
          <p style="color: #00fc4c;">$0</p>
        </div>
        <div class="token-info-item">
          <p class="token-info-title">LP/TOKEN PRICE:</p>
          <p>$0</p>
        </div>
        <div class="token-info-item">
          <p class="token-info-title">EARNED VALUE:</p>
          <p style="color: #00fc4c;">$0</p>
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
          <p style="margin-bottom: 0; font-size: 12px; margin-top: 5px;">BNB-BUSD BALANCE: <span style="color: white">0</span></p>
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
import { toadTokenABI, toadAddress } from '../farms_config.json'

export default {
  props: ['item'],
  created () {
    this.loadInstances()
  },
  beforeRouteLeave (to, from, next) {
    for (const interval in this.intervals) {
      if (this.intervals[interval]) {
        clearInterval(this.intervals[interval])
      }
    }
    next()
  },
  data () {
    return {
      approved: false,
      amount: '',
      intervals: {},
      myAddress: '',
      toadAddress: '',
      tokenAddress: '',
      contractAddress: '',
      tokenABI: [],
      contractABI: [],
      toadInstance: null,
      tokenInstance: null,
      contractInstance: null,
      farm_title: 'TOAD-BNB',
      farm_token: 'TOAD-BNB',
      farm_img: '/bnb.png',
      toad_img: '/RegularToad-small.png',
      token_img: '/bnb.png',
      pool_size: '0.000',
      pool_value: '0.000',
      stake_value: '0.000',
      toadPrice: '0.000',
      lpPrice: '0.000',
      rewards: '0.000',
      rewardsUsd: '0.000',
      token_balance: '0.000',
      staked_tokens: '0.000',
      roi: '0.000',
      apy: '0.000',
      approve_or_deposit: 'APPROVE',
      toggle_exclusive: 0,
      width: 2,
      radius: 10,
      padding: 8,
      lineCap: 'round',
      gradient: ['#8bea4b', 'rgb(75 205 255)', 'rgb(159 68 228)'],
      value: [0, 2, 5, 9, 5, 10, 3, 5, 0, 0, 1, 8, 2, 9, 0],
      gradientDirection: 'top',
      fill: false,
      type: 'trend',
      autoLineWidth: false,
      stats_card: false
    }
  },
  methods: {
    flip () {
      this.$children[1].flip()
      setTimeout(() => { this.stats_card = true }, 200)
    },
    /*eslint-disable */
    round(num, dec) {
      num = Number(num).toFixed(20)
      if(!Number.isFinite(Number(num))) num = '0.0'
      num = Number(num).toFixed(20)
      const regex = new RegExp(`^-?\\d+(?:\\.\\d{0,${dec}})?`)
      let [int, decimals] = num.toString().replace(',', '.').split('.')
      const rounded = num.toString().match(regex)[0]
      return rounded
    },
    miOrK(num) {
      if(num>1e6) return this.round(num/1e6, 3) + 'M'
      else return this.round(num/1e3, 2) + 'K'
    },
    async loadInstances () {
      this.toadAddress = toadAddress
      this.tokenAddress = this.item.tokenAddress
      this.contractAddress = this.item.contractAddress
      this.contractABI = this.item.contractABI
      this.tokenABI = toadTokenABI
      const loadWeb3 = setInterval(() => {
        this.dataseed = this.$parent.$parent.$parent.$parent.$parent.$parent.$parent.$parent.dataseed
        this.toadInstance = new this.dataseed.eth.Contract(this.tokenABI, this.toadAddress)
        this.tokenInstance = new this.dataseed.eth.Contract(this.tokenABI, this.tokenAddress)
        this.contractInstance = new this.dataseed.eth.Contract(this.contractABI, this.contractAddress)
        const web3 = this.$parent.$parent.$parent.$parent.$parent.$parent.$parent.$parent.web3
        if (web3) {
          this.web3 = web3
          clearInterval(loadWeb3)
        }
      }, 500)
      this.intervals['loadData'] = setInterval(async () => {
        this.myAddress = this.$parent.$parent.$parent.$parent.$parent.$parent.$parent.$parent.myAddress
        await this.getPrice()
        this.getStakedAmount()
        this.getDividends()
        this.getBalance()
        this.getStakeVal()  
        this.checkAllowance()
        this.getROI()
        this.getPoolInfo()
      }, 3000)
    },
    async getStakedAmount() {
      let amount = await this.contractInstance.methods.balanceOf(this.myAddress).call()
      this.staked_tokens = this.round(Number(amount)/1e18, 4)
    },
    async getDividends(){
      let amount = await this.contractInstance.methods.estimateDividendsOf(this.myAddress, false).call()
      if(amount < 0.000001) amount = 0
      this.rewards = this.round(Number(amount)/1e18, 4)
      this.rewardsUsd = '$' + this.round(Number(amount*this.toadPrice)/1e18, 4)
    },
    async getPrice() {
      const BUSDinstance = new this.dataseed.eth.Contract(toadTokenABI, '0xe9e7cea3dedca5984780bafc599bd69add087d56')
      const toadBusdInstance = new this.dataseed.eth.Contract(toadTokenABI, '0x15aff98391f928693f55b70b8f4d787031ad6f74')
      let toadBusdBUSDBalance = await BUSDinstance.methods.balanceOf('0x15aff98391f928693f55b70b8f4d787031ad6f74').call()
      let toadBusdToadBalance = await this.toadInstance.methods.balanceOf('0x15aff98391f928693f55b70b8f4d787031ad6f74').call()
      this.toadPrice = this.round(toadBusdBUSDBalance/toadBusdToadBalance, 2)
    },
    async getPoolInfo(){
      let amount = await this.contractInstance.methods.dividendPool().call()
      this.pool_size = this.miOrK(Number(amount)/1e18) + ' TOAD'
      this.pool_value = '$'+ this.miOrK(Number(amount*this.toadPrice)/1e18)
    },
    async getBalance(){
      let amount = await this.tokenInstance.methods.balanceOf(this.myAddress).call()
      this.token_balance = this.round(Number(amount)/1e18, 4)
    },
    async getStakeVal(){
      let amount = await this.contractInstance.methods.balanceOf(this.myAddress).call()
      this.stake_value = '$' + this.round(Number(amount*this.lpPrice)/1e18, 2)
    },
    async getROI(){
      const instance = new this.web3.eth.Contract(toadTokenABI, '0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c')
      let contractBalance = await instance.methods.balanceOf('0x20A3DC9C2ac748e3684015209735b7CDd6CA6Ba5').call()
      let totalSupply = await this.tokenInstance.methods.totalSupply().call()
      let toadBalance = await this.toadInstance.methods.balanceOf('0x20A3DC9C2ac748e3684015209735b7CDd6CA6Ba5').call()
      let toadPriceBnb = contractBalance/toadBalance
      let lpPrice = contractBalance*2/totalSupply
      this.lpPrice = this.round(toadBalance*this.toadPrice*2/totalSupply, 2)
      let supply = await this.contractInstance.methods.totalSupply().call()
      let pool = await this.contractInstance.methods.dividendPool().call()
      let roi = pool*toadPriceBnb/(supply*100*lpPrice)*100

      this.roi = this.round(roi, 2) + '%'

      let initial = 100
      let apy = 0
      for(let i = 0; i<365; i++) {
        initial = initial + initial*roi/100
        roi = roi*0.99
        if(i == 364) {
          this.apy = this.round(initial-100, 2) + '%'
        }
      }
    },
    async checkAllowance(){
      const allowance = await this.tokenInstance.methods.allowance(this.myAddress, this.contractAddress).call()
      if(allowance>20000*1e18) {
        this.approved = true
        this.approve_or_deposit = 'Deposit'
      }
    },
    async approve() {
      const tokenInstance = new this.web3.eth.Contract(this.tokenABI, this.tokenAddress)
      let amount = '1000000'
      amount = this.web3.utils.toWei(amount, 'ether')
      const gasPrice = await this.web3.eth.getGasPrice()
      console.log(this.myAddress, this.contractAddress)
      let args = {
        from:this.myAddress,
        gasPrice:gasPrice,
        value: 0,
      }
      await tokenInstance.methods.approve(this.contractAddress, amount).send(args)
    },
    async deposit() {
      const contractInstance = new this.web3.eth.Contract(this.contractABI, this.contractAddress)
      let amount = this.web3.utils.toWei(this.amount.replace(',','.'), 'ether')
      const gasPrice = await this.web3.eth.getGasPrice()
      let args = {
        from: this.myAddress,
        gasPrice: gasPrice,
        value: 0,
      }
      await contractInstance.methods.deposit(amount).send(args)
    },
    async remove(){
      const contractInstance = new this.web3.eth.Contract(this.contractABI, this.contractAddress)
      let amount = this.web3.utils.toWei(this.amount.replace(',','.'), 'ether')
      const gasPrice = await this.web3.eth.getGasPrice()
      let args = {
        from: this.myAddress,
        gasPrice: gasPrice,
        value: 0,
      }
      await contractInstance.methods.sell(amount).send(args)
    },
    async withdraw() {
      const contractInstance = new this.web3.eth.Contract(this.contractABI, this.contractAddress)
      const gasPrice = await this.web3.eth.getGasPrice()
      let args = {
        from: this.myAddress,
        gasPrice: gasPrice,
        value: 0,
      }
      await contractInstance.methods.withdraw().send(args)
    }
  },
  watch: {
    loader () {
      const l = this.loader
      this[l] = !this[l]
      setTimeout(() => (this[l] = false), 3000)
      this.loader = null
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
  color: #00fc4c;
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
  color: #00fc4c;
}

.token-info {
  width: 50%;
  font-size: 14px;
  font-weight: 500;
  display: grid;
  grid-template-columns: repeat(2,minmax(0,1fr));
}
.token-info p{
  margin: 0;
  line-height: 1 !important;
}
.token-info-title {
  color: #00fc4c;
  width: 50%;
}
.token-info-item {
  display: flex;
}
.token-info-item .token-info-title {
  color: #979CA5;
  width: 150px;
  font-size: 14px !important;
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
