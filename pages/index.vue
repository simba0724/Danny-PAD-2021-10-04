<template>
  <div class="main">
    <div style="margin-top: 80px;" class="pagelogo">
      <img src="/BlueToad.png" />
      <h1 style="text-align: center;">Toad.Network</h1>
      <h5 style="text-align: center;">Defi Ecosystem</h5>
      <v-btn to="/toad" class="logo-link">Launch App</v-btn>
      <div class="link-list">
        <v-btn href="/pad_audit_report.pdf"><img src="/icons/guard.svg" /> Audit Report</v-btn>
        <v-btn to="/">FAQ</v-btn>
        <v-btn href="https://www.dropbox.com/s/bng5e1bq2u03bk6/PAD%20WHITEPAPER.PDF">Whitepaper</v-btn>
      </div>
      <div class="token-info">
        <div class="token-info-item text-center">
          <h1>1.96M</h1>
          <p>Toad Market Cap</p>
        </div>
        <div class="token-info-item text-center">
          <h1>1.48M</h1>
          <p>Locked Liquidity</p>
        </div>
        <div class="token-info-item text-center">
          <h1>153.6K</h1>
          <p>Circulating Supply</p>
        </div>
        <div class="token-info-item text-center">
          <h1>195K</h1>
          <p>Max Supply</p>
        </div>
      </div>
    </div>
    <v-row
      justify="center"
      class="middle"
    >
      <v-col
        cols="12"
        md="6"
        class="left"
      >
        <h1 style="font-size: 36px; font-weight: 500; line-height: 44px; margin-bottom: 30px;">Toad Network</h1>
        <p style="font-size: 16px; line-height: 20px; letter-spacing: 0.25px; color: #979CA5; margin-bottom: 30px;">Toad.Network is an evergrowing defi ecosystem, providing solutions for crypto users, projects and setting new standards for fairer token launches.</p>
        <div class="link-list">
          <v-btn to="/pad"><img src="/favicon.png" /> Buy on PadSwap</v-btn>
          <v-btn to="/"><img src="/icons/pancake.png" /> Buy on PancakeSwap</v-btn>
          <v-btn to="/"><img src="/icons/xt.png" /> Buy on XT.com</v-btn>
        </div>
      </v-col>
      <v-col
        cols="12"
        md="6"
        class="right"
      >
        <router-link to="/" class="card">
          <div>
            <img src="/middle_image/toadfarms.svg" />
            <h3>Toad Farms</h3>
            <p>Earn yield in Toad by providing liquidity.</p>
            <v-icon>fa fa-arrow-right</v-icon>
          </div>
        </router-link>
        <router-link to="/" class="card">
          <div>
            <img src="/middle_image/toadlpfarms.svg" />
            <h3>Toad LP Farms</h3>
            <p>Increase your liquidity share with the DPLP*.</p>
            <v-icon>fa fa-arrow-right</v-icon>
          </div>
        </router-link>
        <router-link to="/" class="card">
          <div>
            <img src="/middle_image/temple.svg" />
            <h3>Temple</h3>
            <p>Praise to convert non-believers into hodlers.</p>
            <v-icon>fa fa-arrow-right</v-icon>
          </div>
        </router-link>
        <router-link to="/" class="card">
          <div>
            <img src="/middle_image/topsecret.svg" />
            <h3>Top Secret</h3>
            <p>Can you guess what the toads are working on?</p>
            <v-icon>fa fa-arrow-right</v-icon>
          </div>
        </router-link>
      </v-col>
    </v-row>
    <div class="video">
      <h2 style="margin-bottom: 30px; font-size: 28px; font-weight: 500">Decentralized Perpetual Liquidity</h2>
      <p style="color: #979CA5; margin-top: 0;">Our Decentralized Perpetual Liquidity Protocol* is designed to redistribute the liquidity tokens originally added by the team back to most valuable liquidity providers.</p>
      <img src="/video.png" />
    </div>
    <div class="video">
      <img src="/padandtoad.png" style="width: 300px; margin: 0 auto;" />
      <h2 style="margin-bottom: 30px; font-size: 28px; font-weight: 500">PadSwap and Toad Network:</h2>
      <p style="color: #979CA5; margin-top: 0;">There are two core components of the Toad Network: Toad and its perpetual liquidity, and PadSwap’s mechanism of creating a coin with intrinsic value. </br></br> With its scarce token supply and difficulty to farm, Toad operates as a store of value with a yield return as well. In the near future Toad will also become the governance token of PAD where Toad holders will be able to vote on decisions to change the tokens in the farm as well as the allocation percentages - making users of the Toad/Pad ecosystem incentivised to find quality and innovative projects and bring them into the Pad ecosystem.</p>
    </div>
    <v-row
      justify="center"
      class="middle"
    >
      <v-col
        cols="12"
        md="6"
        class="left"
      >
        <img src="/icons/padswap.png" style="width: 247px;" />
        <p style="font-size: 16px; line-height: 20px; letter-spacing: 0.25px; color: #979CA5; margin-bottom: 30px;">PadSwap is our own automated market maker. It was built with a vault system, that creates backing from farming and swapping fees for it and its native token, PAD. Earn yield in PAD by providing liquidity and helping PadSwap grow.</p>
        <div class="link-list">
          <v-btn to="/pad" style="color: #FB53EF"><img src="/favicon.png" /> Buy PAD on PadSwap</v-btn>
          <v-btn to="/" style="color: #FB53EF"><img src="/icons/guard1.svg" /> Audit Report</v-btn>
          <v-btn to="/" style="color: #FB53EF">FAQ</v-btn>
          <v-btn to="/" style="color: #FB53EF">Whitepaper</v-btn>
        </div>
        <div class="token-info" style="justify-content: start; margin-bottom: 0;">
          <div class="token-info-item text-center">
            <h1 style="margin-left: 0;">1.05M</h1>
            <p>Total Value Locked</p>
          </div>
          <div class="token-info-item text-center">
            <h1>46K</h1>
            <p>24h Volume</p>
          </div>
          <div class="token-info-item text-center">
            <h1>530K</h1>
            <p>Pad Market Cap</p>
          </div>
          <div class="token-info-item text-center">
            <h1>23.2K</h1>
            <p>Backing Reserves</p>
          </div>
        </div>
      </v-col>
      <v-col
        cols="12"
        md="6"
        class="right"
      >
        <router-link to="/" class="card">
          <div>
            <img src="/middle_image/toadfarms.svg" />
            <h3 style="color: #FB53EF">Swap</h3>
            <p>Earn yield in Toad by providing liquidity.</p>
            <v-icon>fa fa-arrow-right</v-icon>
          </div>
        </router-link>
        <router-link to="/" class="card">
          <div>
            <img src="/middle_image/toadlpfarms.svg" />
            <h3 style="color: #FB53EF">Pad Farms</h3>
            <p>Increase your liquidity share with the DPLP*.</p>
            <v-icon>fa fa-arrow-right</v-icon>
          </div>
        </router-link>
        <router-link to="/" class="card">
          <div>
            <img src="/middle_image/temple.svg" />
            <h3 style="color: #FB53EF">Launch Pad</h3>
            <p>Praise to convert non-believers into hodlers.</p>
            <v-icon>fa fa-arrow-right</v-icon>
          </div>
        </router-link>
        <router-link to="/" class="card">
          <div>
            <img src="/middle_image/topsecret.svg" />
            <h3 style="color: #FB53EF">Vault</h3>
            <p>Can you guess what the toads are working on?</p>
            <v-icon>fa fa-arrow-right</v-icon>
          </div>
        </router-link>
      </v-col>
    </v-row>
    <div class="video faq">
      <h2 style="margin-bottom: 30px; font-size: 28px; font-weight: 500">Frequently Asked Questions</h2>
      <div class="link-list">
        <v-btn-toggle>
          <v-btn>Binance Smart Chain</v-btn>
          <v-btn>Toad Network</v-btn>
          <v-btn>PadSwap</v-btn>
        </v-btn-toggle>
      </div>
      <v-expansion-panels>
        <v-expansion-panel
          v-for="(item,i) in 5"
          :key="i"
        >
          <v-expansion-panel-header>
            What is the Toad Network?
          </v-expansion-panel-header>
          <v-expansion-panel-content>
            The Toad Network is much more than a simple farming project, we are creating a new era of yield farming. At Toad you're not only going to be able to farm Toad, but also Toad-Bnb LP tokens. By doing that we are not only decentralizing the liquidity but also making the project rug pull proof. As all the liquidity from the team is going straight to a farming pool and from there to the hands of holders.
          </v-expansion-panel-content>
        </v-expansion-panel>
      </v-expansion-panels>
    </div>
    <div class="video">
      <h2 style="margin-bottom: 30px; font-size: 28px; font-weight: 500">An Evolving Community within an Evergrowing Defi Ecosystem</h2>
      <p style="color: #979CA5; margin-top: 0;">The Toad Network team is supported by a community of math nerds and meme lords from around the planet, across all timezones. Join us in building and growing together.</p>
      <img src="/toadgroup.png" style="max-width: 600px; width: 100%; margin: 0 auto;" />
    </div>
  </div>
