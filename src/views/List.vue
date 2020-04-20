<template>
	<div>
		<div class="header" v-if="isNone">你还没有添加任何便笺哦~~</div>
		<div class="item-container" v-if="!isDetail">
			<div class="item" v-for="(item,index) in pageLists" :key='index' @click="toggleItem(index)" :class="{'active' : index === activenIndex}">
				<span class="delete" v-show="index==activenIndex" @click.stop="delItem(index)">&times;</span>
				<p class="title">{{item.title}}</p>
				<p class="content">{{shortContent[index]}}</p>
				<div class="footer">
					<div>{{item.time}}</div>
					<a v-if="index==activenIndex" @click.stop="goDetail(index)">查看详情>></a>
				</div>
			</div>
		</div>

		<div class="detail" v-else>
			<p class="title">{{detailTitle}}</p>
			<p class="content">{{detailContent}}</p>
			<button @click="backList()">返回</button>
		</div>
	</div>
</template>

<script>
	import store from '@/store'
	export default {
		name: "List",
		store,
		data: function() {
			return {
				activenIndex: -1,
				isDetail: false,
				detailTitle: '',
				detailContent: ''
			}
		},
		mounted: function() {
			store.commit('getLocal')
		},
		computed:{
			pageLists: function() {
				return store.getters.myLists
			},
			isNone: function() {
				if (this.pageLists.length === 0) {
					return true
				} else {
					return false
				}
			},
			shortContent: function() {
				var shortContent = []
				this.pageLists.forEach(function(item) {
					if (item.content.length <= 21) {
						shortContent.push(item.content)
					} else {
						var cut = item.content.slice(0, 20) + '...'
						shortContent.push(cut)
					}
				})
			return shortContent
		}
	},
	methods:{
		toggleItem(index) {
				if (this.activenIndex === index) {
					this.activenIndex = -1
				} else {
					this.activenIndex = index
				}
			},
			delItem(index) {
				store.commit('delItem', index)
				localStorage["pageLists"] = JSON.stringify(store.state.lists) //存到本地
			},
			goDetail(index) {
				this.isDetail = true
				this.detailTitle = this.pageLists[index].title
				this.detailContent = this.pageLists[index].content
			},
			backList() {
				this.isDetail = false
			}
		}
	}
</script>
<style scoped lang="scss">
.header{
	text-align: center;
	margin: 5px;
	font-size: 14px;
}
.item-container{
	position: absolute;
	top:0%;
	bottom: 60px;
	overflow: auto;
	width: 100%;
}
.item{
	position: relative;
	border: 1px solid #d5d5d5;
	border-radius: 7px;
	margin: 10px;
	font-size: 14px;
	box-shadow: 2px 3px 3px #D5D5D5;
	.title{
		height: 25px;
		line-height: 30px;
		padding: 5px;
		font-weight: bold;
		border-bottom: 1px solid #D5D5D5;
	}
	.content{
		height: 25px;
		line-height: 25px;
		padding: 5px;
	}
	.footer{
		height: 20px;
		display: flex;
		justify-content: space-between;
		font-size: 12px;
		margin: 5px;
		a{
			text-decoration: underline;
			cursor: pointer;
		}
	}
	&.active{
		color: #fff;
		background: #42B983;
	}
	.delete{
		position: absolute;
		top: 5px;
		left: 94%;
		font-size: 20px;
		cursor: pointer;
		&:hover{
			color: ren;
			font-weight: bold;
			font-size: 22px;
		}
	}
}
.detail{
	position: absolute;
	top: 0;
	bottom: 60px;
	overflow: auto;
	width: 100%;
	.title{
		padding: 10px;
		font-weight: bold;
		border-bottom: 1px solid #d5d5d5;
	}
	.content{
		font-size: 14px;
		padding: 15px;
		letter-spacing: 2px;
		word-wrap:break-word
	}
	button{
		width:90%;
		margin-left:5%;
		margin-top:30px;
		margin-bottom:20px;
		height:35px;
		line-height:35px;
		border-radius:7px;
		border:1px solid #42b983;
		background-color:#42b983;
		color:#fff;
		cursor:pointer;
	}
}
</style>
