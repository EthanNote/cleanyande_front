<template>
  <div>
    <div v-for="row in getPostTable" :key="row.$index" style="clear: both">
      <div :class="'img_frame_'+rowSize" v-for="p in row" :key="p.$index">
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
    posts: {
      type: Array,
      default: function () {
        return []
      }
    },
    rowSize: {
      type: Number,
      default: 4
    },
    maxDisplay: {
      type: Number,
      default: 8
    }
  },

  computed: {
    getPostTable () {
      var result = []
      var row = []
      var postList = this.posts
      for (let i = 0; i < postList.length && i < this.maxDisplay; i++) {
        row.push(postList[i])
        if (row.length >= this.rowSize) {
          result.push(row)
          row = []
        }
      }
      if (row.length > 0) {
        result.push(row)
      }
      return result
    }
  }
}
</script>

<style>
body {
  background: #222222;
}
.img_frame_4 {
  width: 25%;
  padding-bottom: 5%;
  float: left;
}

.img_frame_1 {
  width: 100%;
  padding-bottom: 5%;
  float: left;
}

.post_img {
  width: 95%;
}

.switch {
  margin: 1em;
  padding-left: 2em;
  padding-right: 2em;
}
</style>