</template>
<script>
import axios from 'axios'
import { padAddress, BEP20ABI, regularFarms } from '../farms_config.json'
export default {
  data () {
    return {
      farm_name: '',
      isV1: false,
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
      select: 'BNB-BUSD',
      items: []
    }
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
  .main {
    width: 80%;
    max-width: 1400px; 
    margin: 0 auto;
  }
  .pagelogo {
    display: flex;
    flex-direction: column;
  }
  .pagelogo img {
    width: 180px;
    margin: 0 auto;
  }
  .pagelogo h1 {
    font-size: 64px;
    line-height: 84px;
    font-weight: 400;
    letter-spacing: 3px;
    color: #00D741;
    margin-top: 10px;
  }
  .pagelogo h5{
    font-size: 18px;
    line-height: 24px;
    letter-spacing: 3px;
    font-weight: normal;
    margin-top: 10px;
  }
  .pagelogo .logo-link {
    margin: 0 auto;
    margin-top: 40px;
    margin-bottom: 20px;
    font-weight: 500;
    font-size: 16px;
    line-height: 20px;
    background-color: transparent;
    border: 2px solid;
    /*border-image: linear-gradient(152.93deg, #FB40EE 0%, #E098FA 37.38%, #51E071 64.53%, #00FC4C 83.46%);*/
    padding: 15px 120px;
    border-radius: 100px;
    height: auto;
  }
  @media screen and (max-width: 950px) {
    .pagelogo .logo-link {
      padding: 15px 60px;
    }
  }
  .link-list {
    display: flex;
    justify-content: center;
    margin-bottom: 64px;
  }
  .link-list img {
    margin-right: 5px; height: 21px; width: 21px;
  }
  .link-list .v-btn {
    background-color: transparent;
    font-size: 16px;
    line-height: 20px;
    letter-spacing: 0.75px;
    font-weight: 400;
    padding-top: 0px;
    padding-bottom: 0px;
    height: auto;
    border-radius: 0px;
    color: #00FC4C;
  }
  .link-list .v-btn--active::before, .v-btn:hover::before  {
    opacity: 0;
  }
  .link-list .v-btn:not(:last-child) {
    border-right: 1px solid #292D38;
  }
  .token-info {
    width: 100%;
    display: flex;
    justify-content: center;
    margin-bottom: 150px;
  }
  .token-info-item {
    display: flex;
    flex-direction: column;
  }
  .token-info-item h1{
    font-weight: 500;
    font-size: 48px;
    line-height: 52px;
    letter-spacing: 0.25px;
    margin: 0 25px;
    color: white;
  }
  .token-info-item p{
    font-weight: normal;
    font-size: 12px;
    line-height: 16px;
    letter-spacing: 0.25px;
    margin: 0;
    color: #71767F;
  }
  .middle {
    min-height: 484px;
    display: flex;
    padding: 0 30px;
    margin-bottom: 150px;
  }
  .middle .left {
    height: 100%;
    text-align: left;
    display: flex;
    flex-direction: column;
    justify-content: center;
    margin: auto 0;
  }
  .middle .left .link-list {
    justify-content: start;
  }
  .middle .left .link-list .v-btn:first-child {
    padding-left: 0;
  }
  .middle .right {
    height: 100%;
    display: grid;
    grid-template-columns: repeat(2, minmax(0,1fr));
  }

  .middle .right .card {
    margin: 12px;
    height: 230px;
    background-color: #181D26;
    display: flex;
    justify-content: center;
    text-decoration: none;
    border-radius: 15px;
    text-align: center;
  }
  .middle .right .card div {
    display: flex;
    flex-direction: column;
    justify-content: center;
    position: relative;
    width: 90%;
  }
  .middle .right .card div h3 {
    color: #00FC4C;
    font-weight: 500;
    font-size: 16px;
    line-height: 20px;
    margin-top: 5px;
  }
  .middle .right .card div p {
    color: #979CA5;
    font-size: 14px;
    font-weight: normal;
    margin-top: 5px;
  }
  .middle .right .card div i {
    justify-content: end;
    font-size: 14px;
    position: absolute;
    bottom: 15px;
    right: 0px;
  }
  .video {
    display: flex;
    flex-direction: column;
    justify-content: center;
    text-align: center;
    margin: 0 auto;
    margin-bottom: 150px;
    max-width: 800px;
  }
  .faq .link-list .v-btn-toggle {
    background-color: transparent;
    border: 0;
  }
  .faq .link-list .v-btn-toggle > .v-btn {
    color: #979CA5;
    height: auto;
    border-style: none;
  }
  .faq .link-list .v-btn-toggle > .v-item--active {
    color: #00FC4C;
  }
  .faq .link-list .v-btn:not(:last-child) {
    border-right: 1px solid #292D38;
  }
  .faq .v-expansion-panel {
    background-color: transparent !important;
    margin-bottom: 0px;
    border-bottom: 1px solid #292D38;
    border-radius: 0px !important;
  }
  .faq .v-expansion-panel--active {
    border: 1px solid #292D38;
    border-radius: 15px !important;
  }
  .faq .v-expansion-panel-header{
    color: #00FC4C;
    font-size: 16px;
    font-weight: 500;
    padding: 22px 30px;
  }
  .faq .v-expansion-panel-content {
    color: #979CA5;
    font-weight: 400;
    font-size: 16px;
    text-align: left;
    background-color: transparent !important;
  }
  @media screen and (max-width: 786px) {
    .token-info {
      flex-direction: column;
    }
    .token-info-item {
      margin-bottom: 20px;
    }
    .middle .left .link-list {
      flex-direction: column;
    }
    .middle .left .link-list .v-btn {
      border-right: 0;
      margin-bottom: 10px;
      justify-content: start;
      padding: 0;
    }
    .video .link-list .v-item-group {
      flex-direction: column;
    }
    .video .link-list .v-item-group .v-btn {
      border-right: 0;
      margin-bottom: 10px;
      padding: 0;
    }
  }
  @media screen and (max-width: 650px) {
    .pagelogo h1 {
      font-size: 46px;
    }
    .middle .right {
      grid-template-columns: repeat(1, minmax(0,1fr));
    }
  }
</style>
