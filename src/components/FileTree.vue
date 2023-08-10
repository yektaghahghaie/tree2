<template>
  <div class="header">
    <div>
      <div @click="addFile">
        <img src="../components/poshe.webp" class="file" style="width: 20px; height: 30px"/>
      </div>
      <input v-model="FileName" v-if="FileNameInput" @keyup.enter="saveFile" />
    </div>
    <div>
      <div @click="addFolder">
        <img src="../components/poshe.webp" class="folder" style="width: 30px; height: 30px"/>
      </div>
      <input v-model="FolderName" v-if="FolderNameInput" @keyup.enter="saveFolder"/>
    </div>
  </div>
  <ul>
    <li class="file" v-for="file in rootFolder.files" :key="file.id">
      <div class="header">
        <div>{{ file.name }}</div>
        <div :disabled="!file.Delete" @click="deleteFile(file.id)">
          <img src="../components/cross.svg" style="width: 15px; height: 15px"/>
        </div>
      </div>
    </li>

    <li class="folder" v-for="folder in rootFolder.folders" :key="folder.id">
      <div class="header">
        <details>
          <summary>{{ folder.name }}</summary>
          <FileTree />
        </details>
        <div :disabled="!folder.Delete" @click="deleteFolder(folder.id)">
          <img src="../components/cross.svg" style="width: 15px; height: 15px" />
        </div>
      </div>
    </li>
  </ul>
</template>

<script>
export default {
  data() {
    return {
      FileName: "",
      FolderName: "",
      FileNameInput: false,
      FolderNameInput: false,
      rootFolder: {
        id: 1,
        name: "Root",
        files: [],
        folders: [],
        Delete: false,
      },
    };
  },
  methods: {
    addFile() {
      this.FileNameInput = true;
    },
    saveFile() {
      const file = {
        id: Date.now(),
        name: this.FileName,
        Deletable: true,
      };
      this.rootFolder.files.push(file);
      this.FileName = "";
      this.FileNameInput = false;
    },
    deleteFile(fileId) {
      const index = this.rootFolder.files.findIndex(
        (file) => file.id === fileId
      );
      this.rootFolder.files.splice(index, 1);
    },
    addFolder() {
      this.FolderNameInput = true;
    },
    saveFolder() {
      const folder = {
        id: Date.now(),
        name: this.FolderName,
        files: [],
        folders: [],
        Delete: true,
      };
      this.rootFolder.folders.push(folder);
      this.FolderName = "";
      this.FolderNameInput = false;
    },
    deleteFolder(folderId) {
      const index = this.rootFolder.folders.findIndex(
        (folder) => folder.id === folderId
      );
      this.rootFolder.folders.splice(index, 1);
    },
  },
};
</script>

<style>
li button[disabled] {
  opacity: 0.5;
  cursor: not-allowed;
}
body {
  background-color: rgb(242, 237, 247);
}
.header {
  display: flex;
  gap: 30px;
}
li {
  list-style: none;
}
.file:hover {
  cursor: pointer;
}
.folder:hover {
  cursor: pointer;
}
</style>