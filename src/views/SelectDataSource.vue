<template>
  <main class="select-data-source">
    <b-form @submit="go">
      <b-form-group
        id="select"
        :label="$t('index.specifyCatalog')"
        label-for="url"
        :invalid-feedback="error"
        :state="valid"
      >
        <b-form-input id="url" type="url" :value="url" @input="setUrl" placeholder="https://..." />
      </b-form-group>
      <b-button type="submit" variant="primary">{{ $t('index.load') }}</b-button>
    </b-form>
  </main>
</template>

<script>
import { BForm, BFormGroup, BFormInput } from 'bootstrap-vue';
import { mapGetters } from "vuex";

export default {
  name: "SelectDataSource",
  components: {
    BForm,
    BFormGroup,
    BFormInput,
  },
  data() {
    return {
      url: ''
    };
  },
  computed: {
    ...mapGetters(['toBrowserPath']),
    valid() {
      return !this.error;
    },
    error() {
      if (!this.url) {
        return null;
      }
      try {
        let url = new URL(this.url);
        if (!url.protocol) {
          return this.$t('index.urlMissingProtocol');
        }
        else if (!url.host) {
          return this.$t('index.urlMissingHost');
        }
        return null;
      } catch (error) {
        return this.$t('index.urlInvalid');
      }
    }
  },
  methods: {
    setUrl(url) {
      this.url = url;
    },
    go() {
      this.$router.push(this.toBrowserPath(this.url));
    }
  }
};
</script>

<style lang="scss">
@import '../theme/variables.scss';

#stac-browser .select-data-source {
  display: flex;
  flex-direction: column;
  flex: 1;
  overflow: hidden;

  hr {
    width: 100%;
  }
}
</style>
