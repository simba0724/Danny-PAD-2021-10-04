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
        <v-card
          class="farm-card flip-card-front"
          elevation="8">
          <div class="text-center">
            <img class="launchpad-img" :src="farm_img">
            <div class="text-h4 launchpad" style="padding-top:40px; margin-bottom:20px">
            LaunchPad<font style="font-size:15px; vertical-align: super;"> alpha</font>
            </div>
            <br>
            <img height="80px" :src="presale_img">
            <div class="text-h5">FUCK YOU KARENS</div>
            <div class="farm-description" style="margin-bottom: 40px; margin-top:10px">FUK its the world's first spitecoin, their team is dedicated <br> to the re-education of Karens all across the world.</div>
            <h5 class="stats-line">
              <img :src="farm_img">
              Project name:
              <span class="stats-line-info">FUCK YOU KARENS</span>
            </h5>
            <v-divider style="margin-left: 2%; width: 96%;"></v-divider>
            <h5 class="stats-line">
              <img :src="farm_img">
              Ticker:
              <span class="stats-line-info">$FUK</span>
            </h5>
            <v-divider style="margin-left: 2%; width: 96%;"></v-divider>
            <h5 class="stats-line">
              <img :src="farm_img">
              HardCap:
              <span class="stats-line-info">400 BNB</span>
            </h5>
            <v-divider style="margin-left: 2%; width: 96%;"></v-divider>
            <h5 class="stats-line">
              <img :src="farm_img">
              Presale Price:
              <span class="stats-line-info">${{price.toFixed(8)}}</span>
            </h5>
            <v-divider style="margin-left: 2%; width: 96%;"></v-divider>
            <h5 class="stats-line">
              <img :src="farm_img">
              BNB Raised:
              <span class="stats-line-info">{{round(raisedAmount, 4)}} BNB</span>
            </h5>
            <v-divider style="margin-left: 2%; width: 96%;"></v-divider>
            <h5 class="stats-line">
              <img :src="farm_img">
              Remaining Tokens:
              <span class="stats-line-info">{{remainingTokens}}</span>
            </h5>
            <v-divider style="margin-left: 2%; width: 96%;"></v-divider>
            <v-divider style="margin-left: 2%; width: 96%;"></v-divider>
            <h5 class="stats-line">
              <img :src="farm_img">
              Bought Tokens:
              <span class="stats-line-info">{{myTokenss}}</span>
            </h5>
            <v-divider style="margin-left: 2%; width: 96%;"></v-divider>
          </div>
          <v-row
          justify="center"
          style="margin-top:10px; margin-bottom:10px">
            <v-col
            v-if="active"
            class="tt1"
            cols="12"
            md="5">
              <v-text-field
                style="min-height:36px"
                class="vault-input"
                label="BNB amount"
                solo
                v-model="amount"
              ></v-text-field>
            </v-col>
            <v-col
            v-if="active"
            class="tt2"
            cols="12"
            md="5">
              <v-text-field
                style="min-height:36px"
                class="vault-input"
                label="You will receive"
                disabled
                solo
                v-model="expected"
              ></v-text-field>
            </v-col>
            <v-col
            v-if="active"
            class="tt3"
            cols="12"
            md="2">
              <div class="text-center">
                <v-btn
                @click="buy()"
                class="add-remove-btns cool-btn"
                color="primary"
                elevation="0">
                  BUY
                </v-btn>
              </div>
            </v-col>
            <v-col
            v-else
            cols="12"
            md="12">
              <div class="text-center">
                <v-btn
                @click="redeem()"
                class="add-remove-btns cool-btn"
                color="primary"
                elevation="0">
                  Redeem Your Tokens
                </v-btn>
              </div>
            </v-col>
          </v-row>
           <v-progress-linear
              v-model="percentage"
              color="#876bec"
              height="25"
            >{{percentage}}%</v-progress-linear>
        </v-card>
    </v-col>
  </v-row>

</template>

