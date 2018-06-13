<template>
  <div class="container-fluid">
    <h3>Video player</h3>
    <form action="#" class="form-inline" v-on:submit.prevent="onSubmit">
      <!-- <div class="form-group"> -->
      <input type="text" v-model="source" class="form-control" style="width: 85%">
      <button class="btn btn-outline-primary ml-2" @click="requestGenerateFileTreeObject(source)">Refresh</button>
    </form>
    <form action="#" class="form-inline" v-on:submit.prevent="onSubmit">
      <select class="form-control ml-2" v-model="speed">
        <option value="1">1</option>
        <option value="1.25">1.25</option>
        <option value="1.5">1.5</option>
        <option value="1.75">1.75</option>
        <option value="2">2</option>
        <option value="2.5">2.5</option>
        <option value="3">3</option>
      </select>
    </form>
    <div class="embed-responsive embed-responsive-16by9">
      <video controls :src="currentPath"></video>
    </div>
    <ul>
      <li v-for="(file, index) in files" v-if="file.isValid()">
        <file-component :file="file" :index="index" :play="play"></file-component>
      </li>
    </ul>
  </div>
</template>

<script>
// const fs = require('fs');
var Promise = require("bluebird");
var fs = Promise.promisifyAll(require("fs")); //This is most convenient way if it works for you


export default {
  name: 'test',
  data () {
    return {
      text: '',
      files: [],
      source: '/Users/sebastienlavoie/Dropbox/All/04_Resources/Documentation/Tutorials/Photoshop/LearnSquared - Narrative Concept Art/1. Design Principles',
      speed: 3,
      currentlyOpened: 0,
      currentPath: ""
    }
  },

  watch: {
    speed: function() {
      this.setSpeed(this.speed)
    }
  },

  mounted () {
    this.requestGenerateFileTreeObject(this.source)
  },

  components: {
    FileComponent: require("./file.vue")
  },

  methods:  {
    play: function(file) {
      this.currentPath = file.path
    },

    setSpeed: function(speed) {
      var videos = document.getElementsByTagName("video")[0].playbackRate = speed;
    },

    requestGenerateFileTreeObject: function(directoryString) {
      this.files = [];
      this.generateFileTreeObject(directoryString);
    },

    generateFileTreeObject: function(directoryString) {
      var _this = this;
      return fs.readdirAsync(directoryString)
      .then(arrayOfFileNameStrings => {
        const fileDataPromises = arrayOfFileNameStrings.map(fileNameString => {
          const fullPath = `${directoryString}/${fileNameString}`;
          return fs.statAsync(fullPath)
          .then(fileData => {
            const file = {};
            file.filePath = fullPath;
            if(fileData.isFile()) {
              _this.files.push(new File(file))
            } else {
              // recurse
              return _this.generateFileTreeObject(file.filePath)
              .then(fileNamesSubArray => {
                file.files = fileNamesSubArray;
              })
              .catch(console.error);
            }
            /*End recursive condition*/
            return file;
          });
        });
        return Promise.all(fileDataPromises);
      });
    }
  }
}

class File {

  constructor(file) {
    this.path = file.filePath
    this.name = this.path.replace(app.source, "").replace(/^\//, "")
    this.extension = this.name.split('.').pop();
  }

  isValid() {
    var isNotHiddenFile = this.name.charAt(0) !== "."
    var isVideoFile = ['mp4', 'mov'].includes(this.extension)
    if(isNotHiddenFile && isVideoFile) {
      return true;
    }

    return false;
  }
}
</script>
