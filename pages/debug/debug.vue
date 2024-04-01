<template>
	<view class="container">
		<view class="list-item">
			<view class="title-tag">
				<u--text text="空调"></u--text>
			</view>
			<u-divider></u-divider>
			<view>
				<u-row justify="center">
					<u-col span="4" >
						<u--text :text="temp+'℃'" ></u--text>
						<u--text :text="radiovalue1"></u--text>
					</u-col>
					<u-col span="8" style="padding: 0 .5rem;">
						<u-row>
							<u-col span="6">
								<u--text text="空调"></u--text>
							</u-col>
							<u-col span="6" align="flex-end">
								<u-switch v-model="airSwitch"  size="20"></u-switch>
							</u-col>
						</u-row>
						<div style="padding: .25rem;"></div>
						<u-row>
							<u-col span="6">
								<u--text text="设置温度"></u--text>
							</u-col>
							<u-col span="6" align="flex-end">
								<view class="flex">
									<u-button class="custom-style" shape="circle" style="margin-right: .25rem;" icon="arrow-up" size="mini"></u-button>
									<u-button class="custom-style" shape="circle"  icon="arrow-down" loadingSize="10"  size="mini"></u-button>
								</view>
							</u-col>
						</u-row>
						<div style="padding: .25rem;"></div>
						<u-row>
							<u-col span="3">
								<u--text text="循环"></u--text>
							</u-col>
							<u-col span="6" align="flex-end">
								<u-radio-group
								    v-model="radiovalue1"
								   
								  
								  >
								    <u-radio
								      :customStyle="{marginRight: '8px'}"
								      v-for="(item, index) in radiolist1"
								      :key="index"
								      :label="item.name"
								      :name="item.name"
								     
								    >
								    </u-radio>
								  </u-radio-group>
							</u-col>
							
						</u-row>
						<div style="padding: .25rem;"></div>
					</u-col>
					
				</u-row>
			</view>
		</view>
		<view class="list-item">
			<view class="title-tag">
				<u--text text="车机"></u--text>
			</view>
			<u-divider></u-divider>
			<view>
				<u-row justify="center">
					<u-col span="4" >
						<u--text :text="'当前挡位:'+'R档'" size="14" ></u--text>
						<u--text :text="'方向盘方向:'+'左转'"  size="14" ></u--text>
						<u--text :text="'剩余电量:'+'90%'"  size="14" ></u--text>
						<u--text :text="'电源状态:'+'开启'"  size="14" ></u--text>
						<u--text :text="'电池最大容量:'+'100%'"  size="14" ></u--text>
						<br>
						<u--text text="车轮胎压"></u--text>
						<u-number-box v-model="numbervalue" step="0.1" ></u-number-box>
					</u-col>
					<u-col span="8" style="padding: 0 .5rem;">
						<u-row>
							<u-col span="12">
								<u--text text="设置挡位信息"></u--text>
								<view class="flex">
									<u-slider v-model="sliderValue" step="10" min="0" max="20" style="width: 50%;" ></u-slider>
									<u--text :text="sliderValue==0?'P档':sliderValue==10?'R档':'D档'"></u--text>
								</view>
							</u-col>
						</u-row>
						<div style="padding: .25rem;"></div>
						<u-row>
							<u-col span="12">
								<u--text text="方向"></u--text>
								<view class="flex" style="justify-content: start;">
									<u-icon name="arrow-leftward" :color="!turnSwitch?'#909399':'#fa3534'" size="24" @click="turnSwitch=true"></u-icon>
									<div style="padding: .25rem;"></div>
									<u-icon name="arrow-rightward" :color="turnSwitch?'#909399':'#fa3534'" size="24" @click="turnSwitch=false"></u-icon>
								</view>
								
							</u-col>
						</u-row>
						<u-row>
							<u-col span="12">
								<u--text text="启动/关闭"></u--text>
								<u-switch v-model="switchIt" size="20"></u-switch>
							</u-col>
						</u-row>
						<div style="padding: .25rem;"></div>
						<view class="flex" style="justify-content: start;">
							<u-icon name="skip-back-left" size="28"></u-icon>
							<u-icon :name="isPause?'pause-circle':'play-circle'" size="28"></u-icon>
							<u-icon name="skip-forward-right" size="28"></u-icon>
							<u-icon :name="mute?'volume-off':'volume'" size="28"></u-icon>
						</view>
						<div style="padding: .25rem;"></div>
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
				client:undefined,
				airSwitch:true,
				temp:17,
				numbervalue:1.2,
				sliderValue:0,
				turnSwitch:true,
				switchIt:true,
				isPause:true,
				mute:false,
				radiolist1: [{
				            name: '内循环',
				            disabled: false
				          },
				            {
				              name: '外循环',
				              disabled: false
				            },
				          ],
				radiovalue1:'内循环',
				MQTTdist:{
					'02004001000000000000000':()=>(this.airSwitch=true),
					'02004002000000000000000':()=>(this.airSwitch=false),
					'02005003000000000000000':()=>(this.temp++),
					'02005001000000000000000':()=>(this.temp--),
					'02006002000000000000000':()=>(this.radiovalue1='外循环'),
					'02006001000000000000000':()=>(this.radiovalue1="内循环"),
					'03010002000000000000000':()=>(this.sliderValue=10),
					'03010001000000000000000':()=>(this.sliderValue=0),
					'03010003000000000000000':()=>(this.sliderValue=20),
					'03011002000000000000000':()=>(this.turnSwitch=false),
					'03011001000000000000000':()=>(this.turnSwitch=true),
					'03012001000000000000000':()=>(this.switchIt=true),
					'03012002000000000000000':()=>(this.switchIt=false),
					'03014001000000000000000':()=>(this.isPause=false),
					'03013001000000000000000':()=>(this.isPause=true),
					'03017001000000000000000':()=>(this.mute=true),
					'03018001000000000000000':()=>(this.mute=false)
				}
				
			}
		},
		onLoad() {
			this.MQTTINIT();
		},
		methods: {
			
			asciiStringTo(decimalData) {
			  // 使用TextEncoder将Uint8Array转换为二进制字符串
			  const asciiChars = decimalData.map(decimal => String.fromCharCode(decimal));
			  
			  // 将 ASCII 字符连接起来形成字符串
			  const asciiString = asciiChars.join('');
			  return asciiString;
			},
			MQTTINIT(){
					let options = {
						username: 'user',
						clientId: 'mqttx_221a5de1',
						connectTimeout: 600000,
						keepalive: 5, // 心跳，单位秒，如果后台开启监听可以快速知道有没有退出
						clean: true,
						// password: '',
			      		// reconnectPeriod: 5000,
					}
					// 订阅 topic。如果订阅多个 var subscribe = ["test1","test2"]
					const subscribe = ['android2can','can2android']
					if(this.client){ 
						this.client.end()
					}
					// #ifdef APP-PLUS
						this.client = this.$MQTT.connect('wx://192.168.95.1:8083/mqtt',options)  
					// #endif
					// #ifdef H5
						this.client = this.$MQTT.connect('ws://192.168.95.1:8083/mqtt',options)  
					// #endif
					const _this=this;
					this.client.on('connect', (res)=>{  						
						uni.hideLoading()
						uni.showToast({  
							title:"连接成功",  
							duration:2000,  
							icon:"none"  
						})
						this.client.subscribe(subscribe , (err)=>{  
							console.log(err,2222);
							if (!err) {  
								uni.showToast({  
									title:"订阅成功",  
									duration:2000,  
									icon:"none"  
								})  
							}  
						})  
					}).on('message', (topic, message)=>{  
						console.log(topic,this.asciiStringTo(message))  
						const messageString=this.asciiStringTo(message);
						this.MQTTdist[messageString]()
					}).on('reconnect', (topic, message)=>{  
						uni.showLoading({
							title: '重连中...'
						})
						console.log("重连")  
					}).on('error',()=>{
						//重新连接
						client.reconnect()
					})
					// close： function () {} 断开连接回调
					// offline ： function () {} 客户端离线回调
					// 发送，
					// this.client.publish('system','hello',{},(res)=>{
					// 		console.log(res)
					// })
					//  system: topic订阅的主题,第二个参数发送内容Buffer or String类型(可以转成JSON字符串JSON.stringify()),第三个参数[options]看下方【不必填】,第四个回调【不必填】
					// 	qos QoS 等级, Number, default 0
					//	retain 保留消息标志, Boolean, default false
					//	dup 重复标志, Boolean, default false
					
				}
		}
	}
</script>

<style lang="scss">
	@import url('/styles.scss');
</style>
