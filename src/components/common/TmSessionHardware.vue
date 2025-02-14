<template>
  <div class="session">
    <div class="session-header">
      <a @click="goBack">
        <i class="material-icons session-back">arrow_back</i>
      </a>
      <a @click="close">
        <i class="material-icons session-close">close</i>
      </a>
    </div>
    <h2 class="session-title">
      Use my Ledger Nano
    </h2>

    <template v-if="session.browserWithLedgerSupport">
      <div class="session-main">
        <HardwareState :loading="status === `connect` ? false : true">
          <template v-if="status === `connect` || status === `detect`">
            Please plug in your Ledger&nbsp;Nano&nbsp;S and open the Cosmos app
          </template>
          <template v-if="status === `confirmAddress`">
            Sign in with the address
            <span class="address">{{ address }}</span
            >.<br />
            Please confirm on your Ledger.
          </template>
          <p v-if="connectionError" class="error-message">
            {{ connectionError }}
          </p>
        </HardwareState>
      </div>
      <div class="session-footer">
        <p class="ledger-install">
          If you don't have a Ledger Nano, you can
          <a href="" target="_blank" rel="noopener norefferer">buy one here</a>.
        </p>
        <TmBtn
          :value="submitCaption"
          :disabled="status === `connect` ? false : `disabled`"
          @click.native="signIn()"
        />
      </div>
    </template>

    <div v-else class="session-main">
      <p>
        Please use Chrome, Opera, or Brave. Ledger is not supported in this
        browser.
      </p>
    </div>
  </div>
</template>

<script>
import TmBtn from "common/TmBtn"
import { mapGetters } from "vuex"
import HardwareState from "common/TmHardwareState"
export default {
  name: `session-hardware`,
  components: {
    TmBtn,
    HardwareState
  },
  data: () => ({
    status: `connect`,
    connectionError: null,
    address: null
  }),
  computed: {
    ...mapGetters([`session`]),
    submitCaption() {
      return {
        connect: "Sign In",
        detect: "Waiting for Ledger",
        confirmAddress: "Confirming Address"
      }[this.status]
    }
  },
  methods: {
    setState(value) {
      this.$emit(`route-change`, value)
    },
    goBack() {
      this.$emit(`route-change`, "existing")
    },
    close() {
      this.$emit(`close`)
    },
    async signIn() {
      this.connectionError = null
      this.status = `detect`
      this.address = null
      try {
        this.address = await this.$store.dispatch(`connectLedgerApp`)
      } catch ({ message }) {
        this.status = `connect`
        this.connectionError = message
        return
      }

      this.status = `confirmAddress`
      if (await this.confirmAddress()) {
        await this.$store.dispatch(`signIn`, {
          sessionType: `ledger`,
          address: this.address
        })
        return
      }

      this.status = `connect`
    },
    async confirmAddress() {
      try {
        await this.$store.dispatch("confirmLedgerAddress")
        return true
      } catch ({ message }) {
        this.connectionError = message
      }
      return false
    }
  }
}
</script>
<style scoped>
.error-message {
  color: var(--danger);
  font-size: var(--sm);
  font-style: italic;
  margin-bottom: 0;
  padding-top: 1rem;
}

.install-notes {
  flex-direction: column;
}

.ledger-install {
  font-size: var(--sm);
  margin-bottom: 0;
}

.session-footer {
  justify-content: space-between;
}

.address {
  color: var(--link);
  font-weight: 500;
  font-size: 14px;
  white-space: nowrap;
}
</style>
