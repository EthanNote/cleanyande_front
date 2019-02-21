<template>
  <div>
    <div>
      <img
        alt="yande.re"
        id="logo"
        src="https://assets.yande.re/assets/logo_small-418e8d5ec0229f274edebe4af43b01aa29ed83b715991ba14bb41ba06b5b57b5.png"
        width="50%"
      >
    </div>
    <div>
      <ul class="tab-title">
        <li>Rating</li>
        <li @click="tab='safe'" :class="{active:tab==='safe'}">Safe</li>
        <li @click="tab='questionable'" :class="{active:tab==='questionable'}">Questionable</li>
        <li @click="tab='explicit'" :class="{active:tab==='explicit'}">Explicit</li>
      </ul>
      <ul class="tab-title">
        <li>Column</li>
        <li @click="rowSize=4" :class="{active:rowSize==4}">4</li>
        <li @click="rowSize=2" :class="{active:rowSize==2}">2</li>
        <li @click="rowSize=1" :class="{active:rowSize==1}">1</li>
      </ul>
    </div>
    <br>
    <div>
      <div v-show="tab=='safe'">
        <postlist :posts="posts.safe" :maxDisplay="loadCount.safe" :rowSize="rowSize"></postlist>
      </div>
      <div v-show="tab=='questionable'">
        <postlist :posts="posts.questionable" :maxDisplay="loadCount.questionable" :rowSize="rowSize"></postlist>
      </div>
      <div v-show="tab=='explicit'">
        <postlist :posts="posts.explicit" :maxDisplay="loadCount.explicit" :rowSize="rowSize"></postlist>
      </div>
    </div>
    <div style="clear:both">
    <button class="more" v-on:click="loadCount[tab]+=8">More</button>
    </div>
    <!-- <div>{{debug}}</div> -->
    <!-- <div v-for="row in getPostTable" :key="row.$index" style="clear: both">
      <div :class="'img_frame_'+rowSize" v-for="p in row" :key="p.$index">
        <a :href="p.file_url" target="_blank">
          <img class="post_img" :src="p.preview_url">
        </a>
      </div>
    </div>-->
  </div>
</template>

<script>
import Postlist from '@/components/Postlist'

export default {
  components: {
    Postlist
  },
  data () {
    return {
      posts: {
        safe: [],
        questionable: [],
        explicit: []
      },
      loadCount: {
        safe: 8,
        questionable: 8,
        explicit: 8
      },
      tab: 'safe',
      rowSize: 4
    }
  },

  methods: {
    load () {}
  },

  computed: {
    getPostTable () {
      var result = []
      var row = []
      var postList = this.posts[this.rating]
      for (let i = 0; i < postList.length; i++) {
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
  },

  created () {
    var self = this
    fetch('https://yande.re/post.json?tags=rating:safe&limit=100').then(
      response => {
        if (response.ok) {
          response.json().then(json => {
            self.posts.safe = json
          })
        }
      }
    )

    fetch('https://yande.re/post.json?tags=rating:questionable&limit=100').then(
      response => {
        if (response.ok) {
          response.json().then(json => {
            self.posts.questionable = json
          })
        }
      }
    )

    fetch('https://yande.re/post.json?tags=rating:explicit&limit=100').then(
      response => {
        if (response.ok) {
          response.json().then(json => {
            self.posts.explicit = json
          })
        }
      }
    )

    // var sUserAgent = window.navigator.userAgent.toLowerCase()
    // this.debug = sUserAgent
    if (
      !/\/h5/.test(window.location.pathname) &&
      /iphone|nokia|sony|ericsson|mot|samsung|sgh|lg|philips|panasonic|alcatel|lenovo|cldc|midp|wap|mobile|htc|sharp|sie-|ipod|blackberry|meizu|android|netfront|symbian|ucweb|windowsce|palm|operamini|operamobi|openwave|nexusone/i.test(
        window.navigator.userAgent.toLowerCase()
      )
    ) {
      //   this.debug += '  MOBILE PLATFORM'
      this.rowSize = 1
    } else {
      //   this.debug += '  PC PLATFORM'
      this.rowSize = 4
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

.img_frame_2 {
  width: 50%;
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
.tab-title {
  width: 90%;
  clear: both;
}
ul.tab-title {
  padding: 0;
}
.tab-title li {
  float: left;
  width: 25%;
  padding: 10px 0;
  text-align: center;
  background-color: transparent;
  color: #fff;
  cursor: pointer;
  list-style: none;
}
/* 点击对应的标题添加对应的背景颜色 */
.tab-title .active {
  background-color: #09f;
  color: #fff;
}

.more{
    padding: 0.5em 4em;
    background-color: transparent;
    border: solid gray 1px;
    color: white;
}
</style>
