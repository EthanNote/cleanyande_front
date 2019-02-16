<template>
  <div>
    <div id="header">
      <img
        alt="yande.re"
        id="logo"
        src="https://assets.yande.re/assets/logo_small-418e8d5ec0229f274edebe4af43b01aa29ed83b715991ba14bb41ba06b5b57b5.png"
        width="50%"
      >
    </div>
    <div v-for="row in getPostTable" :key="row.$index" style="clear: both">
      <div class="img_frame" v-for="p in row" :key="p.$index">
        <a :href="p.file_url" target="_blank">
          <img class="post_img" :src="p.preview_url">
        </a>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    rowSize: {
      type: Number,
      default: 4
    }
  },
  data() {
    return {
      posts: []
    };
  },

  methods: {
    load() {}
  },

  computed: {
    getPostTable() {
      var result = [];
      var row = [];
      for (let i = 0; i < this.posts.length; i++) {
        row.push(this.posts[i]);
        if (row.length >= this.rowSize) {
          result.push(row);
          row = [];
        }
      }
      if (row.length > 0) {
        result.push(row);
      }
      return result;
    }
  },

  created() {
    var self = this;
    fetch("http://118.24.43.249:5000/data/post").then(response => {
      if (response.ok) {
        // console.log(response.json())
        response.json().then(json => {
          // console.log(json)
          //   console.log(self)
          self.posts = json;
        });
      }
    });
  }
};
</script>

<style>
body {
  background: #222222;
}
.img_frame {
  width: 25%;
  padding-bottom: 5%;
  float: left;
}
.post_img {
  width: 95%;
}

#header {
  text-align: left;
}
</style>
