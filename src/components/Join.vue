<script>
  import { usePlayerStore } from '../main';

  export default {
    setup() {
      const store = usePlayerStore();

      return {
        store
      }
    },
    data() {
      return {
        peer_client: null,
        id_join: '',
        btn_text: this.$t('in_progress')
      }
    },
    computed: {
      isBtnDisabled() {
        if (this.id_join.length === 0) {
          this.btn_text =  this.$t('in_progress');
          return true;
        }
        this.btn_text =  this.$t('join');
        return false;
      },
    },
    mounted() {
      if (this.$route.query.id !== undefined && this.$route.query.id !== '') {
        this.id_join = this.$route.query.id;
        this.store.join(this.$route.query.id)
      }
      localStorage.removeItem('temp_peer');
    }
  }
</script>

<template>
  <Transition name="fade">
    <div id="wrapper-message" v-if="store.message">
      <div>{{ store.message }}</div>
    </div>
  </Transition>

  <h1>{{ $t("character_manage") }}</h1>
  <div class="small-wrapper menu-wrapper">
    <label for="id_join">{{ $t("game_id") }}</label>
    <input name="id_join" autocomplete="off" type="text" id="id_join" v-model="id_join">
    <button id="join" :disabled="isBtnDisabled" v-on:click="store.join(id_join)">{{ btn_text }}</button>
  </div>
</template>

<style lang="scss">
  .fade-enter-active,
  .fade-leave-active {
    > div {
      opacity: 1;
    }
  }

  .fade-enter-from,
  .fade-leave-to {
    > div {
      opacity: 0;
    }
  }

  #wrapper-message {
    display: flex;
    position: absolute;
    bottom: 30px;
    right: 30px;

    > div {
      margin: auto;
      padding: 15px;
      background: white;
      border-radius: 10px;
      color: black;
      transition: opacity 1s ease;
    }
  }
</style>
