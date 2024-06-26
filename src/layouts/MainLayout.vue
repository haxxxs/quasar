<script setup>
import signin from 'components/SignIn.vue'
import { ref } from 'vue'
import { userStore } from 'stores/user'
import { useRouter } from 'vue-router'
import { db } from 'boot/db'
import { useQuasar } from 'quasar'
import ClearConfirmModal from './ClearConfirmModal.vue'

const router = useRouter()
const $q = useQuasar()
const dark = ref($q.dark.isActive)
const darkToggle = () => {
  $q.dark.toggle()
}

const leftDrawerOpen = ref(false)
const enter = ref(false)
const isClear = ref(false)

const user = userStore()

const toggleLeftDrawer = () => {
  leftDrawerOpen.value = !leftDrawerOpen.value
}

const exit = () => {
  user.pk = ''
  user.sk = ''
  user.seed = ''
  router.push('/')
}

const clear = () => {
  db.delete('settings', 'seed')
  user.$reset()
  router.push('/')
  isClear.value = false
}
</script>

<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <q-btn v-if="user.crypt" flat dense round icon="menu" aria-label="Menu" @click="toggleLeftDrawer" />

        <q-toolbar-title>{{ $t('title') }}

        </q-toolbar-title>

        <q-btn icon="login" v-if="user.crypt && user.sk.length === 0" @click="enter = true" color="secondary"></q-btn>
        <q-btn icon="logout" class="q-ml-sm" v-if="user.sk.length > 0" @click="exit" color="secondary"></q-btn>
        <!--TODO: mode in db-->
        <q-toggle v-if="user.crypt" v-model="dark" color="black" @click="darkToggle" />
        <!--TODO: locale in db-->
        <q-select class="q-ml-sm" v-model="$i18n.locale" :options=$i18n.availableLocales />
      </q-toolbar>
    </q-header>
    <q-footer v-if="!user.crypt">
      <q-toolbar class="flex flex-center q-pa-sm justify-evenly">
        <q-item class="git" lickable tag="a" href="https://github.com/yatledger">
          <q-item-section avatar><q-icon name="code" /></q-item-section>
          <q-item-section class="gitBtn">
            <q-item-label>{{ $t('menu.code') }}</q-item-label>
            <q-item-label>{{ $t('menu.codeDesc') }}</q-item-label>
          </q-item-section>
        </q-item>
        <q-item to="/help" clickable tag="span">
          <q-item-section avatar><q-icon name="help" /></q-item-section>
          <q-item-section class="helpBtn">
            <q-item-label>{{ $t('menu.help') }}</q-item-label>
            <q-item-label>{{ $t('menu.helpDesc') }}</q-item-label>
          </q-item-section>
        </q-item>
        <q-item to="/topUsers" clickable tag="span">
          <q-item-section avatar><q-icon name="emoji_events" /></q-item-section>
          <q-item-section class="topUsersBtn">
            <q-item-label>{{ $t('menu.topUsers') }}</q-item-label>
            <q-item-label>{{ $t('menu.topUsersDesc') }}</q-item-label>
          </q-item-section>
        </q-item>
        <q-item clickable tag="a" to="/">
          <q-item-section class="homeBtn" avatar><q-icon name="home" /></q-item-section>
          <q-item-section class="homeText">
            <q-item-label>{{ $t('menu.home') }}</q-item-label>
            <q-item-label>{{ $t('menu.homeDesc') }}</q-item-label>
          </q-item-section>
        </q-item>
      </q-toolbar>
    </q-footer>
    <q-drawer v-if="user.crypt" v-model="leftDrawerOpen" show-if-above bordered>
      <q-list>
        <q-item-label header>{{ $t('menu.title') }}:</q-item-label>
        <q-item clickable tag="a" to="/">
          <q-item-section avatar><q-icon name="home" /></q-item-section>
          <q-item-section>
            <q-item-label>{{ $t('menu.home') }}</q-item-label>
            <q-item-label caption>{{ $t('menu.homeDesc') }}</q-item-label>
          </q-item-section>
        </q-item>
        <q-separator />
        <q-item clickable tag="a" href="https://github.com/yatledger">
          <q-item-section avatar><q-icon name="code" /></q-item-section>
          <q-item-section>
            <q-item-label>{{ $t('menu.code') }}</q-item-label>
            <q-item-label caption>{{ $t('menu.codeDesc') }}</q-item-label>
          </q-item-section>
        </q-item>
        <q-separator />
        <!-- <q-item v-if="!user.crypt" clickable tag="a" to="/sign">
          <q-item-section avatar><q-icon name="stream" /></q-item-section>
          <q-item-section>
            <q-item-label>{{ $t('menu.sign') }}</q-item-label>
            <q-item-label caption>{{ $t('menu.signDesc') }}</q-item-label>
          </q-item-section>
        </q-item> -->
        <q-item v-if="user.crypt" @click="isClear = true" clickable tag="span">
          <q-item-section avatar><q-icon name="delete" /></q-item-section>
          <q-item-section>
            <q-item-label>{{ $t('menu.clear') }}</q-item-label>
            <q-item-label caption>{{ $t('menu.clearDesc') }}</q-item-label>
          </q-item-section>
        </q-item>
        <q-item to="/help" clickable tag="span">
          <q-item-section avatar><q-icon name="help" /></q-item-section>
          <q-item-section>
            <q-item-label>{{ $t('menu.help') }}</q-item-label>
            <q-item-label caption>{{ $t('menu.helpDesc') }}</q-item-label>
          </q-item-section>
        </q-item>
        <q-item to="/tx" clickable tag="span">
          <q-item-section avatar><q-icon name="wallet" /></q-item-section>
          <q-item-section>
            <q-item-label>{{ $t('menu.transaction') }}</q-item-label>
            <q-item-label caption>{{ $t('menu.transactionDesc') }}</q-item-label>
          </q-item-section>
        </q-item>
        <q-item to="/contacts" clickable tag="span">
          <q-item-section avatar><q-icon name="people_alt" /></q-item-section>
          <q-item-section>
            <q-item-label>{{ $t('menu.contacts') }}</q-item-label>
            <q-item-label caption>{{ $t('menu.contactsDesc') }}</q-item-label>
          </q-item-section>
        </q-item>
        <q-item to="/topUsers" clickable tag="span">
          <q-item-section avatar><q-icon name="emoji_events" /></q-item-section>
          <q-item-section>
            <q-item-label>{{ $t('menu.topUsers') }}</q-item-label>
            <q-item-label caption>{{ $t('menu.topUsersDesc') }}</q-item-label>
          </q-item-section>
        </q-item>
        <q-item to="/send" clickable tag="span">
          <q-item-section avatar><q-icon name="emoji_events" /></q-item-section>
          <q-item-section>
            <q-item-label>{{ $t('menu.send') }}</q-item-label>
            <q-item-label caption>{{ $t('menu.sendDesc') }}</q-item-label>
          </q-item-section>
        </q-item>
      </q-list>
    </q-drawer>
    <q-dialog v-model="enter" persistent>
      <signin />
    </q-dialog>
    <q-page-container>
      <router-view />
    </q-page-container>

    <q-footer elevated="" v-if="user.sk.length > 0">
      <q-toolbar class="flex flex-center q-pa-sm justify-evenly">
        <q-btn class="custom-button" round size="1rem" icon="account_box" to="/friends"></q-btn>
        <q-btn class="custom-button" round size="1rem" icon="sync_alt" to="/tx"></q-btn>
        <q-btn class="custom-button" round size="1rem" icon="qr_code_scanner" to="/qr"></q-btn>
        <q-btn class="custom-button" round size="1rem" icon="language" to="/global"></q-btn>
        <q-btn class="custom-button" round size="1rem" icon="manage_accounts" to="/profile"></q-btn>
      </q-toolbar>
    </q-footer>
  </q-layout>
  <ClearConfirmModal v-model="isClear" v-bind:handleConfirm="clear" />
</template>

<style>
@media (orientation: portrait) {
  .git {
    margin-left: 30px;
  }

  .gitBtn {

    display: none;
  }

  .helpBtn {
    display: none;
  }

  .topUsersBtn {
    display: none;
  }

  .homeText {
    display: none;
  }
}

.homeBtn {
  color: white;
}

.homeText {
  color: white;
}

.custom-button {
  background-color: #DDDDEB;
  color: #212235;
}
</style>
