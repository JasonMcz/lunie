<template>
  <SessionWelcome
    v-if="view == 'welcome'"
    @route-change="goTo"
    @close="close"
  />
  <SessionExisting
    v-else-if="view == 'existing'"
    @route-change="goTo"
    @close="close"
  />
  <SessionExplore
    v-else-if="view == 'explore'"
    @route-change="goTo"
    @close="close"
  />
  <SessionSignUp
    v-else-if="view == 'sign-up'"
    @route-change="goTo"
    @close="close"
  />
  <SessionSignIn
    v-else-if="view == 'sign-in'"
    @route-change="goTo"
    @close="close"
  />
  <SessionAccountDelete
    v-else-if="view == 'delete'"
    @route-change="goTo"
    @close="close"
  />
  <SessionHardware
    v-else-if="view == 'hardware'"
    @route-change="goTo"
    @close="close"
  />
  <SessionImport
    v-else-if="view == 'import'"
    @route-change="goTo"
    @close="close"
  />
</template>

<script>
import SessionWelcome from "common/TmSessionWelcome"
import SessionExisting from "common/TmSessionExisting"
import SessionExplore from "common/TmSessionExplore"
import SessionSignUp from "common/TmSessionSignUp"
import SessionSignIn from "common/TmSessionSignIn"
import SessionHardware from "common/TmSessionHardware"
import SessionImport from "common/TmSessionImport"
import SessionAccountDelete from "common/TmSessionAccountDelete"

export default {
  name: `session-router`,
  components: {
    SessionWelcome,
    SessionExisting,
    SessionExplore,
    SessionSignUp,
    SessionSignIn,
    SessionHardware,
    SessionImport,
    SessionAccountDelete
  },
  data: () => ({
    view: "welcome"
  }),
  methods: {
    goTo(view) {
      this.view = view
    },
    close() {
      this.$emit("close")
    }
  }
}
</script>
<style>
@import "../../styles/session.css";
</style>
