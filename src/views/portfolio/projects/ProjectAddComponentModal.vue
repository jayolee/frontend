<template>
  <b-modal
    id="projectAddComponentModal"
    size="md"
    hide-header-close
    no-stacking
    :title="$t('message.component_details')"
  >
    <b-tabs class="body-bg-color" style="border: 0; padding: 0">
      <b-tab class="body-bg-color" style="border: 0; padding: 0" active>
        <template v-slot:title
          ><i class="fa fa-cube"></i> {{ $t('message.identity') }}</template
        >
        <b-card>
          <b-input-group-form-input
            id="component-name-input"
            input-group-size="mb-3"
            type="text"
            v-model="component.name"
            lazy="true"
            required="true"
            feedback="true"
            autofocus="false"
            :label="$t('message.component_name')"
            :tooltip="this.$t('message.component_name_desc')"
            :feedback-text="$t('message.required_component_name')"
          />
          <b-input-group-form-input
            id="component-version-input"
            input-group-size="mb-3"
            type="text"
            v-model="component.version"
            lazy="true"
            required="true"
            feedback="true"
            autofocus="false"
            :label="$t('message.version')"
            :tooltip="this.$t('message.component_version_desc')"
            :feedback-text="$t('message.required_component_version')"
          />
          <b-input-group-form-input
            id="component-group-input"
            input-group-size="mb-3"
            type="text"
            v-model="component.group"
            required="false"
            :label="$t('message.component_namespace_group_vendor')"
            :tooltip="this.$t('message.component_group_desc')"
          />
          <b-input-group-form-input
            id="component-purl-input"
            input-group-size="mb-3"
            type="text"
            v-model="component.purl"
            required="false"
            :label="$t('message.package_url_full')"
            :tooltip="this.$t('message.component_package_url_desc')"
          />
          <b-input-group-form-input
            id="component-cpe-input"
            input-group-size="mb-3"
            type="text"
            v-model="component.cpe"
            required="false"
            :label="$t('message.cpe_full')"
            :tooltip="$t('message.component_cpe_desc')"
          />
          <b-input-group-form-input
            id="component-swidTagId-input"
            input-group-size="mb-3"
            type="text"
            v-model="component.swidTagId"
            required="false"
            :label="$t('message.swid_tagid')"
            :tooltip="$t('message.component_swid_tagid_desc')"
          />
        </b-card>
      </b-tab>
      <b-tab>
        <template v-slot:title
          ><i class="fa fa-caret-square-o-right"></i>
          {{ $t('message.extended') }}</template
        >
        <b-card>
          <b-input-group-form-select
            id="component-classifier-input"
            required="true"
            v-model="component.classifier"
            :options="sortAvailableClassifiers"
            :label="$t('message.classifier')"
            :tooltip="$t('message.component_classifier_desc')"
          />
          <b-input-group-form-input
            id="component-filename-input"
            input-group-size="mb-3"
            type="text"
            v-model="component.filename"
            required="false"
            :label="$t('message.filename')"
            :tooltip="$t('message.component_filename_desc')"
          />
          <b-form-group
            id="component-description-form-group"
            :label="this.$t('message.description')"
            label-for="component-description-input"
          >
            <b-form-textarea
              id="component-description-description"
              v-model="component.description"
              rows="3"
            />
          </b-form-group>
        </b-card>
      </b-tab>
      <b-tab>
        <template v-slot:title
          ><i class="fa fa-balance-scale"></i>
          {{ $t('message.legal') }}</template
        >
        <b-card>
          <b-input-group-form-select
            id="component-license-input"
            required="false"
            v-model="selectedLicense"
            :options="selectableLicenses"
            :label="$t('message.license')"
            :tooltip="$t('message.component_spdx_license_desc')"
          />
          <b-input-group-form-input
            id="component-license-expression"
            input-group-size="mb-3"
            type="text"
            v-model="component.licenseExpression"
            required="false"
            :label="$t('message.license_expression')"
            :tooltip="$t('message.component_license_expression_desc')"
          />
          <b-input-group-form-input
            id="component-license-url-input"
            input-group-size="mb-3"
            type="text"
            v-model="component.licenseUrl"
            required="false"
            :label="$t('message.license_url')"
            :tooltip="$t('message.component_license_url_desc')"
          />
          <b-form-group
            id="component-copyright-form-group"
            :label="this.$t('message.copyright')"
            label-for="component-copyright-input"
          >
            <b-form-textarea
              id="component-description-description"
              v-model="component.copyright"
              rows="3"
            />
          </b-form-group>
        </b-card>
      </b-tab>
      <b-tab>
        <template v-slot:title
          ><i class="fa fa-barcode"></i> {{ $t('message.hashes') }}</template
        >
        <b-card>
          <b-input-group-form-input
            id="component-md5-input"
            input-group-size="mb-3"
            type="text"
            v-model="component.md5"
            required="false"
            :label="$t('hashes.md5')"
            :tooltip="$t('message.component_hash_desc')"
          />
          <b-input-group-form-input
            id="component-sha1-input"
            input-group-size="mb-3"
            type="text"
            v-model="component.sha1"
            required="false"
            :label="$t('hashes.sha_1')"
            :tooltip="$t('message.component_hash_desc')"
          />
          <b-input-group-form-input
            id="component-sha256-input"
            input-group-size="mb-3"
            type="text"
            v-model="component.sha256"
            required="false"
            :label="$t('hashes.sha_256')"
            :tooltip="$t('message.component_hash_desc')"
          />
          <b-input-group-form-input
            id="component-sha512-input"
            input-group-size="mb-3"
            type="text"
            v-model="component.sha512"
            required="false"
            :label="$t('hashes.sha_512')"
            :tooltip="$t('message.component_hash_desc')"
          />
          <b-input-group-form-input
            id="component-sha3256-input"
            input-group-size="mb-3"
            type="text"
            v-model="component.sha3_256"
            required="false"
            :label="$t('hashes.sha3_256')"
            :tooltip="$t('message.component_hash_desc')"
          />
          <b-input-group-form-input
            id="component-sha3512-input"
            input-group-size="mb-3"
            type="text"
            v-model="component.sha3_512"
            required="false"
            :label="$t('hashes.sha3_512')"
            :tooltip="$t('message.component_hash_desc')"
          />
        </b-card>
      </b-tab>
      <b-tab>
        <template v-slot:title
          ><i class="fa fa-file-text-o"></i> {{ $t('message.notes') }}</template
        >
        <b-card>
          <b-form-group
            id="component-notes-form-group"
            :label="this.$t('message.notes')"
            label-for="component-notes-input"
          >
            <b-form-textarea
              id="component-notes-description"
              v-model="component.notes"
              rows="3"
            />
          </b-form-group>
        </b-card>
      </b-tab>
    </b-tabs>
    <template v-slot:modal-footer="{ cancel }">
      <b-button size="md" variant="secondary" @click="cancel()">{{
        $t('message.close')
      }}</b-button>
      <b-button
        size="md"
        variant="primary"
        @click="createComponent()"
        v-permission="PERMISSIONS.PORTFOLIO_MANAGEMENT"
        >{{ $t('message.create') }}</b-button
      >
    </template>
  </b-modal>
