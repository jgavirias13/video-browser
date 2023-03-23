<template>
  <div class="container">
    <SearchBar @termChange="onTermChange" />
    <div class="row">
      <VideoDetail :video="selectedVideo"/>
      <VideoList :videos="videos" @videoSelected="onVideoSelected"/>
    </div>
  </div>
</template>

<script>

import axios from 'axios';
import SearchBar from './components/SearchBar.vue';
import VideoList from './components/VideoList.vue';
import VideoDetail from './components/VideoDetail.vue';

const API_KEY = '';

export default {
    name: "App",
    components: { SearchBar, VideoList, VideoDetail },
    data() {
      return {
        videos: [],
        selectedVideo: null
      }
    },
    methods: {
      onTermChange(searchTerm) {
        axios.get('https://www.googleapis.com/youtube/v3/search', {
          params: {
            key: API_KEY,
            type: 'video',
            part: 'snippet',
            q: searchTerm
          }
        }).then(response => {
          if (response.status === 200) {
            this.videos = response.data?.items;
          }
        });
      },

      onVideoSelected(video) {
        this.selectedVideo = video;
      }
    }
};

</script>