<template>
  <v-row>
    <v-col
    cols="12">
      <div class="text-center">
        <v-btn-toggle
        style="margin-top:35px"
        mandatory
        v-model="toggle_exclusive">
          <v-btn to="/">
            FARMS
          </v-btn>
          <v-btn to="/lpfarms">
            LP FARMS
          </v-btn>
          <v-btn to="/partnerfarms">
            PARTNER FARMS
          </v-btn>
          <v-btn to="/vault">
            VAULT
          </v-btn>
        </v-btn-toggle>
      </div>
    </v-col>
    <v-col cols="12" lg="6" md="8" offset-lg="3" offset-md="2">
    <flipcard>
      <template slot="front">
        <v-card
          class="farm-card flip-card-front"
          elevation="8">
          <div class="text-center">
            <v-icon class="more-info" @click="flip()">
              mdi-chart-bar
            </v-icon>
            <v-autocomplete
              auto-select-first
              style="padding-top:55px; margin-bottom:25px"
              v-model="select"
              :items="items"
              item-text="name"
              item-contract="contract"
              spellcheck="false"
              flat
              label="Select a Farm"
              solo-inverted
            ></v-autocomplete>
            <img class="farm-img" :src="farm_img" height="70px">
            <div class="text-h5">Donate LPS</div>
            <div class="farm-description" style="margin-bottom: 20px;">Donate LPS to make your project rug-proof.</div>
          </div>
          <v-row
          justify="center"
          style="margin-top:10px">
            <v-col
            cols="12"
            md="3">
              <div class="text-center">
                <v-btn
                @click="approved ? deposit() : approve()"
                class="add-remove-btns cool-btn"
                color="primary"
                elevation="0">
                  {{approve_or_deposit}}
                </v-btn>
              </div>
            </v-col>
            <v-col
            cols="12"
            md="6">
              <v-text-field
                class="vault-input"
                style="min-height:36px"
                label="0.000"
                solo
                v-model="amount"
              ></v-text-field>
            </v-col>
            <v-col
            cols="12"
            md="3">
              <div class="text-center">
                <v-btn
                @click="setmax()"
                class="add-remove-btns cool-btn"
                color="primary"
                elevation="0">
                  MAX AMOUNT
                </v-btn>
              </div>
            </v-col>
          </v-row>
        </v-card>
      </template>
      <template slot="back">
        <v-card
          class="farm-card flip-card-front"
          elevation="8">
          <div class="text-center">
            <v-icon class="more-info" @click="flip()">
              mdi-share
            </v-icon>
            <div class="farm-title ">Farm Stats</div>
            <img class="farm-img" :src="farm_img" height="70px">
            <div class="farm-rewards">{{rewards}}</div>
            <span class="farm-rewards-caption">{{select}} Earned</span>
            <h5 class="stats-line">
              <img :src="farm_img">
              Pool Size
              <span class="stats-line-info">{{pool_size}}</span>
            </h5>
            <v-divider style="margin-left: 2%; width: 96%;"></v-divider>
            <h5 class="stats-line">
              <img :src="farm_img">
              Pool Value
              <span class="stats-line-info">{{pool_value}}</span>
            </h5>
            <v-divider style="margin-left: 2%; width: 96%;"></v-divider>
            <h5 class="stats-line">
              <img :src="farm_img">
              LP or Token Price
              <span class="stats-line-info">${{lpPrice}}</span>
            </h5>
            <v-divider style="margin-left: 2%; width: 96%;"></v-divider>
            <h5 class="stats-line">
              <img :src="farm_img">
              Your Stake
              <span class="stats-line-info">{{stake_value}}</span>
            </h5>
            <v-divider style="margin-left: 2%; width: 96%;"></v-divider>
            <h5 class="stats-line">
              <img :src="farm_img">
              Your Rewards
              <span class="stats-line-info">{{rewardsUsd}}</span>
            </h5>
            <v-divider style="margin-left: 2%; width: 96%;"></v-divider>
            <h5 class="stats-line">
              Toad Price
            </h5>
            <v-sparkline
              v-if="stats_card"
              style="
              background: #0000004a;
              margin: 0px 12px -10px;
              border: solid #ffffff21 1px;
              border-radius: 10px;"
              :value="value"
              :gradient="gradient"
              :smooth="radius || false"
              :padding="padding"
              :line-width="width"
              :stroke-linecap="lineCap"
              :gradient-direction="gradientDirection"
              :fill="fill"
              :type="type"
              :auto-line-width="autoLineWidth"
              auto-draw
            ></v-sparkline>
          </div>
        </v-card>
      </template>
    </flipcard>
    </v-col>
  </v-row>

</template>

