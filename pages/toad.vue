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
          <div style="display: flex">
            <lable style="margin-right: 10px; margin-top: 10px;">Sort by:</lable>
            <v-select
              :items="sortby"
              class="filter-select"
            ></v-select>
            <lable style="margin-right: 10px; margin-top: 10px;">Search</lable>
            <v-text-field class="filter-search" style="width: 250px;" label="Enter Token Name"></v-text-field>
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
            <farmitem></farmitem>
            <farmitem></farmitem>
            <farmitem></farmitem>
          </v-expansion-panels>
        </v-card>
      </template>
    </flipcard>
  </v-row>

</template>

<script>
import axios from 'axios'
import FlipCard from '../components/FlipCard.vue'
import FilterLayout from '../components/FilterLayout.vue'
import FarmItem from '../components/ToadFarmItem.vue'
import { padAddress, BEP20ABI, regularFarms } from '../farms_config.json'
export default {
  layout: 'toad',
  data () {
    return {
      farm_name: '',
      isV1: false,
      approved: false,
      stake_flag: false,
      sortby: ['Hot', 'APY', 'Daily ROI', 'Earned', 'Staked'],
      amount: '',
      intervals: {},
      myAddress: '',
      toadAddress: '',
      tokenAddress: '',
      contractAddress: '',
      tokenABI: [],
      contractABI: [],
      padInstance: null,
      tokenInstance: null,
      contractInstance: null,
      farm_img: '/padSwapLogo.svg',
      pool_size: '0.000',
      pool_value: '0.000',
      stake_value: '0.000',
      padPrice: '0.000',
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
      stats_card: false,
      loading: false,
      search: null,
      select: 'BNB-BUSD',
      items: []
    }
  },
  components: {
    flipcard: FlipCard,
    filterlayout: FilterLayout,
    farmitem: FarmItem
  },
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
      const loadWeb3 = setInterval(() => {
        this.items = regularFarms.farms
        this.padAddress = padAddress
        this.tokenABI = BEP20ABI
        const web3 = this.$parent.$parent.$parent.$parent.web3
        this.dataseed = this.$parent.$parent.$parent.$parent.dataseed
        this.padInstance = new this.dataseed.eth.Contract(this.tokenABI, this.padAddress)
        this.tokenInstance = new this.dataseed.eth.Contract(this.tokenABI, this.tokenAddress)
        this.contractInstance = new this.dataseed.eth.Contract(this.contractABI, this.contractAddress)
        this.loadNewInstances(this.select)
        if (web3 && this.contractAddress) {
          this.dataseed = this.$parent.$parent.$parent.$parent.dataseed
          this.padInstance = new this.dataseed.eth.Contract(this.tokenABI, this.padAddress)
          this.tokenInstance = new this.dataseed.eth.Contract(this.tokenABI, this.tokenAddress)
          this.contractInstance = new this.dataseed.eth.Contract(this.contractABI, this.contractAddress)
          this.web3 = web3
          console.log('clearing')
          clearInterval(loadWeb3)
        }
      }, 500)
      this.intervals['loadData'] = setInterval(async () => {
        this.myAddress = this.$parent.$parent.$parent.$parent.myAddress
        await this.getPrice()
        this.getStakedAmount()
        this.getDividends()
        this.getBalance()
        this.getStakeVal()
        this.checkAllowance()
        this.getLpPrice()
        this.getROI()
        this.getPoolInfo()
      }, 3000)
    },
    async loadNewInstances(select) {
      this.farm_name = this.select.replace('(old)', '').replace('(new)', '')
      const farm = this.items.find(i => i.name === select)
      if(farm.old) this.isV1 = true
      else this.isV1 = false
      this.tokenAddress = farm.acceptedToken
      this.contractAddress = farm.contract
      if(farm.abi) this.contractABI = farm.abi
      else this.contractABI = regularFarms.defaultAbi
      this.tokenInstance = new this.dataseed.eth.Contract(this.tokenABI, this.tokenAddress)
      this.contractInstance = new this.dataseed.eth.Contract(this.contractABI, this.contractAddress)
    },
    async getStakedAmount() {
      let amount = await this.contractInstance.methods.sharesOf(this.myAddress).call()
      this.staked_tokens = this.round(Number(amount)/1e18, 6)
    },
    async getDividends(){
      let amount = await this.contractInstance.methods.estimateRewardsOf(this.myAddress).call()
      if(amount < 0.000001) amount = 0
      this.rewards = this.round(Number(amount)/1e18, 4)
      this.rewardsUsd = '$' + this.round(Number(amount*this.padPrice)/1e18, 4)
    },
    async  getPrice() {
      const busdInstance = new this.dataseed.eth.Contract(this.tokenABI, '0xe9e7cea3dedca5984780bafc599bd69add087d56')
      const pairPadBalance = await this.padInstance.methods.balanceOf('0x2856aDD546E729425383173D4f65Bb3e23E796A4').call()
      const pairBusdBalance = await busdInstance.methods.balanceOf('0x2856aDD546E729425383173D4f65Bb3e23E796A4').call()
      this.padPrice = pairBusdBalance/pairPadBalance
    },
    async getPoolInfo() {
      let amount = await this.contractInstance.methods.farmPool().call()
      this.pool_size = this.miOrK(Number(amount)/1e18) + ' PAD'
      this.pool_value = '$'+ this.miOrK(Number(amount*this.padPrice)/1e18)
    },
    async getBalance() {
      let amount = await this.tokenInstance.methods.balanceOf(this.myAddress).call()
      this.token_balance = this.round(Number(amount)/1e18, 4)
    },
    async getStakeVal(){
      let amount = await this.contractInstance.methods.sharesOf(this.myAddress).call()
      this.stake_value = '$' + this.round(Number(amount*this.lpPrice)/1e18, 2)
    },
    async getLpPrice() {
      if(this.tokenAddress == '0x463e737d8f740395abf44f7aac2d9531d8d539e9') {
        const BUSDinstance = new this.dataseed.eth.Contract(this.tokenABI, '0xe9e7cea3dedca5984780bafc599bd69add087d56')
        const toadBusdInstance = new this.dataseed.eth.Contract(this.tokenABI, '0x15aff98391f928693f55b70b8f4d787031ad6f74')
        const toadInstance = new this.dataseed.eth.Contract(this.tokenABI, '0x463e737d8f740395abf44f7aac2d9531d8d539e9')
        let toadBusdBUSDBalance = await BUSDinstance.methods.balanceOf('0x15aff98391f928693f55b70b8f4d787031ad6f74').call()
        let toadBusdToadBalance = await toadInstance.methods.balanceOf('0x15aff98391f928693f55b70b8f4d787031ad6f74').call()
        this.lpPrice = toadBusdBUSDBalance/toadBusdToadBalance
      }
      const lpSupply = await this.tokenInstance.methods.totalSupply().call()
      const res = await axios.post('https://api.thegraph.com/subgraphs/name/toadguy/padswap-subgraph', {
          query: `
          {
              pairs(where: {id: "${this.tokenAddress}"}) {
                  id
                  reserveUSD
              }
          }
          `
      })
      const reserve = res.data.data.pairs[0].reserveUSD
      this.lpPrice = this.round(reserve/(lpSupply/1e18), 4)
    },
    async getROI() {
      const totalShares = await this.contractInstance.methods.totalSupply().call()
      const padBalance = await this.contractInstance.methods.farmPool().call()
      const roi = ((padBalance*this.padPrice*0.1)/(totalShares*this.lpPrice))*100

      this.roi = this.round(roi, 2) + '%'

      let initial = 100
      let apy = 0
      for(let i = 0; i<365; i++) {
          initial = initial + initial*roi/100
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
      else {
        this.approved = false
        this.approve_or_deposit = 'Approve'
      }
    },
    async approve() {
      const tokenInstance = new this.web3.eth.Contract(this.tokenABI, this.tokenAddress)
      let amount = '1000000'
      amount = this.web3.utils.toWei(amount, 'ether')
      const gasPrice = await this.web3.eth.getGasPrice()
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
      await contractInstance.methods.remove(amount).send(args)
    },
    async withdraw() {
      const contractInstance = new this.web3.eth.Contract(this.contractABI, this.contractAddress)
      const gasPrice = await this.web3.eth.getGasPrice()
      let args = {
        from: this.myAddress,
        gasPrice: gasPrice,
        value: 0,
      }
      await contractInstance.methods.harvest().send(args)
    },
  },
  watch: {
    loader () {
      const l = this.loader
      this[l] = !this[l]
      setTimeout(() => (this[l] = false), 3000)
      this.loader = null
    },
    select () {
      const l = this.select
      this[l] = !this[l]
      this.loadNewInstances(l)
    }
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
</style>
