<template>
  <div class="page home">
    <Folders :currentFolderId="currentFolderId" />
    <div class="todos-wrapper">
      <Todos
        v-for="folder in getFolders"
        :key="folder.id"
        :folderId="folder.id"
      />
    </div>
  </div>
</template>

<script>
import { mapActions, mapState } from 'vuex';

import Folders from '@/components/Folders';
import Todos from '@/components/Todos';

export default {
  async created() {
    const response = await this.api.getAllFolders();
    this.setFolders(response.data);
  },
  inject: ['api'],
  name: 'Home',
  components: {
    Folders,
    Todos
  },
  methods: {
    ...mapActions(['setFolders'])
  },
  computed: {
    ...mapState(['folders']),
    currentFolderId: {
      cache: false,
      get() {
        return this.$route.query.folder;
      }
    },
    getFolders: {
      get() {
        const folderId = this.currentFolderId;
        if (this.folders) {
          if (folderId) {
            return this.folders.filter(folder => folder.id === folderId);
          } else {
            return this.folders;
          }
        }

        return [];
      }
    }
  }
};
</script>
