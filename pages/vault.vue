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
            <div class="farm-title">VAULT</div>
            <img class="farm-img" :src="farm_img" height="70px">
            <div class="farm-description ">Burn PAD to redeem its backing</div>
            <v-progress-circular
              :size="92"
              :width="9"
              class="loader"
              indeterminate
              color="#6ab30f"
            ></v-progress-circular>
            <div class="loader-text">Loading vault data...</div>
            <div v-for="(balance, token) in vaultData" :key="token.address">
              <h5 class="stats-line">
                {{token}}
                <span class="stats-line-info">{{miOrK(balance)}}</span>
              </h5>
              <v-divider style="margin-left: 2%; width: 96%;"></v-divider>
            </div>
          </div>
          <v-row
          justify="center"
          style="margin-top:10px">
            <v-col
            class="tt1"
            cols="12"
            md="3">
              <div class="text-center">
                <v-btn
                @click="approved ? burn() : approve()"
                class="add-remove-btns cool-btn"
                color="primary"
                elevation="0">
                  {{approve_or_burn}}
                </v-btn>
              </div>
            </v-col>
            <v-col
            class="tt2"
            cols="12"
            md="6">
              <v-text-field
                style="min-height:36px"
                class="vault-input"
                label="0.000"
                solo
                v-model="amount"
              ></v-text-field>
            </v-col>
            <v-col
            class="tt3"
            cols="12"
            md="3">
              <v-text-field
                style="min-height:36px"
                class="vault-input"
                label="0.000"
                disabled
                solo
                v-model="expected"
              ></v-text-field>
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
            <span class="farm-rewards-caption">Toad Earned</span>
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
              <span class="stats-line-info">{{lpPrice}}</span>
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
import { padAddress, BEP20ABI, vault } from '../farms_config.json'
export default {
  data () {
    return {
      approved: false,
      amount: '',
      intervals: {},
      myAddress: '',
      tokenAddress: '',
      contractAddress: '',
      tokenABI: [],
      contractABI: [],
      padInstance: null,
      pairs: [],
      tokens: [],
      vaultData: {},
      contractInstance: null,
      farm_img: '/padSwapLogo.svg',
      approve_or_burn: 'APPROVE',
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
  components: {
    flipcard: FlipCard
  },
  created () {
    this.loadContractInstances()
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
      num = Number(num)
      num = num.toFixed(20)
      if(!Number.isFinite(Number(num))) num = '0.0'
      const regex = new RegExp(`^-?\\d+(?:\\.\\d{0,${dec}})?`)
      let [int, decimals] = num.toString().replace(',', '.').split('.')
      const rounded = num.toString().match(regex)[0]
      return rounded
    },
    miOrK(num) {
      if(num>1e6) return this.round(num/1e6, 3) + 'M'
      else if(num>1e3) return this.round(num/1e3, 2) + 'K'
      else return this.round(num, 2)
    },
    async loadContractInstances () {
      const loadWeb3 = setInterval(() => {
        this.padAddress = padAddress
        this.tokenABI = BEP20ABI
        this.contractAddress = vault.address
        this.contractABI = vault.abi
        this.pairs = vault.pairs
        this.tokens = vault.tokens
        const web3 = this.$parent.$parent.$parent.$parent.web3
        this.dataseed = this.$parent.$parent.$parent.$parent.dataseed
        this.padInstance = new this.dataseed.eth.Contract(this.tokenABI, this.padAddress)
        this.contractInstance = new this.dataseed.eth.Contract(this.contractABI, this.contractAddress)
        if (web3 && this.contractAddress) {
          this.dataseed = this.$parent.$parent.$parent.$parent.dataseed
          this.padInstance = new this.dataseed.eth.Contract(this.tokenABI, this.padAddress)
          this.contractInstance = new this.dataseed.eth.Contract(this.contractABI, this.contractAddress)
          this.web3 = web3
          console.log('clearing')
          clearInterval(loadWeb3)
        }
      }, 500)
      this.intervals['loadData'] = setInterval(async () => {
        this.myAddress = this.$parent.$parent.$parent.$parent.myAddress
        this.getVaultLpValue()
        this.calculateBackingPerPad()
      }, 5000)
    },
    async getVaultLpValue() {
      const json = {TOTAL: 0}
      json['MARKET CAP'] = await this.getMarketCap()
      for(let i = 0; i < this.pairs.length; i++) {
        const pair = this.pairs[i]
        const lpContract = new this.dataseed.eth.Contract(this.tokenABI, pair)
        const vaultBalance = await lpContract.methods.balanceOf(this.contractAddress).call()
        const lpSupply = await lpContract.methods.totalSupply().call()
        const res = await axios.post('https://api.thegraph.com/subgraphs/name/toadguy/padswap-subgraph', {
            query: `
            {
                pairs(where: {id: "${pair}"}) {
                    id
                    reserveUSD
                    token0{symbol}
                    token1{symbol}
                }
            }  
            `
        })
        
        const token0 = res.data.data.pairs[0].token0.symbol
        const token1 = res.data.data.pairs[0].token1.symbol
        const pairval = (res.data.data.pairs[0].reserveUSD/lpSupply)*vaultBalance
        json.TOTAL = Number(json.TOTAL)+pairval
        if(json[token0]) json[token0] = Number(json[token0]) + pairval/2
        else json[token0] = pairval/2
        if(json[token1]) json[token1] = Number(json[token1]) + pairval/2
        else json[token1] = pairval/2
        if(i ===  this.pairs.length-1) {
            for(let j = 0; j < this.tokens.length; j++){
                const res = await axios.post('https://api.thegraph.com/subgraphs/name/toadguy/padswap-subgraph', {
                    query: `
                    {
                        pairs(where: {id: "${this.tokens[j]}"}) {
                            token0{
                                id
                                symbol
                            }
                            token1{
                                id
                                symbol
                            }
                            token0Price
                            token1Price
                        }
                    }  
                    `
                })
                console.log(res.data)
                const _token0 = res.data.data.pairs[0].token0
                const _token1 = res.data.data.pairs[0].token1
                if (res.data.data.pairs[0].token0.id == '0xe9e7cea3dedca5984780bafc599bd69add087d56') {
                    const otherToken = new this.dataseed.eth.Contract(this.tokenABI, _token1.id)
                    const balance = await otherToken.methods.balanceOf(this.contractAddress).call()
                    json[_token1.symbol] = json[_token1.symbol] + (balance*res.data.data.pairs[0].token0Price/1e18)
                    json.TOTAL = Number(json.TOTAL) + (balance*res.data.data.pairs[0].token0Price/1e18)
                    
                } else {
                    const otherToken = new this.dataseed.eth.Contract(this.tokenABI, _token0.id)
                    const balance = await otherToken.methods.balanceOf(this.contractAddress).call()
                    json[_token0.symbol] = json[_token0.symbol] + (balance*res.data.data.pairs[0].token1Price/1e18)
                    json.TOTAL = Number(json.TOTAL) + (balance*res.data.data.pairs[0].token1Price/1e18)
                }
                if(j === this.tokens.length-1) {
                  if(Object.keys(this.vaultData).length === 0) {
                    document.getElementsByClassName('loader')[0].setAttribute('style', 'display:none')
                    document.getElementsByClassName('loader-text')[0].setAttribute('style', 'display:none')
                  }
                  this.vaultData = json
                }
            }
        }
      }
    },
    async  getMarketCap() {
      const busdInstance = new this.dataseed.eth.Contract(this.tokenABI, '0xe9e7cea3dedca5984780bafc599bd69add087d56')
      const pairPadBalance = await this.padInstance.methods.balanceOf('0x2856aDD546E729425383173D4f65Bb3e23E796A4').call()
      const pairBusdBalance = await busdInstance.methods.balanceOf('0x2856aDD546E729425383173D4f65Bb3e23E796A4').call()
      const padSupply = await this.padInstance.methods.totalSupply().call()
      const padPrice = pairBusdBalance/pairPadBalance
      const mcap = padPrice * padSupply / 1e18
      return mcap
    },
    async calculateBackingPerPad() {
      let padSupply = await this.padInstance.methods.totalSupply().call()
      padSupply = padSupply/1e18
      console.log(this.vaultData)
      this.backingPerPad = this.vaultData.TOTAL/padSupply
    },
    async checkAllowance(){
      const allowance = await this.tokenInstance.methods.allowance(this.myAddress, this.contractAddress).call()
      if(allowance>20000*1e18) {
        this.approved = true
        this.approve_or_burn = 'REDEEM'
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
    async redeem() {
      const contractInstance = new this.web3.eth.Contract(this.contractABI, this.contractAddress)
      let amount = this.web3.utils.toWei(this.amount.replace(',','.'), 'ether')
      const gasPrice = await this.web3.eth.getGasPrice()
      let args = {
        from: this.myAddress,
        gasPrice: gasPrice,
        value: 0,
      }
      await contractInstance.methods.redeemBacking(amount).send(args)
    },
  },
  watch: {
    amount () {
      const l = this.value
      this[l] = !this[l]
      if(this.backingPerPad*this.amount < 0.01) return this.expected = '< 0.01 BUSD'
      this.expected = '≈' + this.round(this.backingPerPad*this.amount, 2) + ' BUSD'
    },
  }
}
</script>
<style scoped>
  .farm-title {
    font-size: 30px;
    padding-top: 25px;
    margin-bottom: 15px;
    font-weight: 500;
    letter-spacing: 2px;
  }
  .farm-description {
    font-size: 17px;
    margin-top: 20px;
    margin-bottom: 50px;
    font-weight: 400;
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
.tt1 {padding-right:0px; padding-left:20px}
.tt2 {padding-right:10px; padding-left:10px}
.tt3 {padding-right:30px; padding-left:0}
 @media all and (max-width: 959px) {
  .tt1 {padding-right:20px; padding-left:20px}
  .tt2 {padding-right:20px; padding-left:20px}
  .tt3 {padding-right:20px; padding-left:20px}
  .v-item-group > .v-btn.v-size--default {
    font-size: 0.75rem;
  }
}
.loader{
  margin-top:-300px
}
.loader-text{
  font-size:1.2rem;
  margin-bottom:50px;
}
</style>
