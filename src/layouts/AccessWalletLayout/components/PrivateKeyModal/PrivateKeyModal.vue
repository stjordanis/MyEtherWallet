<template>
  <b-modal
    ref="privateKey"
    :title="$t('accessWallet.accessByPrivateKey')"
    hide-footer
    class="bootstrap-modal modal-software"
    centered>
    <div class="the-content">
    <form class="private-key-form">
      <div class="input-container">
        <input
          v-model="privateKey"
          type="text"
          name="PrivateKey"
          autocomplete="off">
      </div>
      <standard-button
        :disabled="privateKey === '' && privateKey.length === 0 && privateKey.length < 9"
        @click="unlockWallet"
      >{{ $t("accessWallet.unlock") }}</standard-button>
    </form>
    </div>
  </b-modal>
</template>

<script>
import { WalletInterface } from '@/wallets';
import { PRIV_KEY as privKeyType } from '@/wallets/bip44/walletTypes';
export default {
  data() {
    return {
      privateKey: ''
    };
  },
  methods: {
    unlockWallet() {
      this.$store.dispatch(
        'decryptWallet',
        new WalletInterface(this.privateKey, false, privKeyType)
      );
      this.privateKey = '';
      this.$router.push({ path: 'interface' });
    }
  }
};
</script>
<style lang="scss" scoped>
@import 'PrivateKeyModal-desktop.scss';
@import 'PrivateKeyModal-tablet.scss';
@import 'PrivateKeyModal-mobile.scss';
</style>
