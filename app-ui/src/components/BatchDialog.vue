<template>
  <v-dialog v-model="show" aria-role="dialog" max-width="620" persistent aria-modal @keydown.stop="_onKeys">
    <v-card>
      <v-card-title>
        {{ message }}
      </v-card-title>
      <v-card-text>
        <div class="batch-actions mb-4">
          <v-btn color="primary" size="large" @click.prevent="next">{{ $t('batch-dialog.btn-next') }}</v-btn>
          <v-btn v-if="onFinish" color="green" size="large" @click.prevent="finish">{{ $t('batch-dialog.btn-finish') }}</v-btn>
          <v-btn v-if="onRescan" size="large" @click.prevent="rescan">{{ $t('batch-dialog.btn-rescan') }}</v-btn>
          <v-btn color="warning" size="large" @click.prevent="show = false">{{ $t('batch-dialog.btn-cancel') }}</v-btn>
        </div>
        <v-img v-if="image" :src="'data:image/jpeg;base64,' + image" max-height="65vh" contain />
      </v-card-text>
      <v-card-actions />
    </v-card>
  </v-dialog>
</template>

<script>
import Constants from '../classes/constants';

export default {
  name: 'BatchDialog',
  data() {
    return {
      message: null,
      image: null,
      show: false,
      onFinish: null,
      onNext: null,
      onRescan: null,
    };
  },

  methods: {
    _onKeys(event) {
      if (event.keyCode === Constants.Keys.enter) {
        this.ok();
      }
    },

    finish() {
      this.show = false;
      if (this.onFinish) {
        this.onFinish();
      }
    },

    rescan() {
      this.show = false;
      if (this.onRescan) {
        this.onRescan();
      }
    },

    next() {
      this.show = false;
      if (this.onNext) {
        this.onNext();
      }
    },

    open(options) {
      this.message = options.message;
      this.image = options.image;
      this.onFinish = options.onFinish;
      this.onRescan = options.onRescan;
      this.onNext = options.onNext;
      this.show = true;
    }
  }
};
</script>

<style scoped>
/* Large, evenly sized targets kept above the preview so they never scroll away */
.batch-actions {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 8px;
}

.batch-actions .v-btn {
  height: 52px;
  font-size: 1rem;
}
</style>
