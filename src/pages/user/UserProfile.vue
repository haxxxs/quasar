<script setup>
import { computed } from 'vue'
import useFetchData from 'src/composables/useFetchData'
//
const { getUserById } = useFetchData()
const { result } = getUserById(1)

const user = computed(() => {
  return result.value?.getUser
})

</script>

<template>
  <q-page class="container overflow-y-hidden shadow-10">
    <div class="content" v-if="user">
      <div class="contentWrapper q-pa-md">
        <div class="upperSection">
          <div class="user-data shadow-21">
            <q-avatar size="90px" class="shadow-10">
              <img :src="user.cover">
            </q-avatar>
            <div class="left">
              <p class="user-field">{{ user.name }}
                <q-icon name="edit" class="hoverable" />
              </p>
              <p class="user-field">{{ user.addr }}
                <q-icon name="edit" class="hoverable" />
              </p>
              <p class="user-field">{{ user.desc }}
                <q-icon name="edit" class="hoverable" />
              </p>
              <p class="user-field">{{ user.sign }}
                <q-icon name="content_copy" class="hoverable" />
              </p>
            </div>

          </div>
        </div>

        <q-card-actions class="btn-block">
          <q-btn color="primary" class="btn-user" label="Поддержка" @click="support" />
          <q-btn color="primary" class="btn-user shadow-21" @click="send" label="Отправить" />
          <q-btn color="primary" class="btn-user" label="Настройки" @click="settings" />
        </q-card-actions>
      </div>
    </div>
    <q-spinner v-else color="primary" size="3em" />
  </q-page>
</template>

<style scoped lang="scss">
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  margin: 0 auto;
  padding: 4px 60px;
  max-height: 100dvh;

  .content {
    display: flex;

    .contentWrapper {
      display: flex;
      flex-direction: column;
      align-items: center;
      background-color: #eaeaea;
      border-radius: 20px;
      width: 300px;

      .upperSection {
        width: 100%;

        .user-data {
          display: flex;
          align-items: center;
          flex-direction: column;
          background-color: #eaeaea;
          color: black;

          img {
            background: black;
          }

          .left {
            display: flex;
            flex-direction: column;
            margin-top: 30px;
            margin-bottom: 10px;
            width: 100%;

            p {
              margin-right: 15px;
            }

            .hoverable {
              margin-left: -10px;
            }
          }
        }
      }
    }
  }

  .btn-block {
    display: flex;
    align-items: center;
    justify-content: space-around;
    flex-direction: row;
    padding: 14px 0;
    background-color: #eaeaea;

    .btn-user {
      border-radius: 30px;
    }

  }
}

.row {
  display: flex;
  gap: 20px;
}

.ma-auto {
  margin: 0 auto;
}

.user-field {
  display: flex;
  gap: 14px;
  align-items: center;
  justify-content: space-between;
  cursor: pointer;
}

.hoverable:hover {
  color: $accent;
}

.hoverable:active {
  color: $accent;
}
</style>

<!-- <template>
  <q-input label="Имя" placeholder="На балансе должно быть более 100ѣ" variant="underlined"></q-input>
  <q-input placeholder="Pick a cover" label="Добавьте обложку" hint="На балансе должно быть более 500ѣ"></q-input>
  <q-input type="textarea" label="Описание" no-resize
    hint="Для хранения описания держите на балансе более 250ѣ"></q-input>
  <div class="q-mt-md q-mb-lg"><q-btn color="primary" size="xl" @click="send" label="Отправить" /></div>
</template> -->