<script>
import axios from 'axios'
import FlipCard from '../components/FlipCard.vue'
import { padAddress, BEP20ABI, lpFarms } from '../farms_config.json'
export default {
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
      select: 'PAD-TOAD',
      items: []
    }
  },
  components: {
    flipcard: FlipCard
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
      num = num.toFixed(20)
      if(!Number.isFinite(Number(num))) num = '0.0'
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
        this.items = lpFarms.farms
        this.padAddress = padAddress
        this.tokenABI = BEP20ABI
        const web3 = this.$parent.$parent.$parent.$parent.web3
        this.dataseed = this.$parent.$parent.$parent.$parent.dataseed
        if(this.$route.query.farm) this.loadNewInstances(this.$route.query.farm.toUpperCase())
        else this.loadNewInstances('')
        this.padInstance = new this.dataseed.eth.Contract(this.tokenABI, this.padAddress)
        this.tokenInstance = new this.dataseed.eth.Contract(this.tokenABI, this.tokenAddress)
        this.contractInstance = new this.dataseed.eth.Contract(this.contractABI, this.contractAddress)
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
        this.getBalance()
        this.checkAllowance()
      }, 3000)
    },
    async loadNewInstances(select) {
      let farm = this.items.find(i => i.name === select)
      if(!farm) {
        farm = this.items.find(i => i.name === 'PAD-TOAD')
        this.select = 'PAD-TOAD'
      } else {
        this.select = select
      }
      this.tokenAddress = farm.acceptedToken
      this.contractAddress = farm.contract
      if(farm.abi) this.contractABI = farm.abi
      else this.contractABI = lpFarms.defaultAbi
      this.tokenInstance = new this.dataseed.eth.Contract(this.tokenABI, this.tokenAddress)
      this.contractInstance = new this.dataseed.eth.Contract(this.contractABI, this.contractAddress)
      return
    },
    async getStakedAmount() {
      let amount = await this.contractInstance.methods.sharesOf(this.myAddress).call()
      this.staked_tokens = this.round(Number(amount)/1e18, 6)
    },
    async getDividends(){
      let amount = await this.contractInstance.methods.estimateRewardsOf(this.myAddress).call()
      if(amount < 0.000001) amount = 0
      this.rewards = this.round(Number(amount)/1e18, 4)
      this.rewardsUsd = '$' + this.round(Number(amount*this.lpPrice)/1e18, 4)
    },
    async  getPrice() {
    const busdInstance = new this.dataseed.eth.Contract(this.tokenABI, '0xe9e7cea3dedca5984780bafc599bd69add087d56')
    const pairPadBalance = await this.padInstance.methods.balanceOf('0x2856aDD546E729425383173D4f65Bb3e23E796A4').call()
    const pairBusdBalance = await busdInstance.methods.balanceOf('0x2856aDD546E729425383173D4f65Bb3e23E796A4').call()
    this.padPrice = pairBusdBalance/pairPadBalance
    },
    async getPoolInfo() {
      console.log(this.contractInstance.methods)
      let amount = await this.contractInstance.methods.dividendPool().call()
      this.pool_size = this.miOrK(Number(amount)/1e18) + ' LP\'s'
      this.pool_value = '$'+ this.miOrK(Number(amount*this.lpPrice)/1e18)
    },
    setmax(){
      this.amount = this.token_balance
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
      const lpBalance = await this.contractInstance.methods.dividendPool().call()
      let roi = ((lpBalance*0.0075)/(totalShares))*100
      
      this.roi = this.round(roi, 2) + '%'
      
      let initial = 100
      let apy = 0
      for(let i = 0; i<365; i++) {
          initial = initial + initial*roi/100
          roi = roi - (roi*0.0075)
          if(i == 364) {
            this.apy = this.round(initial-100, 2) + '%'
          }
      }
    },
    async checkAllowance(){
      const allowance = await this.tokenInstance.methods.allowance(this.myAddress, this.contractAddress).call()
      if(allowance>20000*1e18) {
        this.approved = true
        this.approve_or_deposit = 'DONATE'
      }
    },
    async approve() {
      const tokenInstance = new this.web3.eth.Contract(this.tokenABI, this.tokenAddress)
      let amount = '1000000000'
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
      await contractInstance.methods.donateToPool(amount).send(args)
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
    async reinvest() {
      const contractInstance = new this.web3.eth.Contract(this.contractABI, this.contractAddress)
      const gasPrice = await this.web3.eth.getGasPrice()
      let args = {
        from: this.myAddress,
        gasPrice: gasPrice,
        value: 0,
      }
      await contractInstance.methods.reinvest().send(args)
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
  .farm-title {
    font-size: 30px;
    padding-top: 25px;
    margin-bottom:20px;
    font-weight: 500;
    letter-spacing: 2px;
  }
  .farm-card{
    border-radius: 11px;
    padding-bottom: 30px;
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
  .theme--dark.v-card {
    background-color:rgb(33, 36, 41);
  }
  .theme--dark.v-btn:hover::before {
    opacity: 0;
  }
  .theme--dark.v-btn--active:hover::before, .theme--dark.v-btn--active::before {
    opacity: 0.18;
    border-radius: 12px;
    padding: 9px;
    margin: 3px;
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
