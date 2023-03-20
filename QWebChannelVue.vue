<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>QWebChannel在vue版本的使用</title>
		<!-- 引入Vue 注意地址-->
		<script type="text/javascript" src="../js/vue.js"></script>
	</head>
	<body>
		
		<!-- 准备好一个容器-->
		<div id="root">
			<button id="but_bool" class="button" onclick="butClick_bool()">bool类型</button>
			<button id="but_double" class="button" onclick="butClick_double()">double类型</button>
			<button id="but_str" class="button" onclick="butClick_str()">string类型</button>
			<button id="but_arr" class="button" onclick="butClick_arr()">array类型</button>
			<button id="but_obj" class="button" onclick="butClick_obj()">object类型</button> </br>
			
			<button id="but_Property" class="button" onclick="butClick_Property()">读取并修改Qt对象中的属性</button>
			<button id="but_enum" class="button" onclick="butClick_enum()">读取Q_ENUM标记的枚举</button>
			<button id="but_return" class="button" onclick="butClick_return()">调用带返回值的Qt函数</button>
			<textarea ref="myTextarea" id="textAreaId"></textarea>
		</div>
	</body>
	<!-- 引入demo  html中的QWebChannel.js -->
	<script>
		import QWebChannel from '@/utils/qwebchannel'// qt通讯
	</script>
	<script type="text/javascript">
		Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

		const vm = new Vue({
			el:'#root',
			data:{
				name:'尚硅谷',
				// qt
				qwebChannel: null,
				qtString:{
					id:'1',
					key:'abc'
				}
			},
			mounted() {
				// qt通讯
    			// eslint-disable-next-line eqeqeq
				if (typeof qt != 'undefined') {
					window.channel = new QWebChannel(qt.webChannelTransport, (channel) => {
						window.core = channel.objects.CoreId
					})
				}else{
					alert("qc对象未获取到！");
				}
			},
			methods:{
				butClick_bool(){
					// 发送bool类型
					if (window.core) {
						window.core.on_toQtBool(true)
					}
				},
				butClick_double(){
					// 发送double类型
					if (window.core) {
						window.core.on_toQtDouble(123.321)
					}
				},
				butClick_str(){
					// 发送信息 字符串格式
					if (window.core) {
						window.core.on_toQtString(this.qtString.id + ',' + this.qtString.key)
					}
				},
				butClick_arr(){
					// 发送array类型
					if (window.core) {
						const arr = [1, 2, "123"];
						window.core.on_toQtJsonArray(arr)
					}
				},
				butClick_obj(){
					// 发送object类型
					const obj = {
						key1 : 123,
						key2 : 321.1,
						key3 : "abc"
					}
					if (window.core) {
						window.core.on_toQtJsonObject(obj)
					}
					
				},
				butClick_Property(){
					// 读取并修改Qt对象中的属性
					const textArea = this.$refs.myTextarea;
					if (window.core && textArea) {
						textArea.value = textEdit.value + "Qt对象属性：" + window.core.value + '\n';              // 读取Qt中使用Q_PROPERTY定义的属性值
						textArea.scrollTop = textArea.scrollHeight;             // 滚动条一直再最下方
						window.core.value++;              
					}
				},
				butClick_enum(){
					// 读取Q_ENUM标记的枚举
					const textArea = this.$refs.myTextarea;
					if (window.core && textArea) {
						textArea.value = textArea.value + "Qt枚举值：" + window.core.CoreEnum.Value1 + '\n';  
        				textArea.scrollTop = textArea.scrollHeight;         
					}
				},
				butClick_return(){
					// 发送信息 字符串格式
					if (window.core) {
						window.core.on_returnValue(123, function(returnValue)  // 这里使用回调函数获取返回值
						{
						const textArea = this.$refs.myTextarea;
						if(textArea)
						{
							textArea.value = textArea.value + "Qt函数返回值：" + returnValue + '\n';  
							textArea.scrollTop = textArea.scrollHeight;             // 滚动条一直再最下方
						}
					});

					}
				},
				butClick_str(){
					// 发送信息 字符串格式
					if (window.core) {
						window.core.on_toQtString(this.qtString.id + ',' + this.qtString.key)
					}
				}
			}
		})
	</script>
	<style>
		.button {
			background-color: #4CAF50; /* Green */
			border: none;
			color: white;
			padding: 15px 32px;
			text-align: center;
			text-decoration: none;
			display: inline-block;
			font-size: 14px;
			margin: 4px 2px;
			cursor: pointer;
			-webkit-transition-duration: 0.4s; /* Safari */
			transition-duration: 0.4s;
			box-shadow: 0 8px 16px 0 rgba(0,0,0,0.2), 0 6px 20px 0 rgba(0,0,0,0.19);
		}
		.button:hover {
			box-shadow: 0 12px 16px 0 rgba(0,0,0,0.24),0 17px 50px 0 rgba(0,0,0,0.19);
		}

		#textAreaId {
			width: 80%;
			height: 100px;
		}

		h1 {
			font-size: 40px;
			color:red;
		}

	</style>
</html>