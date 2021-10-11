<template>
  <v-app>
    <v-app-bar
      style="background:transparent; box-shadow: none"
      :clipped-left="clipped"
      absolute
      app
    >
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" class="hidden-md-and-up"></v-app-bar-nav-icon>
      <a href="/">
      <img
      class="mr-3 padlogo"
      src="/Toadlogo.svg"
      height="40"/>
      </a>
      <v-spacer></v-spacer>
      <div class="hidden-sm-and-down dappsanav">
      <v-menu
      v-for="(item, index) in navbar"
      :key="index"
      rounded="lg"
      offset-y
      open-on-hover
      >
        <template v-slot:activator="{ on, attrs }" v-if="item.subItems">
          <v-btn
            v-ripple="false"
            style="background:transparent !important; box-shadow:none"
            v-bind="attrs"
            v-on="on"
          >
            {{ item.title }}
            <v-icon style="margin-left:5px;font-size: 20px;">fas fa-angle-down</v-icon>
          </v-btn>
        </template>

        <template v-slot:activator="{ on, attrs }" v-else>
          <v-btn
            v-ripple="false"
            style="background:transparent !important; box-shadow:none"
            v-bind="attrs"
            v-on="on"
            :href="item.href"
            target="item.target"
          >
            {{ item.title }}
          </v-btn>
        </template>

        <v-list style="padding: 20px; background-color: black;" v-if="item.subItems">
          <v-list-item
            v-for="(subitem, subindex) in item.subItems"
            :key="subindex"
            :to="subitem.to"
            :href="subitem.href"
            :target="subitem.target"
          >
            <img :src="iconurl(subitem.icon)" style="margin-right: 10px" />
            <v-list-item-title>
              {{ subitem.title }}
              <div class="description">{{ subitem.description }}</div>
            </v-list-item-title>
          </v-list-item>
        </v-list>
      </v-menu>
      </div>
      <!-- <v-icon @click="switchTheme()">{{ "mdi-theme-light-dark" }}</v-icon> -->
      <v-btn
      id="connect-btn"
      style="margin-right:10px; margin-left: 10px; border-radius:12px; background: transparent; border-radius: 20px; border: 1px solid #00FC4C;"
      v-if="connected === 'Connect'"
      >
        Connect
      </v-btn>
      <v-btn
      id="connect-btn"
      @click="connect()"
      style="margin-right:10px; margin-left: 10px; border-radius:12px; border: 1px solid #00D741; border-radius: 20px; background: black;"
      v-else
      >
        {{connected}}
      </v-btn>
    </v-app-bar>
    <v-navigation-drawer
      class="hidden-md-and-up"
      v-model="drawer"
      fixed
      floating
    >
      <v-list-item-action @click.stop="drawer = !drawer" >
        <v-icon>{{ "mdi-close" }}</v-icon>
      </v-list-item-action>
      <v-list>
        <v-list-group
        v-for="(item, index) in navbar"
        :key="index"
        v-model="item.active"
        :prepend-icon="item.action"
        no-action
        >
        <template v-slot:activator>
          <v-list-item-content>
            <v-list-item-title v-text="item.title" active></v-list-item-title>
          </v-list-item-content>
        </template>

        <v-list-item
          v-for="(subitem, subindex) in item.subItems"
          :key="subindex"
          :to="subitem.to"
          :href="subitem.href"
          :target="subitem.target"
        >
          <v-list-item-content>
            <v-list-item-title v-text="subitem.title"></v-list-item-title>
            <div class="description">{{ subitem.description }}</div>
          </v-list-item-content>
        </v-list-item>
      </v-list-group>
    </v-list>
    </v-navigation-drawer>
    <v-main>
      <nuxt />
    </v-main>
    <!-- <v-footer
      :absolute="!fixed"
      app
    >
      <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer> -->
  </v-app>
</template>

<script>
import WalletConnectProvider from '@walletconnect/web3-provider'
import Web3 from 'web3'
import Web3Modal from 'web3modal'
import '@fortawesome/fontawesome-free/css/all.css'

