<template>
	<div class="main-form" v-if="!isPass">
		<div class="main-content">
			<div class="age-logo"></div>
			<p class="age-text">請 輸 入 您 的 出 生 (西元)年 月 日</p>
			<div class="age-input">
				<input type="text" class="age-input-text" v-model="year" maxlength="4" placeholder="YYYY" />
				<input type="text" class="age-input-text" v-model="month" maxlength="2" placeholder="MM" />
				<input type="text" class="age-input-text" v-model="day" maxlength="2" placeholder="DD" />
			</div>
			<div class="btn-wrap" @click="doSubmit()">送出</div>
			<p class="age-text age-text-bottom">必須滿18歲才可進入，謝謝</p>
		</div>

		<transition name="slide-fade">
			<div :class="['modal-content', { 'modal-content-date': showDataError }]" v-show="showAgeError || showDataError">
				<div class="close" @click="closeError()"></div>
				<div class="msg-header">網站訊息</div>
				<div class="msg-text age" v-show="showAgeError">很抱歉，未滿18歲不得進入</div>
				<div class="msg-text data" v-show="showDataError">請輸入正確日期格式</div>
			</div>
		</transition>

		<div class="lang-mask error-mask" @click="closeError()" v-show="showAgeError || showDataError"></div>

		<div class="bg-blur"></div>
	</div>
</template>

<script>
import moment from 'moment'
export default {
	data() {
		return {
			day: null,
			month: null,
			year: null,
			showAgeError: false,
			showDataError: false,
			lang: null,
			isPass: false,
		}
	},
	computed: {
		date() {
			return `${this.year}-${this.month}-${this.day}`
		},
	},
	methods: {
		closeError() {
			this.showAgeError = false;
			this.showDataError = false;
		},
		doSubmit() {
			let age = moment().diff(this.date, 'years', true)
			if ((!moment(this.date, 'YYYY-M-D', true).isValid()) || (this.year < moment().year() - 100) || (age <= 0)) {
				this.showDataError = true
				return
			} else if (age < 18) {
				this.showAgeError = true
				return
			}
			setTimeout(() => {
				this.isPass = true;
			}, 1000)
		},
	}
}
</script>

<style>
.main-form {
	position: fixed;
	left: 0;
	top: 0;
	width: 100%;
	height: 100vh;
	z-index: 221;
	margin: 0;
	color: #fff;
	background-color: rgba(3, 28, 51, 0.7);
}

.main-form .main-content {
	position: absolute;
	top: 40%;
	left: 50%;
	width: 22rem;
	padding: 1rem;
	text-align: center;
	transform: translateY(-40%) translateX(-50%);
}

.main-form .main-content .age-logo {
	background-image: url(../assets/logo.png);
	width: 160px;
	height: 40px;
	margin: auto;
}

.main-form .main-content .age-text {
	margin: 2rem auto;
	font-size: 18px;
	cursor: default;
}

.main-form .main-content .age-text-bottom {
	margin: 1.7rem auto;
	font-size: 14px;
}

.main-form .main-content .age-input {
	display: inline-flex;
	margin: 0 0 20px;
}

.main-form .main-content .age-input .age-input-text {
	display: inline-block;
	border-radius: 7px;
	border: solid 1px rgba(255, 255, 255, 0);
	background-color: rgba(255, 255, 255, 0.3);
	text-align: center;
	width: 100%;
	height: 50px;
	line-height: 50px;
	color: #fff;
	font-size: 18px;
	margin: 0 6px;
	cursor: default;
}

.main-form .main-content .age-input .age-input-text::placeholder {
	/* Chrome, Firefox, Opera, Safari 10.1+ */
	color: #b4b4b4;
	opacity: 1;
	/* Firefox */
}

.main-form .main-content .age-input .age-input-text:focus {
	border: solid 1px #408aed;
}

.main-form .main-content .language-form {
	position: relative;
	margin: 0 auto 20px;
	width: 96%;
	cursor: pointer;
}

.main-form .main-content .language-form:before {
	content: "";
	display: inline-block;
	width: 25px;
	height: 25px;
	background-image: url(../assets/lang.png);
	background-size: cover;
	position: absolute;
	top: 50%;
	margin-top: -12px;
	left: 16px;
}

