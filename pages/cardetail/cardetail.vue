<template>
	
	<view class="container">
		<u-navbar title="车辆详细"  :placeholder="true" :autoBack="true">
			
		</u-navbar>
		<view  class="list-item">
			<view class="flex">
				<u--image :showLoading="true"  :src="$url+data.image"  radius="15" :lazy-load="true"  mode="widthFix" ></u--image>
			</view>
			<view class="flex">
				<text >{{data.licencePlate}}</text>
				<text style="padding: 0 1rem;">{{data.ownerDriver+'的'+data.vehicleBrand+' · '+data.vehicleModel}}</text>
				<view>
					<u-tag :text="data.carConfiguration" size="mini" type="primary"></u-tag>
				</view>
			</view>
			<u-divider></u-divider>
			<view>
				<view>
					<u-row gutter="10">
						<u-col span="3"  align="center">
							<u--text text="电量" align="center" :bold="true"></u--text>
							<text style="margin: .5rem 0;">{{data.batteryHealth}}<text style="font-size: 12px;">%</text></text>
						</u-col>
						<u-col span="3"  align="center">
							<u--text text="里程数" align="center" :bold="true"></u--text>
							<text style="margin: .5rem 0;">{{data.kilometresTravelled}}<text style="font-size: 12px;">km</text></text>
							
						</u-col>
						<u-col span="3"  align="center">
							<u--text text="剩余里程" align="center" :bold="true"></u--text>
							<text style="margin: .5rem 0;">{{data.remainingMileage}}<text style="font-size: 12px;">km</text></text>
							
						</u-col>
						<u-col span="3"  align="center">
							<u--text text="行驶次数" align="center" :bold="true"></u--text>
							<text style="margin: .5rem 0;">{{data.numberOfTrips}}<text style="font-size: 12px;">次</text></text>
						</u-col>
					</u-row>
				</view>
			</view>
			<view>
				<view class="title-tag" style="margin: 1rem 0;"><text>配置信息</text></view>
				<u-row  >
					<u-col span="6">
						<u--text :text="'下次保养公里:'+data.remainingKilometers+'km'" ></u--text>
					</u-col>
					<u-col span="6" >
						<u--text :text="'电池健康:'+data.batteryHealth"></u--text>
					</u-col>
				</u-row>
				<u-row justify="center"  >
					<u-col span="6">
						<u--text :text="'剩余电量:'+data.dumpEnergy+'%'"></u--text>
					</u-col>
					<u-col span="6">
						<u--text :text="'违章:'+data.outstandingViolation" ></u--text>
					</u-col>
					
				</u-row>
				
				<u-row justify="center"  >
					<u-col span="6">
						<u--text :text="'驱动形式:'+data.drivingForm" ></u--text>
					</u-col>
					<u-col span="6">
						<u--text :text="'前后制动器类型:'+data.frontAndRearBrakeTypes" ></u--text>
					</u-col>
					
				</u-row>
				<u-row justify="center"  >
					<u-col span="12" >
						<u--text :text="'前悬挂类型:'+data.frontSuspensionType" ></u--text>
					</u-col>
					
				</u-row>

				<u-row justify="center"  >
					<u-col span="12" >
						<u--text :text="'注册日期:'+data.registrationDate" ></u--text>
					</u-col>
					
				</u-row>
				<u-row justify="center"  >
					<u-col span="12" >
						<u--text :text="'发动机号:'+data.engineNumber" ></u--text>
					</u-col>
					
				</u-row>
				<u-row justify="center"  >
					<u-col span="12" >
						<u--text :text="'备胎规格:'+data.spareGauge" ></u--text>
					</u-col>
					
				</u-row>
				
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				id:null,
				data:[]
			}
		},
		onLoad(option) {
			this.id=option.id;
			this.getCarDetail();
			
		},
		methods: {
			getCarDetail(){
				this.$request.get(`/api/recharge/vehicle/${this.id}`)
				.then(res=>{
					if(res.code==200){
						this.data=res.data;
						
					}else{
						this.$request.requestShowToastError(res.msg);
					}
				})
				.catch(err=>{
					this.$request.requestShowToastError(err);
				})
			}
		}
	}
</script>

<style lang="scss">
	@import url('../../styles.scss');

</style>
