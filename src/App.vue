<template>
  <div class="card">
    <div class="top">
      <p>Drag & Drop File</p>
    </div>
    <div class="drag-area">
      <span v-if="!isDragging">
        Drag & Drop files here
        <span class="select" role="button" @click="selectFiles">
          Choose File
        </span>
      </span>
      <div v-else class="select">Drop files here</div>
      <input
        type="file"
        name="file"
        class="file"
        ref="fileInput"
        multiple
        @change="onFileSelect"
      />
    </div>
    <div class="container">
      <div class="image" v-for="(file, index) in files" :key="onFileSelect">
        <ul>
          <li>{{ file.name }}</li>
          <li class="percentage" :style="{ width: percentage + '%' }">
            {{ percent }}
          </li>
        </ul>
      </div>
    </div>
    <button type="button">Upload</button>
  </div>
</template>

<script>
export default (await import("vue")).defineComponent({
  data() {
    return {
      files: [],
      isDragging: false,
      percentage: 0,
    };
  },
  created() {
    let interval = setInterval(() => {
      if (this.percentage < 100) {
        this.percentage += 0.1;
      } else clearInterval(interval);
    }, 5);
  },
  computed: {
    percent() {
      return this.percentage.toFixed();
    },
  },
  methods: {
    selectFiles() {
      this.$refs.fileInput.click();
    },
    onFileSelect(event) {
      const files = event.target.files;
      if (files.length === 0) return;
      for (let i = 0; i < files.length; i++) {
        if (!this.files.some((e) => e.name === files[i].name)) {
          this.files.push({
            name: files[i].name,
            url: URL.createObjectURL(files[i]),
          });
        }
      }
    },
  },
});
</script>
<style scoped>
.card {
  widows: 100%;
  padding: 10px;
  box-shadow: 0 0 5px #ffdfdf;
  border-radius: 5px;
  overflow: hidden;
}
.card .top {
  text-align: center;
}
.card p {
  font-weight: bold;
  color: #fe0000;
}
.card .container .image {
  width: 75px;
  margin-right: 5px;
  height: 75px;
  position: relative;
  margin-bottom: 8px;
}
.card button {
  width: 100%;
  margin-top: 10px;
  outline: 0;
  border: 0;
  color: #fff;
  border-radius: 4px;
  font-weight: 400;
  padding: 8px 13px;
  background-color: #fe0000;
}

.percentage {
  background-color: rgb(15, 78, 121);
  color: black;
  padding: 2px 2px 2px 2px;
  text-align: center;
}
.card .drag-area {
  height: 150px;
  border-radius: 5px;
  border: 2px dashed #fe0000;
  background: #f4f3f9;
  color: #fe0000;
  display: flex;
  justify-content: center;
  align-items: center;
  user-select: center;
  -webkit-user-select: none;
  margin-top: 10px;
}
.card .drag-area .visible {
  font-size: 10px;
}
.card .select {
  color: #5256ad;
  margin-left: 5px;
  cursor: pointer;
  transition: 0.4s;
}
.card .select:hover {
  opacity: 0.6s;
}
.card .container {
  width: 100%;
  height: auto;
  display: flex;
  justify-content: flex-start;
  align-items: flex-start;
  flex-wrap: wrap;
}
.card .container .image {
  width: 100%;
  height: 100%;
  border-radius: 5px;
}

.card .container .image span {
  position: absolute;
  top: -2px;
  right: 9px;
  font-size: 20px;
  cursor: pointer;
}
.card input,
.card .drag-area .on-drop,
.card .drag-area .dragover .visible {
  display: none;
}
.delete {
  z-index: 999;
  color: #fe0000;
}
</style>