<script>
/*eslint-disable */
import { BEP20ABI } from '../farms_config.json'
export default {
  data () {
    return {
      intervals: {},
      myAddress: '',
      tokenABI: [],
      contractABI: [
	{
		"constant": false,
		"inputs": [],
		"name": "buy",
		"outputs": [],
		"payable": true,
		"stateMutability": "payable",
		"type": "function"
	},
	{
		"anonymous": false,
		"inputs": [
			{
				"indexed": false,
				"name": "amount",
				"type": "uint256"
			},
			{
				"indexed": false,
				"name": "tokens",
				"type": "uint256"
			}
		],
		"name": "Buy",
		"type": "event"
	},
	{
		"constant": false,
		"inputs": [],
		"name": "claimTokens",
		"outputs": [],
		"payable": false,
		"stateMutability": "nonpayable",
		"type": "function"
	},
	{
		"constant": false,
		"inputs": [],
		"name": "endPresale",
		"outputs": [],
		"payable": false,
		"stateMutability": "nonpayable",
		"type": "function"
	},
	{
		"constant": false,
		"inputs": [
			{
				"name": "_newValue",
				"type": "uint256"
			}
		],
		"name": "setBnbPrice",
		"outputs": [],
		"payable": false,
		"stateMutability": "nonpayable",
		"type": "function"
	},
	{
		"inputs": [],
		"payable": false,
		"stateMutability": "nonpayable",
		"type": "constructor"
	},
	{
		"constant": false,
		"inputs": [
			{
				"name": "amount",
				"type": "uint256"
			}
		],
		"name": "withdrawFunds",
		"outputs": [],
		"payable": false,
		"stateMutability": "nonpayable",
		"type": "function"
	},
	{
		"constant": false,
		"inputs": [
			{
				"name": "amount",
				"type": "uint256"
			}
		],
		"name": "withdrawTokens",
		"outputs": [],
		"payable": false,
		"stateMutability": "nonpayable",
		"type": "function"
	},
	{
		"constant": true,
		"inputs": [],
		"name": "bnbPrice",
		"outputs": [
			{
				"name": "",
				"type": "uint256"
			}
		],
		"payable": false,
		"stateMutability": "view",
		"type": "function"
	},
	{
		"constant": true,
		"inputs": [
			{
				"name": "",
				"type": "address"
			}
		],
		"name": "boughtTokens",
		"outputs": [
			{
				"name": "",
				"type": "uint256"
			}
		],
		"payable": false,
		"stateMutability": "view",
		"type": "function"
	},
	{
		"constant": true,
		"inputs": [],
		"name": "buyLimit",
		"outputs": [
			{
				"name": "",
				"type": "uint256"
			}
		],
		"payable": false,
		"stateMutability": "view",
		"type": "function"
	},
	{
		"constant": true,
		"inputs": [
			{
				"name": "amount",
				"type": "uint256"
			}
		],
		"name": "calculateBuy",
		"outputs": [
			{
				"name": "",
				"type": "uint256"
			}
		],
		"payable": false,
		"stateMutability": "view",
		"type": "function"
	},
	{
		"constant": true,
		"inputs": [],
		"name": "ceoAddress",
		"outputs": [
			{
				"name": "",
				"type": "address"
			}
		],
		"payable": false,
		"stateMutability": "view",
		"type": "function"
	},
	{
		"constant": true,
		"inputs": [],
		"name": "getTokenBalance",
		"outputs": [
			{
				"name": "",
				"type": "uint256"
			}
		],
		"payable": false,
		"stateMutability": "view",
		"type": "function"
	},
	{
		"constant": true,
		"inputs": [],
		"name": "isClosed",
		"outputs": [
			{
				"name": "",
				"type": "bool"
			}
		],
		"payable": false,
		"stateMutability": "view",
		"type": "function"
	},
	{
		"constant": true,
		"inputs": [],
		"name": "multiplier",
		"outputs": [
			{
				"name": "",
				"type": "uint256"
			}
		],
		"payable": false,
		"stateMutability": "view",
		"type": "function"
	},
	{
		"constant": true,
		"inputs": [],
		"name": "myTokens",
		"outputs": [
			{
				"name": "",
				"type": "uint256"
			}
		],
		"payable": false,
		"stateMutability": "view",
		"type": "function"
	},
	{
		"constant": true,
		"inputs": [],
		"name": "presaleSize",
		"outputs": [
			{
				"name": "",
				"type": "uint256"
			}
		],
		"payable": false,
		"stateMutability": "view",
		"type": "function"
	},
	{
		"constant": true,
		"inputs": [],
		"name": "token",
		"outputs": [
			{
				"name": "",
				"type": "address"
			}
		],
		"payable": false,
		"stateMutability": "view",
		"type": "function"
	},
	{
		"constant": true,
		"inputs": [],
		"name": "tokenPrice",
		"outputs": [
			{
				"name": "",
				"type": "uint256"
			}
		],
		"payable": false,
		"stateMutability": "view",
		"type": "function"
	},
	{
		"constant": true,
		"inputs": [],
		"name": "totalBoughtTokens",
		"outputs": [
			{
				"name": "",
				"type": "uint256"
			}
		],
		"payable": false,
		"stateMutability": "view",
		"type": "function"
	}
],
      contractAddress: '0x10310Cf63A330f995d67b55d007CE0df3eaCe658',
      tokenAddress: '0xA898bbb508C04BE26af3d319b7775927AFCB02AF',
      tokenInstance: null,
      contractInstance: null,
      farm_img: '/padSwapLogo.svg',
      presale_img: '/fuk.png',
      active: true,
      amount: null,
      expected: null,
      price: 0.00000085,
      raisedAmount: '0.000',
      bnbPrice: 350,
      percentage: 0,
      allTokens: 150000000000,
      remainingTokens: '0.000',
      myTokenss: '0.000',
    }
  },
  components: {
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
        this.tokenABI = BEP20ABI
        const web3 = this.$parent.$parent.$parent.$parent.web3
        this.dataseed = this.$parent.$parent.$parent.$parent.dataseed
        this.tokenInstance = new this.dataseed.eth.Contract(this.tokenABI, this.tokenAddress)
        this.contractInstance = new this.dataseed.eth.Contract(this.contractABI, this.contractAddress)
        if (web3) {
          const web3 = this.$parent.$parent.$parent.$parent.web3
          this.dataseed = this.$parent.$parent.$parent.$parent.dataseed
          this.tokenInstance = new this.dataseed.eth.Contract(this.tokenABI, this.tokenAddress)
          this.contractInstance = new this.dataseed.eth.Contract(this.contractABI, this.contractAddress)
          this.web3 = web3
          console.log('clearing')
          clearInterval(loadWeb3)
        }
      }, 500)
      this.intervals['loadData'] = setInterval(async () => {
        this.myAddress = this.$parent.$parent.$parent.$parent.myAddress
        this.getRaiseAmount()
        this.getRemainingTokens()
        this.getBnbPrice()
        this.getPercentage()
        this.checkActive()
        this.myTokens()
      }, 3000)
    },
    async myTokens() {
      const res = await this.contractInstance.methods.boughtTokens(this.myAddress).call()
      this.myTokenss = this.round(res/1e18, 2)
    },
    async getRaiseAmount() {
      const balance = await this.dataseed.eth.getBalance(this.contractAddress)
      this.raisedAmount = balance/1e18
    },

    async getRemainingTokens() {
      const remaining = await this.contractInstance.methods.getTokenBalance().call()
      this.remainingTokens = remaining / 1e18
    },

    async getBnbPrice() {
      this.bnbPrice = await this.contractInstance.methods.bnbPrice().call()
    },

    async getPercentage() {
      const soldTokens = await this.allTokens - this.remainingTokens
      this.percentage = this.round(soldTokens/this.allTokens*100)
    },

    async checkActive() {
      const closed  = await this.contractInstance.methods.isClosed().call()
      this.active = !closed
    },

    async buy() {
      const contractInstance = new this.web3.eth.Contract(this.contractABI, this.contractAddress)
      let amount = this.web3.utils.toWei(this.amount.replace(',','.'), 'ether')
      const gasPrice = await this.web3.eth.getGasPrice()
      let args = {
        from: this.myAddress,
        gasPrice: gasPrice,
        value: amount,
      }
      await contractInstance.methods.buy().send(args)
    },
    async redeem() {
      const contractInstance = new this.web3.eth.Contract(this.contractABI, this.contractAddress)
      const gasPrice = await this.web3.eth.getGasPrice()
      let args = {
        from: this.myAddress,
        gasPrice: gasPrice,
        value: 0,
      }
      await contractInstance.methods.claimTokens().send(args)
    }
  },
  watch: {
    amount () {
      const l = this.amount
      this[l] = !this[l]
      this.amount = this.amount.replace(',', '.')
      this.expected = this.round(this.amount*this.bnbPrice/this.price, 2)
    },
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
.launchpad {
  line-height: 100px;
  vertical-align: top;
  display: inline;
}
.launchpad-img {
  height: 50px;
  margin-top: 30px;
  display: inline;
  margin-right: 5px;
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
</style>
