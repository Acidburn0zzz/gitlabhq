<script>
  import { mapActions } from 'vuex';
  import icon from '~/vue_shared/components/icon.vue';
  import router from '../../ide_router';

  export default {
    components: {
      icon,
    },
    props: {
      file: {
        type: Object,
        required: true,
      },
    },
    computed: {
      iconName() {
        return this.file.tempFile ? 'file-addition' : 'file-modified';
      },
      iconClass() {
        return `multi-file-${this.file.tempFile ? 'addition' : 'modified'} append-right-8`;
      },
    },
    methods: {
      ...mapActions([
        'discardFileChanges',
        'updateViewer',
      ]),
      openFileInEditor(file) {
        this.updateViewer('diff');

        router.push(`/project${file.url}`);
      },
    },
  };
</script>

<template>
  <div class="multi-file-commit-list-item">
    <button
      type="button"
      class="multi-file-commit-list-path"
      @click="openFileInEditor(file)">
      <span class="multi-file-commit-list-file-path">
        <icon
          :name="iconName"
          :size="16"
          :css-classes="iconClass"
        />{{ file.path }}
      </span>
    </button>
    <button
      type="button"
      class="btn btn-blank multi-file-discard-btn"
      @click="discardFileChanges(file.path)"
    >
      Discard
    </button>
  </div>
</template>
