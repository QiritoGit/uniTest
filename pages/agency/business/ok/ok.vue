<template>
	<view class="bgc-e5 page">
		<invoice-suggest :suggest="suggest"></invoice-suggest>
		<ul class="boxf-t20">
			<li class="border"><pluralLine :line="materialInfo"></pluralLine></li>
		</ul>
		<ul class="boxf-t20">
			<li class="border"><baseLine :line="shopInfo"></baseLine></li>
			<li>
				<baseLine :line="nameInfo">
					<slot><text @click="showPhoto" class="slot">查看身份证</text></slot>
				</baseLine>
			</li>
			<li>
				<baseLine :line="telInfo">
					<slot>
						<image @click="goTel" class="imgx35 slot" src="../../../../static/img/icon/tel2.png"></image>
					</slot>
				</baseLine>
			</li>
		</ul>
		<ul class="boxf-t20">
			<li class="border"><baseLine :line="fileList[0]"></baseLine></li>
			<li class="border"><baseLine :line="fileList[1]"></baseLine></li>
			<li class="border"><baseLine :line="fileList[2]"></baseLine></li>
		</ul>
	</view>
</template>

<script>
	import { mapState, mapMutations } from 'vuex';
	
	import invoiceSuggest from "../../../../components/byInvoice/invoiceSuggest.vue"
	import baseLine from "../../../../components/byInvoice/baseLine.vue"
	import pluralLine from "../../../../components/byInvoice/pluralLine.vue"
	export default {
		data() {
			return {
				suggest:{
					image:'../../static/img/other/wait-time.png',
					title:'审核已通过',
					text:'请携带以下材料到工商窗口进行办理'
				},
				shopInfo:{
					title:'名  称',
					text:''
				},
				materialInfo:{
					title:'所需材料',
					text:[
						{
							line:'1、(需打印)业务综合申报材料(共4页)'
						},
						{
							line:'2、(需打印)经营场所证明（共1页）'
						},
						{
							line:'3、代理人身份证原件'
						}
					]
				},
				nameInfo:{
					title:'姓  名',
					text:''
				},
				telInfo:{
					title:'联系方式',
					text:''
				},
				fileList:[
					{
						title:'通知文号',
						text:'(景工商)个体名预核字[2019]第004368号'
					},
					{
						title:'经营场所',
						text:'浙江省丽水市景宁畲族自治县红星街道惠民路82号五楼501-4'
					},
					{
						title:'经营范围',
						text:'网上销售：服装，鞋帽配饰，箱包，母婴用品及日用百货，化妆品及卫生用品（依法须经批准的项目，经相关部门批准后方可开展经营活动）'
					}
				]
			}
		},
		components:{
			invoiceSuggest,
			baseLine,
			pluralLine
		},
		onLoad:function(e){
			this.shopInfo.text = e.shop;
			this.nameInfo.text = e.name;
			this.telInfo.text = e.tel;
			this.suggest.title = e.state;
		},
		methods: {
			goTel(){
				uni.makePhoneCall({
					phoneNumber: '114'
				});
			},
			showPhoto(){
				console.log('1')
				// 这里 要考虑一下是oss传过来图片，还是直接引用本地图片
				// uni.previewImage({
				// 	urls: '',
				// 	longPressActions: {
				// 	itemList: ['', ''],
				// 		success: function (res) {
				// 		}   
				// 	}
				// });
				uni.chooseImage({
					count: 2,
					sizeType: ['original', 'compressed'],
					sourceType: ['album'],
					success: function (res) {
						// 预览图片
						uni.previewImage({
							urls: res.tempFilePaths,
							longPressActions: {
							itemList: ['发送给朋友', '收藏'],
								success: function (res) {
									console.log('选中了第' + (res.tapIndex + 1) + '个按钮');
								},
								fail: function (res) {
									console.log(res.errMsg);
								}    
							}
						});
					}
				});
			}
		}
	}
</script>

<style scoped>
	.page{
		padding-bottom: 100upx;
	}
	.border{
		border-bottom: 1px solid #E5E5E5;
	}
	.slot{
		float: right;
		color: #0E8EFF;
	}
</style>