.main-form .main-content .language-form:after {
	content: "";
	display: inline-block;
	position: absolute;
	width: 11px;
	height: 11px;
	top: 50%;
	margin-top: -10px;
	right: 20px;
	border: none;
	border-right: 2px solid #8abcff;
	border-bottom: 2px solid #8abcff;
	-webkit-transform: rotate(45deg);
	transform: rotate(45deg);
	z-index: -1;
}

.main-form .main-content .language-form .lang-check label {
	position: relative;
	border: solid 1px rgba(255, 255, 255, 0);
	background-color: rgba(255, 255, 255, 0.3);
	border-radius: 7px;
	width: 100%;
	height: 50px;
	line-height: 50px;
	color: #fff;
	font-size: 18px;
	display: block;
	cursor: pointer;
	text-align: left;
	text-indent: 3.5rem;
}

.main-form .main-content .language-form .lang-check label:hover {
	border: solid 1px #408aed;
}

.main-form .main-content .language-form .lang-check input:not(:checked)+label {
	display: none;
}

.main-form .main-content .btn-wrap {
	background-color: #5941eb;
	width: 97%;
	height: 52px;
	line-height: 52px;
	border-radius: 7px;
	margin: auto;
	cursor: pointer;
	font-size: 18px;
	transition: all 0.2s;
}

.main-form .main-content .btn-wrap:hover {
	background-color: #66a7ff;
}

.main-form .lang-content {
	position: absolute;
	top: 40%;
	left: 50%;
	width: 20rem;
	transform: translateX(-50%);
	z-index: 200;
}

.main-form .lang-content .lang-check-content {
	position: absolute;
	background-color: #111;
	width: 100%;
}

.main-form .lang-content .lang-check-content .lang-check {
	line-height: 3rem;
	height: 3rem;
	text-align: center;
	border: solid 1px #408aed;
	color: #408aed;
	cursor: pointer;
	transition: all 0.2s;
}

.main-form .lang-content .lang-check-content .lang-check:hover {
	color: #fff;
	background-color: rgba(180, 180, 180, 0.3);
}

.main-form .lang-content .lang-check-content .lang-check label {
	text-align: center;
	display: inline-block;
	width: 100%;
	cursor: pointer;
	font-size: 18px;
}

.main-form .lang-mask {
	position: fixed;
	top: 0;
	left: 0;
	right: 0;
	bottom: 0;
	background-color: rgba(0, 0, 0, 0.8);
	z-index: 101;
}

.main-form .lang-mask.error-mask {
	background-color: rgba(0, 0, 0, 0.5);
}

.main-form .modal-content {
	border-radius: 5px;
	max-width: 500px;
	padding: 1rem 2rem 2rem;
	color: #666;
	text-align: justify;
	background-color: #fff;
	cursor: default;
	z-index: 999;
	position: absolute;
	top: 38%;
	left: 50%;
	transform: translateY(-38%) translateX(-50%);
}

.main-form .modal-content .close {
	position: absolute;
	top: 1.5rem;
	right: 2rem;
	cursor: pointer;
	width: 18px;
	height: 18px;
}

.main-form .modal-content .close:before,
.main-form .modal-content .close:after {
	content: "";
	width: 18px;
	height: 2px;
	position: absolute;
	background-color: #666;
}

.main-form .modal-content .close:before {
	transform: rotate(45deg) translate(6px, 6px);
}

.main-form .modal-content .close:after {
	transform: rotate(-45deg) translate(-6px, 6px);
}

.main-form .modal-content-date .close {
	top: 0.5rem;
	right: 0.6rem;
}

.main-form .modal-content .msg-header {
	color: #408aed;
	font-size: 20px;
	line-height: 2;
	border-bottom: 1px solid #d0d0d0;
	padding-bottom: 0.5rem;
}

.main-form .modal-content .msg-text {
	color: #666;
	font-size: 16px;
	line-height: 1.2;
	margin-top: 1rem;
}

.main-form .bg-blur {
	position: absolute;
	top: 0;
	right: 0;
	bottom: 0;
	left: 0;
	backdrop-filter: blur(0.35rem);
	z-index: -1;
}

input[type="radio"] {
	visibility: hidden;
	height: 0;
	width: 0;
	display: none;
}

.slide-fade-enter-active {
	transition: all 0.3s ease;
}

.slide-fade-enter,
.slide-fade-leave-active {
	margin-top: 2rem;
	opacity: 0;
}
</style>
