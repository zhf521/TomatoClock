<template>
	<view class="todo">
		我是任务
	</view>
	<view class="clock">
		<text v-if="!completed">番茄钟倒计时:{{formattedTime}} </text>
		<text v-else>番茄钟已完成！</text>
	</view>
	<view class="btn">
		<button @click="startTimer" v-if="!isRunning && !completed">{{isGoOn ? '继续' : '开始'}}</button>
		<button @click="pauseTimer" v-if="isRunning && !completed">暂停</button>
		<button @click="stopTimer" v-if="isStop">停止</button>
	</view>
</template>

<script setup>
	import {
		ref,
		computed,
		watch
	} from 'vue'
	const time = ref(15) // 初始倒计时时间（单位：秒）
	const timer = ref() // 计时器ID
	const isRunning = ref(false) // 是否正在运行计时器
	const completed = ref(false) // 是否完成番茄钟
	const isGoOn = ref(false) // 是否继续
	const isStop = ref(false) // 是否停止

	// 格式化剩余时间
	const formattedTime = computed(() => {
		const minutes = Math.floor(time.value / 60);
		const seconds = time.value % 60;
		return `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
	});
	// 监听时间变化，并处理倒计时结束的情况
	watch(time, (newTime) => {
		if (newTime === 0) {
			completed.value = true;
			clearInterval(timer.value);
		}
	})
	// 开始计时器
	const startTimer = () => {
		isRunning.value = true
		isStop.value = false
		timer.value = setInterval(() => {
			time.value--
		}, 1000)
	}

	// 暂停计时器
	const pauseTimer = () => {
		isRunning.value = false
		clearInterval(timer.value)
		isGoOn.value = true
		isStop.value = true
	}

	// 停止计时器
	const stopTimer = () => {
		isRunning.value = false
		clearInterval(timer.value)
		time.value = 15 // 倒计时时间为初始值
		completed.value = false
		isGoOn.value = false
		isStop.value = false
	}
</script>

<style lang="scss">

</style>