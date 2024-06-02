<template>
  <div>
    <upload
      ref="upload"
      name="files"
      :async-save-url="asyncSaveUrl"
      :async-remove-url="asyncRemoveUrl"
      :async-auto-upload="false"
      :files="files"
      :validation-allowed-extensions="allowedExtensions"
      :validation-max-file-size="validationMaxFileSize * 1024 * 1024 * 20"
    >
    </upload>
  </div>
</template>
<style>
.k-actions {
  display: none !important;
}
</style>

<script>
import { Upload } from '@progress/kendo-upload-vue-wrapper';

export default {
  name: 'UploadComponent',
  components: {
    upload: Upload,
  },
  props: {
    files: {
      type: Array,
      required: true,
    },
    asyncSaveUrl: {
      type: String,
      default: () => '#',
    },
    asyncRemoveUrl: {
      type: String,
      default: () => '#',
    },
    allowedExtensions: {
      type: String,
      default: () => '[]',
    },
    validationMaxFileSize: {
      type: Number,
      default: () => 1,
    },
  },
  methods: {
    getFiles: function () {
      return this.$refs.upload
        .kendoWidget()
        .getFiles()
        .filter((x) => x.rawFile)
        .map((x) => x.rawFile);
      // alert('asdf');
    },
  },
  mounted() {
    var postFormData = function (url, data, fileEntry, xhr) {
      var module = this;
      fileEntry.data('request', xhr);
      setTimeout(function () {
        var e = { target: { responseText: '', statusText: 'OK', status: 200 } };
        module.onRequestSuccess.call(module, e, fileEntry);
      }, 1000);
    };
    const vm = this;
    var submitRemove = function (fileNames, eventArgs, onSuccess, onError) {
      if (confirm('確定移除?')) {
        // alert('確定清除?');
        const removeIndex = vm.files.findIndex(
          (_) => _.ID === eventArgs.files[0].ID
        );
        if (removeIndex !== -1) {
          vm.files[removeIndex].IsDelete = true;
        }
        onSuccess();
      }
    };
    var upload = this.$refs.upload.kendoWidget();
    upload._module.postFormData = postFormData;
    upload._submitRemove = submitRemove;
    // console.log(upload.async);
  },
};
</script>
