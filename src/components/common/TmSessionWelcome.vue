<template>
  <div id="session-welcome" class="session">
    <div class="session-header">
      <a @click="closeSession">
        <i class="material-icons session-close">close</i>
      </a>
    </div>

    <img class="lunie-logo" src="~assets/images/cosmos-wallet-logo.svg" />

    <h2 class="session-title">
      Welcome to Lunie
    </h2>

    <p class="session-paragraph">
      The easiest way to send, stake, and vote on the Cosmos Hub.
    </p>

    <div class="session-list">
      <LiSession
        id="creat-new-address"
        icon="person_add"
        title="Create a new address"
        @click.native="() => setState('sign-up')"
      />
      <LiSession
        id="use-an-existing-address"
        icon="person"
        title="Use an existing address"
        @click.native="() => setState('existing')"
      />
    </div>

    <p class="footnote">
      By using Lunie, you accept our
      <router-link to="/terms" class="link">Terms of Service</router-link> and
      <router-link to="/privacy" class="link">Privacy Policy</router-link>.
    </p>
  </div>
</template>

<script>
import LiSession from "common/TmLiSession"
export default {
  name: `session-welcome`,
  components: {
    LiSession
  },
  methods: {
    setState(value) {
      this.$emit(`route-change`, value)
    },
    closeSession() {
      this.$emit(`close`)
    }
  }
}
</script>
<style scoped>
.session-title {
  padding-left: 1rem;
}

.lunie-logo {
  height: 4rem;
  margin: 0 0 1rem 1rem;
  align-self: flex-start;
}

.session-paragraph {
  padding: 0 1rem;
}
</style>
