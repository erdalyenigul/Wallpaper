<template>
	<div class="firstRandomBg" v-if="!blocks">
    	<div class="welcomeText">
    		<span class="headline">Wallpaper of the moment</span>
    		<span class="description">A Huge collection of stunning HD wallpapers that you can find easily on this app, change your device's background with thousands of choices and categories of HD Wallpepers. <br><br> Just search and download. Its totally free!</span>
    	</div>
    </div>
</template>

<script>
	import { bus } from '../main';

	export default {
		data() {
			return {
				blocks : '',
				data : []
			}
		},
		mounted() {
			bus.$on('blocksStatus', (data) => {			
				this.blocks = data		
				console.log(this.blocks + 'blocksdeger')							
			});
			bus.$on('dataStatus', (data) => {
				this.data = data	
				console.log(this.data + 'datadeger')							
			});
		}
	}
</script>

<style lang="scss">
	.firstRandomBg{display:flex; align-items:center; justify-content:center; flex-wrap: wrap; margin:0 auto; width:70%;
		img{width:100%;}
		.welcomeText{float:left; width:100%; text-align: center;
			.headline{font-family: 'Ranga', cursive; font-size: 50px; margin-bottom: 20px; float:left; width:100%;}
			.description{font-size: 16px; line-height: 22px; float:left; width:100%;}
			.tryMyLuckBtnWrap{float:left; width:100%;
				.tryMyLuckBtn{background-color: #863b3b; color:#fff; padding:10px 30px; border:0; border-radius: 30px; font-size: 14px; outline: 0; cursor:pointer;}
			}
		}				
		.welcomeImg{float:left; width:50%;}
	}
</style>