const web3Modal = new Web3Modal({
  network: 'binance', // replace mainnet to binance
  cacheProvider: false,
  providerOptions: {
    walletconnect: {
      package: WalletConnectProvider,
      options: {
        rpc: {
          56: 'https://bsc-dataseed1.defibit.io/',
          97: 'https://data-seed-prebsc-2-s1.binance.org:8545/'
        },
        bridge: 'https://pancakeswap.bridge.walletconnect.org/',
        network: 'binance'
      }
    }
  }
})
export default {
  mounted () {
    this.dataseed = new Web3('https://bsc-dataseed1.defibit.io/')
  },
  data () {
    return {
      web3: null,
      connected: 'Connect',
      clipped: false,
      drawer: false,
      fixed: false,
      navbar: [
        {
          title: 'Toad',
          subItems: [
            {
              title: 'New',
              icon: 'New',
              description: 'Latest news and announcements',
              to: '/'
            },
            {
              title: 'Toad Farms',
              icon: 'Toad',
              description: 'Earn tokens by providing liquidity',
              to: '/toad'
            },
            {
              title: 'Temple',
              icon: 'Temple',
              description: 'Convert non-believers into hodlers',
              to: '/'
            }
          ]
        },
        {
          title: 'Padswap',
          subItems: [
            {
              title: 'Swap',
              icon: 'Swap',
              description: 'Buy or sell tokens on PadSwap',
              href: 'https://padswap.exchange/#/swap',
              target: '_blank'
            },
            {
              title: 'Pad Farms',
              icon: 'Pad',
              description: 'Earn tokens by providing liquidity',
              to: '/pad'
            },
            {
              title: 'Add Liquidity',
              icon: 'Pad',
              description: 'Create LP tokens to add to farms',
              to: '/pad'
            },
            {
              title: 'Vault',
              icon: 'Vault',
              description: 'PAD`s backing reserves',
              to: '/'
            },
            {
              title: 'Stats',
              icon: 'Stats',
              description: 'Analyze token prices and volume',
              href: 'https://info.padswap.exchange/home',
              target: '_blank'
            }
          ]
        },
        {
          title: 'Shop',
          subItems: [
            {
              title: 'Piramyd Store',
              icon: 'Piramyd',
              description: 'Official Toad Merch',
              href: 'https://piramyd.me/toad-network-x-piramyd/',
              target: '_blank'
            },
            {
              title: 'Amazon Store',
              icon: 'Amazon',
              description: 'Official Toad Merch',
              href: 'https://piramyd.me/toad-network-x-piramyd/',
              target: '_blank'
            }
          ]
        },
        {
          title: 'Games',
          href: 'https://toad.academy/games',
          target: '_blank'
        },
        {
          title: 'About',
          subItems: [
            {
              title: 'TOAD Wiki',
              icon: 'Wiki',
              description: 'Learn more about TOAD and PAD',
              href: 'http://toadwiki.herokuapp.com/',
              target: '_blank'
            },
            {
              title: 'Toad Academy',
              icon: 'Academy',
              description: 'Learn more about crypto',
              href: 'https://toad.academy/learn',
              target: '_blank'
            },
            {
              title: 'Toad Network Audit Report',
              icon: 'NetworkReport',
              description: 'Contracts audit report by SpadeAudits',
              to: '/'
            },
            {
              title: 'PadSwap Audit Report',
              icon: 'NetworkReport',
              description: 'Contracts audit report by SpadeAudits',
              href: '/pad_audit_report.pdf',
              target: '_blank'
            },
            {
              title: 'Toad Network White Paper',
              icon: 'WhitePaper',
              description: 'Toad Network`s White Paper',
              to: '/'
            },
            {
              title: 'PadSwap White Paper',
              icon: 'WhitePaper',
              description: 'PadSwap`s White Paper',
              href: 'https://www.dropbox.com/s/bng5e1bq2u03bk6/PAD%20WHITEPAPER.PDF',
              target: '_blank'
            }
          ]
        }
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'Toad Network'
    }
  },

  methods: {
    iconurl (icon) {
      return '/menuicons/' + icon + '.svg'
    },
    async connect () {
      const provider = await web3Modal.connect()
      const web3 = new Web3(provider)
      const accounts = await web3.eth.getAccounts()
      if (!provider.selectedAddress) { provider.selectedAddress = accounts[0] }
      this.connected = provider.selectedAddress.substring(0, 3) + '...' + provider.selectedAddress.substring(provider.selectedAddress.length - 3, provider.selectedAddress.length)
      this.myAddress = provider.selectedAddress
      this.web3 = web3
    },
    switchTheme () {
      this.$vuetify.theme.dark = !this.$vuetify.theme.dark
      localStorage.setItem('theme', this.$vuetify.theme.dark ? 'dark' : 'light')
    }
  }
}
</script>
<style scoped>
.v-toolbar__content, .v-toolbar__extension {
  padding: 10px 120px !important;
}
.v-sheet.theme--light > .v-toolbar__content > .dappsanav > .v-btn{
  color: #4c4c4c !important;
}
.v-sheet.theme--light > .v-toolbar__content > #connect-btn{
  color: white !important;
}
.v-toolbar__content.v-btn.v-btn--has-bg {
  background: black !important;
}
.v-toolbar__content .v-btn:hover {
  color: #00D741;
}

.theme--dark.v-navigation-drawer{
  background-color:rgb(33, 36, 41);
}
.v-navigation-drawer {
  z-index: 10;
  width: 96vw !important;
  margin-left:2vw;
  height: 97vh !important;
  top: 1.5vh !important;
  border-radius: 10px !important;
}
.v-list-item__action {
  margin-bottom: 0px;
  margin-left: calc(93vw - 25px);
  margin-top: 30px;
  cursor: pointer;
}
.v-application--is-ltr .v-list-group--no-action > .v-list-group__items > .v-list-item {
    padding-left: 32px;
}

.v-menu__content .v-list-item{
  padding: 10px;
}

.v-menu__content .v-list-item:hover {
  color: #00D741 !important;
  background-color: #212429;
  border-radius: 10px;
}
.v-list-item__action {
  margin-bottom: 0px;
  margin-left: calc(93vw - 25px);
  margin-top: 30px;
  cursor: pointer;
}
.v-application--is-ltr .v-list-group--no-action > .v-list-group__items > .v-list-item {
    padding-left: 32px;
}
.theme--dark.v-application {
  background-image: linear-gradient(
    to bottom,
    rgba(0,0,0, 0),
    rgba(0,0,0, 0),
    rgba(0,0,0, 100)
  ), url('/bg-home.png') !important;
  background-size: 100% 810px !important;
  background-repeat: no-repeat !important;
  background-color: rgb(0 0 0) !important;
  color: #FFFFFF;
}
.description {
  margin-top: 2px;
  font-size: 0.825rem;
  width: -moz-fit-content;
  color: rgb(136, 141, 155);
}
header { position: absolute; }
@media all and (max-width: 959px) {
  .padlogo {height:30px; margin-left:-5px; margin-right:-5px; margin-top:5px}
  body{overflow-x: hidden;}
}

</style>
