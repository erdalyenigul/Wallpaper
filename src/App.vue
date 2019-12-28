<template>
  <div class="mainWrapper">
    <div class="middleWrap">
      <div class="errorMsg" v-if="errorMsg">{{ errorMsg }}</div>
      <div class="header">  
        <div class="headLogo">
          <a href="/wallpaper" class="img"><img src="./assets/images/logo.png"></a>
          <h1 class="headText">Free Mobile And Desktop <br> <span>Wallpapers</span></h1>
        </div>  
        <div class="searchInput">
          <input type="text" name="search" placeholder="Search photo.." 
          v-model="searchKey" 
          v-on:keyup.enter="getPhotosApi">
          <a href="javascript:;" @click="getPhotosApi">Search</a>
        </div>        
      </div>
      <router-view></router-view>     
      <div class="currentPage" v-if="blocks">Page : {{ pageNumber }} / {{ totalPages }}</div>
      <div class="listImg" v-if="blocks">
        <div class="listImgItem" v-for="(items, index) in blocks">
          <div class="listImgItemIn">             
            <a href="javascript:;" class="imgBigModalBtn">
              <img :src="data[index].urls.small"  v-on:click="imgIndex(index)">
            </a>
          </div>
        </div>
        <div class="prevNextBtns" v-if="blocks">
          <button class="prevBtn" v-on:click="prevPage(); backToTop()" v-if="pageNumber > 1"><img src="./assets/images/left.png"> <span>Prev Page</span></button>          
          <button class="nextBtn" v-on:click="nextPage(); backToTop()"><span>Next Page</span> <img src="./assets/images/right.png"></button>
        </div>  
        <div class="imgBigModal" v-if="index > -1">  
          <a href="javascript:;" class="imgBigModalCloseBtn" v-on:click="closeModal"><img src="./assets/images/close.png"></a>
          <a href="javascript:;" :href="data[index].links.download" class="imgBigDownloadBtn" target="_blank"><img src="./assets/images/download.svg"></a>
          <div class="imgBigModalContent">
            <a href="javascript:;" class="modalPrevImgBtn" v-on:click="modalPrevImg"><img src="./assets/images/left.png"></a>
            <span class="imgBigModalImgWrap">
              <img :src="data[index].urls.regular">
              <span class="authorName">
                Photo by <a target="_blank" v-bind:href="data[index].user.links.html">{{ data[index].user.name }}</a> 
                on <a target="_blank" href="https://unsplash.com/?utm_source=moreisee&utm_medium=referral">Unsplash</a>
              </span> 
            </span>
            <a href="javascript:;" class="modalNextImgBtn" v-on:click="modalNextImg"><img src="./assets/images/right.png"></a>   
          </div>
        </div>
        <button class="goTop" v-if="isVisible" @click="backToTop">
            <img src="./assets/images/up-arrow.svg">
        </button>
      </div>
      <div class="footer">
        <div class="copyright">Copyright 2019 Erdal Yenigul / <a href="http://www.erdalyenigul.com" target="_blank">www.erdalyenigul.com</a></div>
        <div class="socialBox">
          Contact : <a href="mailto:erdalyenigul@gmail.com" target="_blank">erdalyenigul@gmail.com</a>
        </div>
        <div class="developer">
          <p>Development with <a href="https://vuejs.org/" target="_blank">VueJs + Vue Router + Scss</a></p>
          <p>Photos by <a href="https://unsplash.com/" target="_blank">Unsplash</a></p>   
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import axios from "axios";
  import { bus } from './main';

  export default {
    data : function () {
      return {
        data : [],
        errorMsg : "",
        searchKey : "",
        randomBg : "",
        blocks : "",
        pageNumber : 1,
        totalPages : "",
        apiKey : "ad16b589b46599ef6324e8fa87e302ff86efd9cdc11a7912be306c26fef949b8",
        index : -1,
        isVisible: false, 
        blocksBus : ''  
      }
    },
    methods : {
      getPhotosApi : function() {
        var self = this;        
        axios.get('https://api.unsplash.com/search/photos?&query=' + self.searchKey + '&page=' + self.pageNumber + '&per_page=50&client_id=' + self.apiKey + '')
          .then(function(response) {
            self.data = response.data.results;                      
            self.blocks = Object.keys(self.data).length;
            self.totalPages = response.data.total_pages;  
            self.pageNumber = 1
        }, function(error) {
          self.errorMsg = error;
        });       
      },      
      imgIndex : function(index) {
        this.index = index;
      },
      closeModal : function() {
        this.index = -1;
      },
      modalNextImg : function() {
        this.index++;
      },
      modalPrevImg : function() {
        this.index--;
      },
      nextPage : function() {
        this.pageNumber++;  
        var self = this;  
        axios.get('https://api.unsplash.com/search/photos?&query=' + self.searchKey + '&page=' + self.pageNumber + '&per_page=30&client_id=' + self.apiKey + '')
          .then(function(response) {
            self.data = response.data.results;
            self.blocks = Object.keys(self.data).length;
        }, function(error) {
          self.errorMsg = error;
        });       
      },
      prevPage : function() {
        this.pageNumber = this.pageNumber - 1;
        var self = this;  
        axios.get('https://api.unsplash.com/search/photos?&query=' + self.searchKey + '&page=' + self.pageNumber + '&per_page=30&client_id=' + self.apiKey + '')
          .then(function(response) {
            self.data = response.data.results;
            self.blocks = Object.keys(self.data).length;
        }, function(error) {
          self.errorMsg = error;
        });   
      },
      initToTopButton: function() {
          $(document).bind('scroll', function() {
            if ($(document).scrollTop() > 100) {
              $('.goTop').addClass('isVisible');
              this.isVisible = true;
            } else {
              $('.goTop').removeClass('isVisible');
              this.isVisible = false;
            }
          }.bind(this));
        },
        backToTop: function() {
          $('html,body').stop().animate({
            scrollTop: 0
          }, 'slow', 'swing');
        }       
    },
    created() {
      this.$nextTick(function() {
          this.initToTopButton();
        });
    }   
  } 
</script>

</template>

<style lang="scss">
  @import "assets/css/main.scss";
</style>
