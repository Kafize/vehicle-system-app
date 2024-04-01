<template>
	<view>
		<view>
			<u-navbar title="应用列表" leftIcon="" :placeholder="true">
			</u-navbar>
		</view>
		<view  class="container">
			<u-picker :show="show" ref="uPicker" :columns="columns" @confirm="confirm" keyName="label" @change="changeHandler"></u-picker>
			<view>
				<view class="list-item">
					<view>
						<u-row>
							<u-col span="6" align="start">
								<u-icon
								@click="show=true" 
								    name="arrow-down"
								    size="19"
									:label="wheather.cityName"
									labelPos="left"
								></u-icon>
							</u-col>
							<u-col span="6" >
								<u--text :text="todayDate" align="right"></u--text>
							</u-col>
						</u-row>
					</view>
					<u-divider></u-divider>
					<view>
						<u-row justify="space-between" align="center">
							<u-col span="6" align="center">
								<view class="flex">
									<view >
										<u-icon :name="$url+wheather.weatherIcon" :label="wheather.weather"  size="24" ></u-icon>
									</view>
								</view>
								<u--text :text="wheather.minTemperature+'℃~'+wheather.maxTemperature+'℃'" size="20" align="center"></u--text>
							</u-col>
							<u-col span="6" align="center">
								<u--text :text="wheather.currentTemperature+'℃'" size="28" align="center"></u--text>
							</u-col>
						</u-row>
						<view>
							<br>
							<view>
								<u-row justify="space-between">
									<u-col span="3"  align="center">
										<u--text text="湿度" align="center" :bold="true"></u--text>
										<text>{{wheather.humidity}}</text>
									</u-col>
									<u-col span="3"  align="center">
										<u--text text="风速" align="center" :bold="true"></u--text>
										<text>{{wheather.windSpeed}} <text style="font-size: 12px;">km/h</text></text>
									</u-col>
									<u-col span="3"  align="center">
										<u--text text="能见度" align="center" :bold="true"></u--text>
										<text>{{wheather.visibility}} <text style="font-size: 12px;">%</text></text>
									</u-col>
									<u-col span="3"  align="center">
										<u--text text="空气质量" align="center" :bold="true"></u--text>
										<text>{{wheather.airQuality}}  <text style="font-size: 12px;">m</text></text>
									</u-col>
								</u-row>
							</view>
						</view>
					</view>					
				</view>
			</view>
			<view>
				<view v-for="item in appList" class="list-item">
					<view>
						<u-row>
							<u-col span="2" style="position: relative;">
								<u-badge type="error" :isDot="true" :absolute="true" style="z-index: 1;left: 40px;" :show="!!item.isUpdated"></u-badge>
								<u-icon :name="$url+item.appIcon" size="42"></u-icon>
							</u-col>
							<u-col  span="10">
								<u-row justify="space-between" align="center" >
									<u-col span="6" >
										<view class="flex" style="justify-content: start;">
											<text style="padding-right: .25rem;">{{item.appName}}</text>
											<u-badge type="error" value="推荐"  :show="!!item.isRecommended"></u-badge>
										</view>
										<view>
											<u--text :text="item.appVersion" color="#909399" size="14"></u--text>
										</view>
									</u-col>
									<u-col span="6" align="flex-end" >
										<view >
											<u-tag text="立即下载"   type="primary" size="mini" @click="downFileApp(item.appUrl),addHis(item.id)" />
										</view>
									</u-col>
								</u-row>
							</u-col>
						</u-row>
					</view>
					<u-divider></u-divider>
					<view>
						<u-row>
							<view>
								<u--text :text="item.appDescription" :lines="2"></u--text>
							</view>
						</u-row>
					</view>
					<br>
					<view>
						<view>
							<image :src="$url+item.appBackground" mode="widthFix" height="auto" style="width: 100%;"></image>
						</view>
					</view>
				</view>
				<view>
					<u-popup :show="popupshow" @close="popupshow=true" mode="center" zIndex="100" :closeOnClickOverlay="true" @open="popupshow=true" round="10"  >
						<view >
							<u-line-progress :percentage="progresValue" :closeable="true" style="width: 100%;" activeColor="#3c9cff"></u-line-progress>
						</view>
						<view style="">
							<u--text text="我用十年青春,赴你最后之约"></u--text>
						</view>
					</u-popup>
				</view>
			</view>
			<view>
				<view>
					<view class="">
						<view v-for="i in themeApplist" class="list-item">
							<view class="title-tag">
								<text>{{i.topicName}}</text>
							</view>
							<u-divider></u-divider>
							<view v-for="item in i.appList">
								<u-row>
									<u-col span="2" style="position: relative;">
										<u-badge type="error" :isDot="true" :absolute="true" style="z-index: 1;left: 40px;" :show="!!item.isUpdated"></u-badge>
										<u-icon :name="$url+item.appIcon" size="42"></u-icon>
									</u-col>
									<u-col  span="10">
										<u-row justify="space-between" align="center" >
											<u-col span="6" >
												<view class="flex" style="justify-content: start;">
													<text style="padding-right: .25rem;">{{item.appName}}</text>
													<u-badge type="error" value="推荐"  :show="!!item.isRecommended"></u-badge>
												</view>
												<view>
													<u--text :text="item.appVersion" color="#909399" size="14"></u--text>
												</view>
											</u-col>
											<u-col span="6" align="flex-end" >
												<view >
													<u-tag text="立即下载"   type="primary" size="mini" @click="downFileApp(item.appUrl)" />
												</view>
											</u-col>
										</u-row>
									</u-col>
								</u-row>
								<view>
									<u-row>
										<view>
											<u--text :text="item.appDescription" :lines="2"></u--text>
										</view>
									</u-row>
								</view>
								<br>
								<view>
									<view>
										<image :src="$url+item.appBackground" mode="widthFix" height="auto" style="width: 100%;"></image>
									</view>
								</view>
								<u-divider></u-divider>
							</view>
							
						</view>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				appList:[],
				themeApplist:[],
				show:false,
				wheather:[],
				city:[],
				columns: [],
				columnData: [],
				popupshow:false,
				progresValue:0
			}
		},
		onLoad() {
			this.getAppList();
			this.gethemeAppList()
			this.getWeather(13);
			this.getCity();
		},
		computed:{
			todayDate(){
				let date = new Date();
				let year = date.getFullYear();
				let month = date.getMonth() + 1; // 注意月份是从0开始计数的，所以需要+1
				let day = date.getDate();
				let formattedDate = `${year}-${month < 10 ? '0' + month : month}-${day < 10 ? '0' + day : day}`;
				return formattedDate;
			}
		},
		methods: {
			getAppList(){
				this.$request.get('/control/app/list')
				.then(res=>{
					if(res.code==200){
						this.appList=res.rows;
					}else{
						uni.showToast({
							title: res.msg,
							icon:'error'
						});
					}
				})
				.catch(err=>{
					console.log(err);
				})
			},
			gethemeAppList(){
				this.$request.get('/control/topic/list')
				.then(res=>{
					if(res.code==200){
						this.themeApplist=res.rows;
						
					}else{
						this.$request.requestShowToastError(res.msg);
					}
				})
				.catch(err=>{
					this.$request.requestShowToastError(err);
				})
			},
			getWeather(cityId){
				this.$request.get(`/control/weather/list?cityId=${cityId}`)
				.then(res=>{
					if(res.code==200){
						this.wheather=res.rows[0]||this.wheather;
					}else{
						this.$request.requestShowToastError(res.msg);
					}
				})
				.catch(err=>{
					this.$request.requestShowToastError(err);
				})
			},
			getCity(){
				this.$request.get('/control/city/list')
				.then(res=>{
					if(res.code==200){
						this.city=res.rows||this.city;
						this.columns=[],this.columnData=[];
						this.columns.push(this.city.map(o=>o.cityName))
						this.columns.push(this.city[0].children.map(o=>o.cityName))
						this.columnData=
							this.city.map(o=>{
								return o.children.map(i=>{
									return {label:i.cityName,id:i.id}
								})
							})
						
						
					}else{
						this.$request.requestShowToastError(res.msg);
					}
				})
				.catch(err=>{
					this.$request.requestShowToastError(err);
				})
			},
			changeHandler(e) {
				const {
					columnIndex,
					value,
					values,
					index,
					
					picker = this.$refs.uPicker
				} = e
				if (columnIndex === 0) {
					picker.setColumnValues(1, this.columnData[index])
				}
			},		
			confirm(e) {
				this.getWeather(this.columnData[e.indexs[0]][e.indexs[1]].id)
				this.show = false
			},
			downFileApp(url){
				console.log(url)
				this.popupshow=true;
				uni.downloadFile({
				  url: this.$url+url||'', // 文件的远程地址
				  success: function(res) {
				    if (res.statusCode === 200) {
				      const tempFilePath = res.tempFilePath; // 下载的临时文件路径
				      console.log('下载成功，临时文件路径：' + tempFilePath);
					  this.popupshow=true;
					  
				      // 执行其他操作，比如保存文件或展示给用户等
				    }
				  },
				  fail: function(err) {
					this.popupshow=true;
					this.progresValue=0;
					this.$request.requestShowToastError(err);
					
				  }
				})
				.onProgressUpdate(res=>{
					this.progresValue=res.progress
				})
			},
			addHis(appId){
				this.$request.post('/control/record',{'appId':appId})
				.then(res=>{
					if(res.code==200){
						this.$request.requestShowToastError(res.msg);
					}
				})
			}
		}
	}
</script>

<style lang="scss">
	.container{
		padding: 1rem;
		background-color: $u-bg-color;
		.list-item{
			padding: 1rem;
			margin-bottom: 1rem ;
			border-radius: 15rpx;
			background-color: #fff;
		}
		.flex{
			display: flex;
			align-items: center;
			
		}
		.title-tag{
			padding-left: .5rem;
			border-left: solid red 4px;
		}
	}
	
</style>
