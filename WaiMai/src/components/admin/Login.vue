<template>
	<div id="login" class="materialContainer">
		<form id="loginForm" onsubmit="return false">
			<div class="box">
				<div class="title">登录</div>
				<div class="input">
					<label for="name">用户名</label>
					<input type="text" name="name" id="name">
					<span class="spin" v-if="!loginStatus">用户名或密码不正确</span>
					<span class="spin" v-else></span>

				</div>
				<div class="input">
					<label for="pass">密码</label>
					<input type="password" name="pass" id="pass">
					<span class="spin"></span>
				</div>
				<div @click="login()" class="button login">
					<button>
						<span>登录</span>
						<i class="fa fa-check"></i>
					</button>
				</div>
				<a href="javascript:" class="pass-forgot">忘记密码？</a>
			</div>
		</form>
		<div class="overbox">
			<div class="material-button alt-2">
				<span class="shape"></span>
			</div>
			<div class="title">注册</div>
			<div class="input">
				<label for="regname">用户名</label>
				<input type="text" name="regname" id="regname">
				<span class="spin"></span>
			</div>
			<div class="input">
				<label for="regpass">密码</label>
				<input type="password" name="regpass" id="regpass">
				<span class="spin"></span>
			</div>
			<div class="input">
				<label for="reregpass">确认密码</label>
				<input type="password" name="reregpass" id="reregpass">
				<span class="spin"></span>
			</div>
			<div class="button">
				<button>
					<span>注册</span>
				</button>
			</div>
		</div>

	</div>
</template>

<script>

	import '../../../static/login/css/style.css';
	import '../../../static/login/js/index.js';
	import '../../../static/login/js/login.js';
	import '../../../static/login/js/index.js';
	import 'font-awesome/css/font-awesome.min.css';


	export default {
		created() {
			//this.getAdmin();
			document.body.style.backgroundImage = "url(../static/login/images/bg.png)";
		},
		destroyed() {
			document.body.style.backgroundImage = "url()";
		},
		name: "Login",
		components: {
			// Confirm
		},
		data() {
			return {
				// 登录状态
				loginStatus: true
			}
		},
		methods: {
			getMessage(val) {
				alert(val);
			},
			getAdmin: function () {
				this.$axios.get(this.Global.login + "/stu/stu").then(res => {
					var result = res.data;
					console.log(result);
				})
			},
			// 登录
			login: function () {
				var _this = this;
				var form = document.getElementById('loginForm');
				var formData = new FormData(form);
				var name = formData.get('name');
				var pass = formData.get('pass');
				//'http://localhost/admin.php/admin/loginModel?AdminName='+name+'&AdminPassword='+pass,
				var data = { adminName: name, adminPassword: pass }
				// 判断不能为空
				if (name == "" || pass == "") {
					alert("用户名和密码不能为空")
				} else {
					// 请求登录
					this.$axios.post('http://localhost/admin.php/admin/loginModel', this.$qs.stringify(data)).then(res => {
						switch (res.data) {
							case "parent_admin":
								this.$router.push({ name: 'Admin', params: { show: 'parent_admin' } });
								 this.$cookies.set("show", this.$route.params.show); //存session
								 
								break;
							case "admin":
								this.$router.push({ name: 'Admin', params: { show: 'admin' } });
								this.$cookies.set("show", this.$route.params.show);
								break;
							default:
								this.loginStatus = false;
						}
					});
				}

			}
		}
	}

</script>