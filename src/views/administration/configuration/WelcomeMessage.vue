<template>
  <div>
    <b-card no-body :header="header">
      <b-card-group>
        <b-card-body>
          <p>{{ $t('admin.welcome_message_desc') }}</p>
          <b-form-group
            :label="$t('admin.welcome_message')"
            label-size="lg"
            label-class="font-weight-bold pt-0 mb-2"
          >
            <div id="customToolbar">
              <c-switch
                id="isWelcomeMessage"
                color="primary"
                v-model="isWelcomeMessage"
                label
                v-bind="labelIcon"
              />{{ $t('admin.welcome_message_enable') }}
            </div>
            <textarea
              ref="editor"
              :disabled="!isWelcomeMessage"
              class="form-control"
              :value="welcomeMessage"
              @input="updateCode"
            />
          </b-form-group>
        </b-card-body>
      </b-card-group>
      <b-card-footer>
        <b-button
          :disabled="!this.welcomeMessage && this.isWelcomeMessage"
          variant="outline-primary"
          class="px-4"
          @click="saveChanges"
        >
          {{ $t('message.update') }}
        </b-button>
      </b-card-footer>
    </b-card>
    <div class="container">
      <b-row class="justify-content-center">
        <b-col md="8">
          <b-card v-if="isWelcomeMessage" class="bg-grey-900">
            <div slot="header">
              <h4>{{ $t('admin.preview') }}</h4>
            </div>
            <p><span v-html="welcomeMessage" /></p>
          </b-card>
        </b-col>
      </b-row>
    </div>
  </div>
</template>
<script>
import axios from 'axios';
import { Switch as cSwitch } from '@coreui/vue';
import configPropertyMixin from '../mixins/configPropertyMixin';
import common from '../../../shared/common';

export default {
  mixins: [configPropertyMixin],
  props: {
    header: String,
  },
  components: {
    cSwitch,
  },
  data() {
    return {
      isWelcomeMessage: false,
      welcomeMessage: '',
    };
  },
  created() {
    let message_url = `${this.$api.BASE_URL}/${this.$api.URL_CONFIG_PROPERTY}/public/general/welcome.message.html`;
    let enabled_url = `${this.$api.BASE_URL}/${this.$api.URL_CONFIG_PROPERTY}/public/general/welcome.message.enabled`;
    axios.get(message_url).then((response) => {
      this.welcomeMessage = decodeURIComponent(response.data.propertyValue);
    });
    axios.get(enabled_url).then((response) => {
      this.isWelcomeMessage = common.toBoolean(response.data.propertyValue);
    });
  },
  computed: {},
  methods: {
    updateCode() {
      const editor = this.$refs.editor;
      this.welcomeMessage = editor.value;
      editor.style.height = editor.scrollHeight + 'px';
    },
    saveChanges() {
      this.updateConfigProperties([
        {
          groupName: 'general',
          propertyName: 'welcome.message.html',
          propertyValue: encodeURIComponent(
            this.welcomeMessage !== '' ? this.welcomeMessage : ' ',
          ),
        },
        {
          groupName: 'general',
          propertyName: 'welcome.message.enabled',
          propertyValue: this.isWelcomeMessage,
        },
      ]);
    },
  },
};
</script>
