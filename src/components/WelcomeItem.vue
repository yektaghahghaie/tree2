<template>
   <div>
          <button @click="addFile">Add File</button>
          <button @click="addFolder">Add Folder</button>
          <ul>
            <li>
              <span>Main File</span>
              <ul>
                <li v-for="file in files" :key="file.id">
                  <span v-if="!file.folder">{{ file.name }}</span>
                  <span v-else @click="toggleFolder(file.id)">{{ file.name }}</span>
                  <button @click="deleteItem(file.id)">Delete</button>
                  <ul v-if="file.folder && file.opened">
                    <li v-for="subfile in file.subfiles" :key="subfile.id">
                      <span v-if="!subfile.folder">{{ subfile.name }}</span>
                      <span v-else @click="toggleFolder(subfile.id)">{{ subfile.name }}</span>
                      <button @click="deleteItem(subfile.id)">Delete</button>
                    </li>
                    <li>
                      <button @click="addSubfile(file.id)">Add Subfile</button>
                      <button @click="addSubfolder(file.id)">Add Subfolder</button>
                    </li>
                  </ul>
                </li>
              </ul>
            </li>
          </ul>
        </div>




      
</template>
<script>
import { ref, reactive } from 'vue';



export default {
  setup() {
    const files = reactive([
      {
        id: 1,
        name: 'Main File',
        folder: true,
        opened: false,
        subfiles: [
          { id: 2, name: 'Subfile 1', folder: false },
          { id: 3, name: 'Subfile 2', folder: false }
        ]
      }
    ]);

    const addFile = () => {
      const newFile = {
        id: Date.now(),
        name: `File ${files.length + 1}`,
        folder: false
      };
      files.push(newFile);
    };

    const addSubfile = (parentId) => {
      const parentFile = files.find((file) => file.id === parentId);
      if (parentFile) {
        const newSubfile = {
          id: Date.now(),
          name: `Subfile ${parentFile.subfiles.length + 1}`,
          folder: false
        };
        parentFile.subfiles.push(newSubfile);
      }
    };

    const addFolder = () => {
      const newFolder = {
        id: Date.now(),
        name: `Folder ${files.length + 1}`,
        folder: true,
        opened: true,
        subfiles: []
      };
      files.push(newFolder);
    };

    const addSubfolder = (parentId) => {
      const parentFile = files.find((file) => file.id === parentId);
      if (parentFile) {
        const newSubfolder = {
          id: Date.now(),
          name: `Subfolder ${parentFile.subfiles.length + 1}`,
          folder: true,
          opened: true,
          subfiles: []
        };
        parentFile.subfiles.push(newSubfolder);
      }
    };

    const toggleFolder = (id) => {
      const file = files.find((file) => file.id === id);
      if (file) {
        file.opened = !file.opened;
      }
    };

    const deleteItem = (id) => {
      const index = files.findIndex((file) => file.id === id);
      if (index > -1) {
        files.splice(index, 1);
      }
    };

    return {
      files,
      addFile,
      addSubfile,
      addFolder,
      addSubfolder,
      toggleFolder,
      deleteItem
    };
  },



  
};
</script>
<style scoped>

</style>
