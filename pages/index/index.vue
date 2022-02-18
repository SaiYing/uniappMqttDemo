<template>
	<view class="content">
		<button @click="connect" type="primary">connect</button>
		<view class="log">
			<view v-for="(log,index) in logs" class="" :key="index">
				{{log}}
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: 'Hello',
				logs: []
			}
		},
		methods: {
			connect() {
				var self = this
				self.logs.push('connect')
				var mqtt = require('mqtt/dist/mqtt.js')
				// #ifdef H5
				var client = mqtt.connect('ws://mqtt.hnbxwhy.com:8083/mqtt')
				// #endif
				// #ifdef MP-WEIXIN||APP-PLUS
				var client = mqtt.connect('wx://mqtt.hnbxwhy.com:8083/mqtt')
				// #endif
				client.on('connect', function() {
					self.logs.push('on connect')
					
				}).on('reconnect', function() {
					self.logs.push('on reconnect')
				}).on('error', function() {
					self.logs.push('on error')
				}).on('end', function() {
					self.logs.push('on end')
				}).on('message', function(topic, message) {
					console.log("topic",topic)
					console.log('message',message.toString())
					console.log('message2',JSON.parse(message.toString()))
					self.logs.push(message.toString())
				})
				client.subscribe('AppAct22279', function(err) {
					if (!err) {
						client.publish('AppAct22279', '<p>你好我是刘新可</p>')
					}
				})
			}
		}
	}
</script>

<style>
	.content {
		text-align: center;
		height: 400upx;
		word-break: break-all;
	}
</style>
