<template>
	<view class="container">
		<u-navbar title="车辆" leftIcon="" :placeholder="true">
			<view @click="godebug"
			                class="u-nav-slot"
			                slot="right"
			            >
			                <u-icon
			                    name="grid"
			                    size="20"
			                ></u-icon>
			            </view>
		</u-navbar>
		
		<view>
			<view v-for="o in carList" class="list-item" @click="goCarDetail(o.id)">
				<view class="flex">
					<u--image :showLoading="true"  :src="$url+o.image"  radius="15" :lazy-load="true"  mode="widthFix" ></u--image>
				</view>
				<view class="flex">
					<text >{{o.licencePlate}}</text>
					<text style="padding: 0 1rem;">{{o.ownerDriver+'的'+o.vehicleBrand+' · '+o.vehicleModel}}</text>
					<view>
						<u-tag :text="o.carConfiguration" size="mini" type="primary"></u-tag>
					</view>
				</view>
				<u-divider></u-divider>
				<view>
					<view>
						<u-row gutter="10">
							<u-col span="3"  align="center">
								<u--text text="电量" align="center" :bold="true"></u--text>
								<text style="margin: .5rem 0;">{{o.batteryHealth}}<text style="font-size: 12px;">%</text></text>
							</u-col>
							<u-col span="3"  align="center">
								<u--text text="里程数" align="center" :bold="true"></u--text>
								<text style="margin: .5rem 0;">{{o.kilometresTravelled}}<text style="font-size: 12px;">km</text></text>
								
							</u-col>
							<u-col span="3"  align="center">
								<u--text text="剩余里程" align="center" :bold="true"></u--text>
								<text style="margin: .5rem 0;">{{o.remainingMileage}}<text style="font-size: 12px;">km</text></text>
								
							</u-col>
							<u-col span="3"  align="center">
								<u--text text="行驶次数" align="center" :bold="true"></u--text>
								<text style="margin: .5rem 0;">{{o.numberOfTrips}}<text style="font-size: 12px;">次</text></text>
								
							</u-col>
						</u-row>
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
				carList:[]
			}
		},
		onLoad() {
			this.getCarList();
		},
		methods: {
			getCarList(){
				this.$request.get('/api/recharge/vehicle/list')
				.then(res=>{
					if(res.code==200){
						this.carList=res.rows;
					}else{
						this.$request.requestShowToastError(res.msg);
					}
				})
				.catch(err=>{
					this.$reques.requestShowToastError(err);
				})
			},
			goCarDetail(id){
				uni.navigateTo({
					url: '/pages/cardetail/cardetail?id='+id,
					success: res => {},
					fail: () => {},
					complete: () => {}
				});
			},
			godebug(){
				uni.navigateTo({
					url:'/pages/debug/debug'
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
			justify-content: center;
		}
		.title-tag{
			padding-left: .5rem;
			border-left: solid red 4px;
		}
	}
</style>
