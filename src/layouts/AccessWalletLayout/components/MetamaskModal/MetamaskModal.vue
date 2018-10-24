<template>
  <b-modal
    ref="metamask"
    :title="$t('accessWallet.accessByMetaMask')"
    hide-footer
    class="bootstrap-modal modal-metamask"
    centered>
    <div class="metamask-modal-content">
      <div class="modal-multi-icons">
        <img
          class="icon"
          src="~@/assets/images/icons/button-metamask-fox.svg">
        <img
          class="icon"
          src="~@/assets/images/icons/clip.svg">
        <img
          class="icon logo-small"
          src="~@/assets/images/logo-small.png">
      </div>
      <div class="d-block content-container text-center">
        <h4>
          {{ $t("accessWallet.metaMaskModalDesc") }}
        </h4>
      </div>
      <div class="accept-terms">
        <label class="checkbox-container">{{ $t("accessWallet.acceptTerms") }} <a href="/">{{ $t("common.terms") }}</a>.
          <input
            type="checkbox"
            @click="accessMyWalletBtnDisabled = !accessMyWalletBtnDisabled" >
          <span class="checkmark"/>
        </label>
      </div>
      <div class="button-container">
        <standard-button
          v-if="accessMyWalletBtnDisabled"
          :options="buttonDisabled"
          @click.native="interact = true"
        />
        <router-link to="interface">

          <standard-button
            v-if="!accessMyWalletBtnDisabled"
            :options="buttonAccessMyWallet"
            @click.native="interact = true"
          />

          <!--
          <b-btn
            :disabled="accessMyWalletBtnDisabled"
            class="mid-round-button-green-filled close-button">
            {{ $t("accessWallet.accessMyWallet") }}
          </b-btn>
        -->
        </router-link>

        <!--
        <b-btn
          :disabled="accessMyWalletBtnDisabled"
          class="mid-round-button-green-filled close-button">
          {{ $t("accessWallet.accessMyWallet") }}
        </b-btn>
      -->
      </div>
      <customer-support/>
    </div><!-- .metamask-modal-content -->
  </b-modal>
</template>

<script>
import CustomerSupport from '@/components/CustomerSupport';
import { Web3Wallet } from '@/wallets/software';
import Web3 from 'web3';

export default {
  components: {
    'customer-support': CustomerSupport
  },
  props: {
    networkAndAddressOpen: {
      type: Function,
      default: function() {}
    }
  },
  data() {
    return {
      buttonDisabled: {
        title: 'Access My Wallet',
        buttonStyle: 'grey',
        rightArrow: false,
        leftArrow: false,
        fullWidth: true
      },
      buttonAccessMyWallet: {
        title: 'Access My Wallet',
        buttonStyle: 'green',
        rightArrow: false,
        leftArrow: false,
        fullWidth: true
      },
      accessMyWalletBtnDisabled: true
    };
  },
  mounted() {
    this.web3WalletExists = this.checkWeb3();
  },
  methods: {
    reload() {
      window.location.reload();
    },
    getWeb3Wallet() {
      // NOTE: Uncomment code and debug when metamask's new version launches
      // if (window.web3 === undefined) {
      //   window.addEventListener('message', ({ data }) => {
      //     if (data && data.type && data.type === 'ETHEREUM_PROVIDER_SUCCESS') {
      //       window.web3 = new Web3(ethereum);
      //     }
      //   });
      //   window.postMessage(
      //     { type: 'ETHEREUM_PROVIDER_REQUEST', web3: true },
      //     '*'
      //   );
      // }

      if (this.checkWeb3() !== true) return;
      new Web3(window.web3.currentProvider).eth
        .getAccounts()
        .then(accounts => {
          if (!accounts.length) return (this.unlockWeb3Wallet = true);
          const address = accounts[0];
          const wallet = new Web3Wallet(address);
          this.$store.dispatch('setWeb3Wallet', wallet);
          this.$store.dispatch('setWeb3Instance', window.web3.currentProvider);
          this.$router.push({ path: 'interface' });
        })
        .catch(() => {
          return (this.web3WalletExists = false);
        });
    },
    checkWeb3() {
      if (window.web3 !== undefined) return true;
      return false;
    }
  }
};
</script>

<style lang="scss" scoped>
@import 'MetamaskModal.scss';
</style>
