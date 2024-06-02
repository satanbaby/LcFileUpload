<template>
  <div>
    <input type="button" value="送出" @click="submit" />
    <div class="demo-section k-content">
      <LcFileUpload
        ref="files"
        name="files"
        :files="files"
        :allowed-extensions="['.pdf', 'docx']"
      >
      </LcFileUpload>
      {{ files }}
    </div>
  </div>
</template>

<script>
// import { Upload } from '@progress/kendo-upload-vue-wrapper';
import LcFileUpload from './LcFileUpload.vue';

export default {
  name: 'App',
  components: {
    LcFileUpload: LcFileUpload,
  },
  data: function () {
    return {
      files: [
        { ID: 1, name: 'file1.doc', size: 525, extension: '.doc' },
        { ID: 2, name: 'file2.jpg', size: 600, extension: '.jpg' },
        { ID: 3, name: 'file3.xls', size: 720, extension: '.xls' },
      ],
    };
  },
  methods: {
    submit: function () {
      const vm = this;
      console.log('存在', vm.files);
      console.log('新上傳', vm.$refs.files.getFiles());
    },
  },
};
</script>
