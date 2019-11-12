<!-- 
 <template>
 	<view class="photo-box" >
 		<view class="photos">
 			<view class="bigItem">
 				<image class="bigImage" :src="headImage.url" @click="addPhotos(6)"></image>
 			</view>
 			<view v-if="imageList[0]" class="item" :style="'left:'+position.x1+'rpx;top:'+position.y1+'rpx;z-index:'+(moveDom=='userImage-box1'?'10000;':'9998;')" @touchmove.stop.prevent="onChange" id="userImage-box1" @touchstart="onTouchStart" @touchend="onTouchEnd">
 				<image  mode="center" :src="imageList[0].mediaurl" class="userImage" @click="addPhotos(1)"></image>
 			</view>
 			<view class="item addItem" v-else :style="'left:'+position.x1+'rpx;top:'+position.y1+'rpx;'">
 				<image class='addImage-icon' src="../../../static/commonIcon/add-icon.png" @click="addPhotos(0)" mode=""></image>
 			</view>
 			<view v-if="imageList[1]" class="item" :style="'left:'+position.x2+'rpx;top:'+position.y2+'rpx;z-index:'+(moveDom=='userImage-box2'?'10000;':'9998;')" @touchmove.stop.prevent="onChange" id="userImage-box2" @touchstart="onTouchStart" @touchend="onTouchEnd">
 				<image  mode="center" :src="imageList[1].mediaurl" class="userImage" @click="addPhotos(2)"></image>
 			</view>
 			<view class="item addItem" v-else :style="'left:'+position.x2+'rpx;top:'+position.y2+'rpx;'">
 				<image class='addImage-icon' src="../../../static/commonIcon/add-icon.png" @click="addPhotos(0)" mode=""></image>
 			</view>
 			
 			<view v-if="imageList[2]" class="item" :style="'left:'+position.x3+'rpx;top:'+position.y3+'rpx;z-index:'+(moveDom=='userImage-box3'?'10000;':'9998;')" @touchmove.stop.prevent="onChange" id="userImage-box3" @touchstart="onTouchStart" @touchend="onTouchEnd">
 				<image  mode="center" :src="imageList[2].mediaurl" class="userImage" @click="addPhotos(3)"></image>
 			</view>
 			<view class="item addItem" v-else :style="'left:'+position.x3+'rpx;top:'+position.y3+'rpx;'">
 				<image class='addImage-icon' src="../../../static/commonIcon/add-icon.png" @click="addPhotos(0)" mode=""></image>
 			</view>
 			<view v-if="imageList[3]" class="item" :style="'left:'+position.x4+'rpx;top:'+position.y4+'rpx;z-index:'+(moveDom=='userImage-box4'?'10000;':'9998;')" @touchmove.stop.prevent="onChange" id="userImage-box4" @touchstart="onTouchStart" @touchend="onTouchEnd">
 				<image  mode="center" :src="imageList[3].mediaurl" class="userImage" @click="addPhotos(4)"></image>
 			</view>
 			<view class="item addItem" v-else :style="'left:'+position.x4+'rpx;top:'+position.y4+'rpx;'">
 				<image class='addImage-icon' src="../../../static/commonIcon/add-icon.png" @click="addPhotos(0)" mode=""></image>
 			</view>
 			<view v-if="imageList[4]" class="item" :style="'left:'+position.x5+'rpx;top:'+position.y5+'rpx;z-index:'+(moveDom=='userImage-box5'?'10000;':'9998;')" @touchmove.stop.prevent="onChange" id="userImage-box5" @touchstart="onTouchStart" @touchend="onTouchEnd">
 				<image  mode="center" :src="imageList[4].mediaurl" class="userImage" @click="addPhotos(5)"></image>
 			</view>
 			<view class="item addItem" v-else :style="'left:'+position.x5+'rpx;top:'+position.y5+'rpx;'">
 				<image  mode="center" class='addImage-icon' src="../../../static/commonIcon/add-icon.png" @click="addPhotos(0)"></image>
 			</view>
 		</view>
 		<view class="text">
 			第一张图会成为您的头像哦，上传更多的图片
 		</view>
 		<changePhoto @funcPhoto="getMsgPhoto" ref="changeAvatar"  @imageUrl="getImageUrl" :imageLength='imageLength' :showDelete='showDeleteImage' @deleteImage='deleteImage'></changePhoto>
 	</view>
 	
 </template>
 
 <script>
 	import changePhoto from '@/components/changePhoto.vue';
 	import api from '@/common/api.js';
 	import { mapState } from 'vuex';
	let onePoint = {
	  x: 0,
	  y: 0
	}
 	export default{
 		components:{
 			changePhoto,
 			
 		},
 		computed: {
 			...mapState({
 				myInfo(state) {
 					return state.user.myInfo;
 				}
 			})
 		},
		props:['avatar','mediaList'],
 		data(){
 			return{
				showDeleteImage:false,
				moveIng:false,
				headImage:{
					mediaid:'',
					url:''
				},
				imageLength:0,//头图数量
				moveDom:'userImage-box1',
 				position:{	
 					y1:0,
 					x1:462,
 					y2:231,
 					x2:462,
 					y3:462,
 					x3:462,	
 					y4:462,
 					x4:231,
 					y5:462,
 					x5:0,
 				},
				imageIndex:0,
 				// imageList:['http://iph.href.lu/150x150','http://iph.href.lu/160x160','http://iph.href.lu/170x170','http://iph.href.lu/180x180','http://iph.href.lu/180x180','http://iph.href.lu/180x180']
 				imageList:[],
 				userInfo:undefined,
 			}
 		},
 		methods:{
 			onChange:function(e){
				var that = this
				 if (e.touches.length < 2) {
				 var onePointDiffX = e.touches[0].pageX * 2 - onePoint.x
				 var onePointDiffY = e.touches[0].pageY * 2 - onePoint.y
					if(e.currentTarget.id == 'userImage-box1'){
						if(that.position.x1 + onePointDiffX > 462){
							that.position.x1 = 462
						}else if(that.position.x1 + onePointDiffX < 0){
							that.position.x1 = 0
						}else{
							that.position.x1 = that.position.x1 + onePointDiffX;
						}
						if(that.position.y1 + onePointDiffY > 462){
							that.position.y1 = 462
						}else if(that.position.y1 + onePointDiffX < 0){
							that.position.y1 = 0
						}else{
							that.position.y1 = that.position.y1 + onePointDiffY;
						}

					}
					if(e.currentTarget.id == 'userImage-box2'){
						if(that.position.x2 + onePointDiffX > 462){
							that.position.x2 = 462
						}else if(that.position.x2 + onePointDiffX < 0){
							that.position.x2 = 0
						}else{
							that.position.x2 = that.position.x2 + onePointDiffX;
						}
						if(that.position.y2 + onePointDiffY > 462){
							that.position.y2 = 462
						}else if(that.position.y2 + onePointDiffX < 0){
							that.position.y2 = 0
						}else{
							that.position.y2 = that.position.y2 + onePointDiffY;
						}
					}
					if(e.currentTarget.id == 'userImage-box3'){
						if(that.position.x3 + onePointDiffX > 462){
							that.position.x3 = 462
						}else if(that.position.x3 + onePointDiffX < 0){
							that.position.x3 = 0
						}else{
							that.position.x3 = that.position.x3 + onePointDiffX;
						}
						if(that.position.y3 + onePointDiffY > 462){
							that.position.y3 = 462
						}else if(that.position.y3 + onePointDiffX < 0){
							that.position.y3 = 0
						}else{
							that.position.y3 = that.position.y3 + onePointDiffY;
						}
					}
					if(e.currentTarget.id == 'userImage-box4'){
						if(that.position.x4 + onePointDiffX > 462){
							that.position.x4 = 462
						}else if(that.position.x4 + onePointDiffX < 0){
							that.position.x4 = 0
						}else{
							that.position.x4 = that.position.x4 + onePointDiffX;
						}
						if(that.position.y4 + onePointDiffY > 462){
							that.position.y4 = 462
						}else if(that.position.y4 + onePointDiffX < 0){
							that.position.y4 = 0
						}else{
							that.position.y4 = that.position.y4 + onePointDiffY;
						}
					}
					if(e.currentTarget.id == 'userImage-box5'){
						if(that.position.x5 + onePointDiffX > 462){
							that.position.x5 = 462
						}else if(that.position.x5 + onePointDiffX < 0){
							that.position.x5 = 0
						}else{
							that.position.x5 = that.position.x5 + onePointDiffX;
						}
						if(that.position.y5 + onePointDiffY > 462){
							that.position.y5 = 462
						}else if(that.position.y5 + onePointDiffX < 0){
							that.position.y5 = 0
						}else{
							that.position.y5 = that.position.y5 + onePointDiffY;
						}
					}
				    onePoint.x = e.touches[0].pageX * 2
				    onePoint.y = e.touches[0].pageY * 2
				 }
 			},
			deleteImage(){
				let that = this;	
					if(this.imageList[this.imageIndex-1].id){
						let data = {
							id:this.imageList[this.imageIndex-1].id
						}
						console.log(this.imageList[this.imageIndex-1].id)
						api.microsite.headImage.deleteMediaById(data).then(res => {
							that.$emit('update',that.headImage.url,that.imageList)
							
							this.imageList.splice(this.imageIndex-1,1);
						})
					}else{
						this.imageList.splice(this.imageIndex-1,1);
					}
				
			},
			onTouchStart(e){
				let that = this;
				let id = e.currentTarget.id;
				this.moveDom = id;
				 onePoint.x = e.touches[0].pageX * 2
				 onePoint.y = e.touches[0].pageY * 2
			},
 			onTouchEnd(e){
				let that = this;
				function change(value){
					let itemImageUrl = that.imageList[value].mediaurl;
					let itemImageId = that.imageList[value].mediaid;
					let headImageUrl = that.headImage.url;
					let headImageId = that.headImage.mediaid;
					that.imageList[value].mediaurl = headImageUrl;
					that.imageList[value].mediaid = headImageId;
					that.headImage.url = itemImageUrl;
					that.headImage.mediaid = itemImageId
					that.$emit('update',that.headImage.url,that.imageList)
				}
				if(e.currentTarget.id == 'userImage-box1'){
					if(that.position.x1 < 345 && that.position.y1 < 345){
						change(0)
					}
					that.position.x1 = 462;
					that.position.y1 = 0;
				}
				if(e.currentTarget.id == 'userImage-box2'){
					if(that.position.x2 < 345 && that.position.y2 < 345){
						change(1)
					}
					that.position.x2 = 462;
					that.position.y2 = 231;
				}
				if(e.currentTarget.id == 'userImage-box3'){
					if(that.position.x3 < 345 && that.position.y3 < 345){
						change(2)
					}
					that.position.x3 = 462;
					that.position.y3 = 462;
				}
				if(e.currentTarget.id == 'userImage-box4'){
					if(that.position.x4 < 345 && that.position.y4 < 345){
						change(3)
					}
					that.position.x4 = 231;
					that.position.y4 = 462;
				}
				if(e.currentTarget.id == 'userImage-box5'){
					if(that.position.x5 < 345 && that.position.y5 < 345){
						change(4)
					}
					that.position.x5 = 0;
					that.position.y5 = 462;
				}
 			},
 			addPhotos(index){
 				//添加图片
				console.log(index)
				this.imageIndex = index;
				//0是添加图片
				if((index !== 0) && (index !== 6)){
					this.showDeleteImage = true
				}else{
					this.showDeleteImage = false
				}
 				this.$refs.changeAvatar.photoStu();
 			},
 			getImageUrl(value){
 				//获取要上传图片的base64地址
				wx.showLoading()
				let that = this;
				value.forEach( (item,index) => {
					let data = {
						imageBase64Content : item
					}
					api.microsite.headImage.upload(data).then(res => {
						console.log(res)
						if(that.imageIndex === 0){
							//0是添加图片
							let data = {
								mediaid:res[0],
								mediaurl:res[1],
								orderby:that.imageList.length+1
							}
							that.imageList.push(data)	
						}else if(that.imageIndex === 6){
							that.headImage.url = res[1];
							that.headImage.mediaid = res[0];
						}else{
							that.imageList[that.imageIndex - 1].mediaid = res[0];
							that.imageList[that.imageIndex - 1].mediaurl = res[1];
						}
										
					}).then(() => {
						that.$emit('update',that.headImage.url,that.imageList)
					})
				})
 				setTimeout(function () {
 				  wx.hideLoading()
 				}, 1000)
 				
 				
 			},
 			getUserInfo(){
 				let that = this;
 				let data = {
 					micrositeid:this.myInfo.micrositeid,
 					userid:this.myInfo.userid
 				}
 				api.microsite.info.byId(data).then(res => {
 					console.log(res,'info')
 					that.userInfo = res;
 				})
 			}
 			
 		},
 		created(){
 			this.getUserInfo();
 		},
		watch:{
			avatar:function(n,o){
				this.headImage.url = n
			},
			mediaList:function(n,o){
				console.log(n,n.length,'mediaList')
				this.imageList = n;
				this.imageLength = n.length;
			}
		}
		
 	}
 </script>
 
 <style lang="scss">
 	.photo-box{
 		.text{
 			padding: 40rpx 0rpx;
 			font-size: 24rpx;
 			text-align: center;
 			line-height: 34rpx;
 			color:rgba(0,0,0,0.25);
 		}
 	}
 	.photos{
 		width:689rpx;
 		height: 689rpx;
 		position: relative;
 		.bigItem{
 			width: 459rpx;
 			height: 459rpx;
 			border-radius: 10rpx;
 			position: absolute;
 			top:0;
 			left: 0;
 			image{
 				width: 459rpx;
 				height: 459rpx;
 				border-radius: 10rpx;
 			}
 		}
 		.item{
 			width: 228rpx;
 			height: 228rpx;
 			border-radius: 10rpx;
 			background-color: #F2F2F2;
 			display: flex;
 			justify-content: center;
 			align-items: center;
 			position: absolute;
 			z-index: 9997;
 			.userImage{
 				width: 228rpx;
 				height: 228rpx;
 				border-radius: 10rpx;
 				z-index: 9998;
 			}
 			.addImage-icon{
 				width: 64rpx;
 				height:64rpx;
 			}
 		}
 		.addItem{
 			z-index: 0;
 		}
 	}
 </style>
 -->
 
 
 
 
 <!-- <template>
 	<view class="photo-box" >
 		<view class="photos">
 			<view class="bigItem">
 				<image class="bigImage" :src="headImage.url" @click="addPhotos(6)"></image>
 			</view>
 			<view v-if="imageList[0]" class="item" :style="'left:'+position.x1+'rpx;top:'+position.y1+'rpx;z-index:'+(moveDom=='userImage-box1'?'10000;':'9998;')">
 				<image  mode="aspectFill" :src="imageList[0].mediaurl" class="userImage" @click="addPhotos(1)"></image>
 			</view>
 			<view class="item addItem" v-else :style="'left:'+position.x1+'rpx;top:'+position.y1+'rpx;'">
 				<image class='addImage-icon' :src="`${imgUrl}images/myqs-images/commonIcon/add-icon.png`" @click="addPhotos(0)" mode=""></image>
 			</view>
 			<view v-if="imageList[1]" class="item" :style="'left:'+position.x2+'rpx;top:'+position.y2+'rpx;z-index:'+(moveDom=='userImage-box2'?'10000;':'9998;')">
 				<image  mode="aspectFill" :src="imageList[1].mediaurl" class="userImage" @click="addPhotos(2)"></image>
 			</view>
 			<view class="item addItem" v-else :style="'left:'+position.x2+'rpx;top:'+position.y2+'rpx;'">
 				<image class='addImage-icon' :src="`${imgUrl}images/myqs-images/commonIcon/add-icon.png`" @click="addPhotos(0)" mode=""></image>
 			</view>
 			
 			<view v-if="imageList[2]" class="item" :style="'left:'+position.x3+'rpx;top:'+position.y3+'rpx;z-index:'+(moveDom=='userImage-box3'?'10000;':'9998;')">
 				<image  mode="aspectFill" :src="imageList[2].mediaurl" class="userImage" @click="addPhotos(3)"></image>
 			</view>
 			<view class="item addItem" v-else :style="'left:'+position.x3+'rpx;top:'+position.y3+'rpx;'">
 				<image class='addImage-icon' :src="`${imgUrl}images/myqs-images/commonIcon/add-icon.png`" @click="addPhotos(0)" mode=""></image>
 			</view>
 			<view v-if="imageList[3]" class="item" :style="'left:'+position.x4+'rpx;top:'+position.y4+'rpx;z-index:'+(moveDom=='userImage-box4'?'10000;':'9998;')">
 				<image  mode="aspectFill" :src="imageList[3].mediaurl" class="userImage" @click="addPhotos(4)"></image>
 			</view>
 			<view class="item addItem" v-else :style="'left:'+position.x4+'rpx;top:'+position.y4+'rpx;'">
 				<image class='addImage-icon' :src="`${imgUrl}images/myqs-images/commonIcon/add-icon.png`" @click="addPhotos(0)" mode=""></image>
 			</view>
 			<view v-if="imageList[4]" class="item" :style="'left:'+position.x5+'rpx;top:'+position.y5+'rpx;z-index:'+(moveDom=='userImage-box5'?'10000;':'9998;')">
 				<image  mode="aspectFill" :src="imageList[4].mediaurl" class="userImage" @click="addPhotos(5)"></image>
 			</view>
 			<view class="item addItem" v-else :style="'left:'+position.x5+'rpx;top:'+position.y5+'rpx;'">
 				<image class='addImage-icon' :src="`${imgUrl}images/myqs-images/commonIcon/add-icon.png`" @click="addPhotos(0)"></image>
 			</view>
 		</view>
 		<view class="text">
 			第一张图会成为您的头像哦，上传更多的图片
 		</view>
 		<changePhoto @funcPhoto="getMsgPhoto" ref="changeAvatar"  @imageUrl="getImageUrl" :showDelete='showDeleteImage' @deleteImage='deleteImage'></changePhoto>
		<image-cropper :src="tempFilePath" @confirm="confirm" @cancel="cancel" :show-reset-btn="false" :show-rotate-btn="false"></image-cropper>
 	</view>
 	
 </template>
 
 <script>
 	import changePhoto from '@/components/changePhoto.vue';
	import ImageCropper  from '@/components/invinbg-image-cropper/invinbg-image-cropper.vue'
 	import api from '@/common/api.js';
 	import { mapState } from 'vuex';
 	let onePoint = {
 	  x: 0,
 	  y: 0
 	}
 	export default{
 		components:{
 			changePhoto,
			ImageCropper
 			
 		},
 		computed: {
 			...mapState({
 				myInfo(state) {
 					return state.user.myInfo;
 				}
 			})
 		},
 		props:['avatar','mediaList'],
 		data(){
 			return{
				imgUrl:this.COS_BASE_URI,
				tempFilePath :'',//裁剪前url
				cropFilePath:'',//裁剪后url
 				showDeleteImage:false,
 				moveIng:false,
 				headImage:{
 					mediaid:'',
 					url:''
 				},
 				imageLength:0,//头图数量
 				moveDom:'userImage-box1',
 				position:{	
 					y1:0,
 					x1:462,
 					y2:231,
 					x2:462,
 					y3:462,
 					x3:462,	
 					y4:462,
 					x4:231,
 					y5:462,
 					x5:0,
 				},
 				imageIndex:0,
 				// imageList:['http://iph.href.lu/150x150','http://iph.href.lu/160x160','http://iph.href.lu/170x170','http://iph.href.lu/180x180','http://iph.href.lu/180x180','http://iph.href.lu/180x180']
 				imageList:[],
 				userInfo:undefined,
 			}
 		},
 		methods:{
 			deleteImage(){
 				let that = this;	
 					if(this.imageList[this.imageIndex-1].id){
 						let data = {
 							id:this.imageList[this.imageIndex-1].id
 						}
 						console.log(this.imageList[this.imageIndex-1].id)
 						api.microsite.headImage.deleteMediaById(data).then(res => {
 							that.$emit('update',that.headImage.url,that.imageList)
 							
 							this.imageList.splice(this.imageIndex-1,1);
 						})
 					}else{
 						this.imageList.splice(this.imageIndex-1,1);
 					}
 				
 			},
 			addPhotos(index){
 				//添加图片
 				console.log(index)
 				this.imageIndex = index;
 				//0是添加图片
 				if((index !== 0) && (index !== 6)){
 					this.showDeleteImage = true
 				}else{
 					this.showDeleteImage = false
 				}
 				this.$refs.changeAvatar.photoStu();
 			},
 			getImageUrl(value){
 				//获取要上传图片的base64地址
 				wx.showLoading()
 				let that = this;
				console.log(value,'裁剪前地址')
				this.tempFilePath = value			
 			},
			confirm(value){
				//裁剪图片确认回调
				let that = this;
				console.log(value.detail.tempFilePath,'裁剪后')
				this.tempFilePath = ''
				let url = value.detail.tempFilePath;
				const base64 = 'data:image/jpg;base64,' + wx.getFileSystemManager().readFileSync(url, "base64");
				let data = {
					imageBase64Content : base64
				}
				api.microsite.headImage.upload(data).then(res => {
					console.log(res,'上传图片')
					if(that.imageIndex === 0){
						//0是添加图片
						let data = {
							mediaid:res[0],
							mediaurl:res[1],
							orderby:that.imageList.length+1
						}
						that.imageList.push(data)	
					}else if(that.imageIndex === 6){
						that.headImage.url = res[1];
						that.headImage.mediaid = res[0];
					}else{
						that.imageList[that.imageIndex - 1].mediaid = res[0];
						that.imageList[that.imageIndex - 1].mediaurl = res[1];
					}
									
				}).then(() => {
					that.$emit('update',that.headImage.url,that.imageList)
				})
			},
			cancel(){
				//裁剪图片取消
				this.tempFilePath = ''
				console.log('取消')
			},
 			getUserInfo(){
 				let that = this;
 				let data = {
 					micrositeid:this.myInfo.micrositeid,
 					userid:this.myInfo.userid
 				}
 				api.microsite.info.byId(data).then(res => {
 					console.log(res,'info')
 					that.userInfo = res;
 				})
 			}
 			
 		},
 		created(){
 			this.getUserInfo();
 		},
 		watch:{
 			avatar:function(n,o){
 				this.headImage.url = n
 			},
 			mediaList:function(n,o){
 				console.log(n,n.length,'mediaList')
 				this.imageList = n;
 				this.imageLength = n.length;
 			}
 		}
 		
 	}
 </script>
 
 <style lang="scss">
 	.photo-box{
 		.text{
 			padding: 40rpx 0rpx;
 			font-size: 24rpx;
 			text-align: center;
 			line-height: 34rpx;
 			color:rgba(0,0,0,0.25);
 		}
 	}
 	.photos{
 		width:689rpx;
 		height: 689rpx;
 		position: relative;
 		.bigItem{
 			width: 459rpx;
 			height: 459rpx;
 			border-radius: 10rpx;
 			position: absolute;
 			top:0;
 			left: 0;
 			image{
 				width: 459rpx;
 				height: 459rpx;
 				border-radius: 10rpx;
 			}
 		}
 		.item{
 			width: 228rpx;
 			height: 228rpx;
 			border-radius: 10rpx;
 			background-color: #F2F2F2;
 			display: flex;
 			justify-content: center;
 			align-items: center;
 			position: absolute;
 			z-index: 9997;
 			.userImage{
 				width: 228rpx;
 				height: 228rpx;
 				border-radius: 10rpx;
 				z-index: 9998;
 			}
 			.addImage-icon{
 				width: 64rpx;
 				height:64rpx;
 			}
 		}
 		.addItem{
 			z-index: 0;
 		}
 	}
 </style> -->
 
 