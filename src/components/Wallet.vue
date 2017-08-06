<template>
 <div class="container">
  <div class="row">
    <div class="col-xs-12 col-sm-8 col-sm-push-2">
      <h1 class="text-center">CloverToken</h1>
      <hr/>
      <br/>
    </div>
  </div>

  <div id="petsRow" class="row">
    <div class="col-sm-6 col-sm-push-3 col-md-4 col-md-push-4">
      <div class="panel panel-default">
        <div class="panel-heading">
          <h3 class="panel-title">My Wallet</h3>
          <sub>{{account}}</sub>
        </div>
        <div class="panel-body">
          <h4>Balance</h4>
          <strong>Balance</strong>: <span id="TTBalance" v-html='balance'></span> TT<br/><br/>
          <h4>Transfer</h4>
          <input type="text" class="form-control" id="TTTransferAddress" v-model='toAddress' placeholder="Address" />
          <input type="text" class="form-control" id="TTTransferAmount" v-model='toAmount' placeholder="Amount" />
          <button class="btn btn-primary" id="transferButton" type="button" @click='transfer()'>Transfer</button>
        </div>
      </div>
    </div>
  </div>
</div>
</template>

<script>
import Web3 from 'web3'

export default {

  name: 'Wallet',
  data () {
    return {
      toAddress: null,
      toAmount: 0,
      web3: null,
      web3Provider: null,
      balance: 0,
      contracts: {},
      account: null
    }
  },
  mounted () {
    this.getProvider()
  },
  methods: {
    getProvider () {
      // Checking if Web3 has been injected by the browser (Mist/MetaMask)
      if (typeof web3 !== 'undefined') {
        // Use Mist/MetaMask's provider
        this.web3Provider = web3.currentProvider
        web3 = new Web3(this.web3Provider)
      } else {
        // fallback - use your fallback strategy (local node / hosted node + in-dapp id mgmt / fail)
        this.web3Provider = new Web3.providers.HttpProvider('http://localhost:8545')
        web3 = new Web3(this.web3Provider)
      }
      this.getContract()
    },
    getContract () {
      var TutorialTokenArtifact = require('../../build/contracts/TutorialToken.json')
      this.contracts.TutorialToken = this.$TruffleContract(TutorialTokenArtifact)
      this.contracts.TutorialToken.setProvider(this.web3Provider)
      this.networkCheck()
      this.getBalances()
    },
    networkCheck () {
      web3.version.getNetwork((err, netId) => {
        if (err) {
          console.log(err)
          return
        }
        switch (netId) {
          case '1':
            console.log('This is mainnet')
            break
          case '2':
            console.log('This is the deprecated Morden test network.')
            break
          case '3':
            console.log('This is the ropsten test network.')
            break
          default:
            console.log('This is an unknown network.')
        }
      })
    },
    getBalances (adopters, account) {
      console.log('getting balances...')
      web3.eth.getAccounts((error, accounts) => {
        if (error) {
          console.log(error)
          return
        }
        this.account = accounts[0]
        console.log(accounts)
        this.contracts.TutorialToken.deployed().then((instance) => {
          var tutorialTokenInstance = instance

          return tutorialTokenInstance.balanceOf(this.account)
        }).then((result) => {
          console.log(result)
          this.balance = result.c[0]
        }).catch((err) => {
          console.log(err.message)
        })
      })
    }
  }
}
require('bootstrap-css-only')
</script>

<style lang="css" scoped>

</style>