</template>

<script>
import BInputGroupFormInput from '../../../forms/BInputGroupFormInput';
import BInputGroupFormSelect from '../../../forms/BInputGroupFormSelect';
import permissionsMixin from '../../../mixins/permissionsMixin';
import availableClassifiersMixin from '@/mixins/availableClassifiersMixin';

export default {
  name: 'ProjectAddComponentModal',
  mixins: [permissionsMixin, availableClassifiersMixin],
  components: {
    BInputGroupFormInput,
    BInputGroupFormSelect,
  },
  props: {
    uuid: String,
  },
  data() {
    return {
      selectableLicenses: [],
      selectedLicense: '',
      component: {},
    };
  },
  beforeMount() {
    this.resetValues();
    this.retrieveLicenses();
  },
  methods: {
    createComponent: function () {
      this.$root.$emit('bv::hide::modal', 'projectAddComponentModal');
      let url = `${this.$api.BASE_URL}/${this.$api.URL_COMPONENT}/project/${this.uuid}`;
      this.axios
        .put(url, {
          name: this.component.name,
          version: this.component.version,
          group: this.component.group,
          description: this.component.description,
          license: this.selectedLicense,
          licenseExpression: this.component.licenseExpression,
          licenseUrl: this.component.licenseUrl,
          filename: this.component.filename,
          classifier: this.component.classifier,
          purl: this.component.purl,
          cpe: this.component.cpe,
          swidTagId: this.component.swidTagId,
          copyright: this.component.copyright,
          md5: this.component.md5,
          sha1: this.component.sha1,
          sha256: this.component.sha256,
          sha512: this.component.sha512,
          sha3_256: this.component.sha3_256,
          sha3_512: this.component.sha3_512,
          notes: this.component.notes,
        })
        .then((response) => {
          this.$emit('refreshTable');
          this.$toastr.s(this.$t('message.component_created'));
        })
        .catch((error) => {
          this.$toastr.w(this.$t('condition.unsuccessful_action'));
        });
      this.resetValues();
    },
    resetValues: function () {
      this.component = {
        name: null,
        version: null,
        group: null,
        description: null,
        license: null,
        licenseExpression: null,
        licenseUrl: null,
        filename: null,
        classifier: null,
        purl: null,
        cpe: null,
        swidTagId: null,
        copyright: null,
        md5: null,
        sha1: null,
        sha256: null,
        sha512: null,
        sha3_256: null,
        sha3_512: null,
        notes: null,
      };
    },
    retrieveLicenses: function () {
      let url = `${this.$api.BASE_URL}/${this.$api.URL_LICENSE_CONCISE}`;
      this.axios
        .get(url)
        .then((response) => {
          // Allow for license to be un-selected.
          this.selectableLicenses.push({ value: '', text: '' });
          for (let i = 0; i < response.data.length; i++) {
            let license = response.data[i];
            this.selectableLicenses.push({
              value: license.licenseId,
              text: license.name,
            });
          }
        })
        .catch((error) => {
          this.$toastr.w(this.$t('condition.unsuccessful_action'));
        });
    },
  },
};
</script>

<style scoped>
.tab-content .tab-pane {
  padding: 0 !important;
}
.tab-content {
  border: 0 !important;
}
.card {
  border: 0;
  padding: 0;
  margin-bottom: 0;
}
</style>
