(function($) {
	var rotateLeft = function(lValue, iShiftBits) {
		return(lValue << iShiftBits) | (lValue >>> (32 - iShiftBits));
	}
	var addUnsigned = function(lX, lY) {
		var lX4, lY4, lX8, lY8, lResult;
		lX8 = (lX & 0x80000000);
		lY8 = (lY & 0x80000000);
		lX4 = (lX & 0x40000000);
		lY4 = (lY & 0x40000000);
		lResult = (lX & 0x3FFFFFFF) + (lY & 0x3FFFFFFF);
		if(lX4 & lY4) return(lResult ^ 0x80000000 ^ lX8 ^ lY8);
		if(lX4 | lY4) {
			if(lResult & 0x40000000) return(lResult ^ 0xC0000000 ^ lX8 ^ lY8);
			else return(lResult ^ 0x40000000 ^ lX8 ^ lY8);
		} else {
			return(lResult ^ lX8 ^ lY8);
		}
	}
	var F = function(x, y, z) {
		return(x & y) | ((~x) & z);
	}
	var G = function(x, y, z) {
		return(x & z) | (y & (~z));
	}
	var H = function(x, y, z) {
		return(x ^ y ^ z);
	}
	var I = function(x, y, z) {
		return(y ^ (x | (~z)));
	}
	var FF = function(a, b, c, d, x, s, ac) {
		a = addUnsigned(a, addUnsigned(addUnsigned(F(b, c, d), x), ac));
		return addUnsigned(rotateLeft(a, s), b);
	};
	var GG = function(a, b, c, d, x, s, ac) {
		a = addUnsigned(a, addUnsigned(addUnsigned(G(b, c, d), x), ac));
		return addUnsigned(rotateLeft(a, s), b);
	};
	var HH = function(a, b, c, d, x, s, ac) {
		a = addUnsigned(a, addUnsigned(addUnsigned(H(b, c, d), x), ac));
		return addUnsigned(rotateLeft(a, s), b);
	};
	var II = function(a, b, c, d, x, s, ac) {
		a = addUnsigned(a, addUnsigned(addUnsigned(I(b, c, d), x), ac));
		return addUnsigned(rotateLeft(a, s), b);
	};
	var convertToWordArray = function(string) {
		var lWordCount;
		var lMessageLength = string.length;
		var lNumberOfWordsTempOne = lMessageLength + 8;
		var lNumberOfWordsTempTwo = (lNumberOfWordsTempOne - (lNumberOfWordsTempOne % 64)) / 64;
		var lNumberOfWords = (lNumberOfWordsTempTwo + 1) * 16;
		var lWordArray = Array(lNumberOfWords - 1);
		var lBytePosition = 0;
		var lByteCount = 0;
		while(lByteCount < lMessageLength) {
			lWordCount = (lByteCount - (lByteCount % 4)) / 4;
			lBytePosition = (lByteCount % 4) * 8;
			lWordArray[lWordCount] = (lWordArray[lWordCount] | (string.charCodeAt(lByteCount) << lBytePosition));
			lByteCount++;
		}
		lWordCount = (lByteCount - (lByteCount % 4)) / 4;
		lBytePosition = (lByteCount % 4) * 8;
		lWordArray[lWordCount] = lWordArray[lWordCount] | (0x80 << lBytePosition);
		lWordArray[lNumberOfWords - 2] = lMessageLength << 3;
		lWordArray[lNumberOfWords - 1] = lMessageLength >>> 29;
		return lWordArray;
	};
	var wordToHex = function(lValue) {
		var WordToHexValue = "",
			WordToHexValueTemp = "",
			lByte, lCount;
		for(lCount = 0; lCount <= 3; lCount++) {
			lByte = (lValue >>> (lCount * 8)) & 255;
			WordToHexValueTemp = "0" + lByte.toString(16);
			WordToHexValue = WordToHexValue + WordToHexValueTemp.substr(WordToHexValueTemp.length - 2, 2);
		}
		return WordToHexValue;
	};
	var uTF8Encode = function(string) {
		string = string.replace(/\x0d\x0a/g, "\x0a");
		var output = "";
		for(var n = 0; n < string.length; n++) {
			var c = string.charCodeAt(n);
			if(c < 128) {
				output += String.fromCharCode(c);
			} else if((c > 127) && (c < 2048)) {
				output += String.fromCharCode((c >> 6) | 192);
				output += String.fromCharCode((c & 63) | 128);
			} else {
				output += String.fromCharCode((c >> 12) | 224);
				output += String.fromCharCode(((c >> 6) & 63) | 128);
				output += String.fromCharCode((c & 63) | 128);
			}
		}
		return output;
	};
	$.md5 = function(string) {
		var x = Array();
		var k, AA, BB, CC, DD, a, b, c, d;
		var S11 = 7,
			S12 = 12,
			S13 = 17,
			S14 = 22;
		var S21 = 5,
			S22 = 9,
			S23 = 14,
			S24 = 20;
		var S31 = 4,
			S32 = 11,
			S33 = 16,
			S34 = 23;
		var S41 = 6,
			S42 = 10,
			S43 = 15,
			S44 = 21;
		string = uTF8Encode(string);
		x = convertToWordArray(string);
		a = 0x67452301;
		b = 0xEFCDAB89;
		c = 0x98BADCFE;
		d = 0x10325476;
		for(k = 0; k < x.length; k += 16) {
			AA = a;
			BB = b;
			CC = c;
			DD = d;
			a = FF(a, b, c, d, x[k + 0], S11, 0xD76AA478);
			d = FF(d, a, b, c, x[k + 1], S12, 0xE8C7B756);
			c = FF(c, d, a, b, x[k + 2], S13, 0x242070DB);
			b = FF(b, c, d, a, x[k + 3], S14, 0xC1BDCEEE);
			a = FF(a, b, c, d, x[k + 4], S11, 0xF57C0FAF);
			d = FF(d, a, b, c, x[k + 5], S12, 0x4787C62A);
			c = FF(c, d, a, b, x[k + 6], S13, 0xA8304613);
			b = FF(b, c, d, a, x[k + 7], S14, 0xFD469501);
			a = FF(a, b, c, d, x[k + 8], S11, 0x698098D8);
			d = FF(d, a, b, c, x[k + 9], S12, 0x8B44F7AF);
			c = FF(c, d, a, b, x[k + 10], S13, 0xFFFF5BB1);
			b = FF(b, c, d, a, x[k + 11], S14, 0x895CD7BE);
			a = FF(a, b, c, d, x[k + 12], S11, 0x6B901122);
			d = FF(d, a, b, c, x[k + 13], S12, 0xFD987193);
			c = FF(c, d, a, b, x[k + 14], S13, 0xA679438E);
			b = FF(b, c, d, a, x[k + 15], S14, 0x49B40821);
			a = GG(a, b, c, d, x[k + 1], S21, 0xF61E2562);
			d = GG(d, a, b, c, x[k + 6], S22, 0xC040B340);
			c = GG(c, d, a, b, x[k + 11], S23, 0x265E5A51);
			b = GG(b, c, d, a, x[k + 0], S24, 0xE9B6C7AA);
			a = GG(a, b, c, d, x[k + 5], S21, 0xD62F105D);
			d = GG(d, a, b, c, x[k + 10], S22, 0x2441453);
			c = GG(c, d, a, b, x[k + 15], S23, 0xD8A1E681);
			b = GG(b, c, d, a, x[k + 4], S24, 0xE7D3FBC8);
			a = GG(a, b, c, d, x[k + 9], S21, 0x21E1CDE6);
			d = GG(d, a, b, c, x[k + 14], S22, 0xC33707D6);
			c = GG(c, d, a, b, x[k + 3], S23, 0xF4D50D87);
			b = GG(b, c, d, a, x[k + 8], S24, 0x455A14ED);
			a = GG(a, b, c, d, x[k + 13], S21, 0xA9E3E905);
			d = GG(d, a, b, c, x[k + 2], S22, 0xFCEFA3F8);
			c = GG(c, d, a, b, x[k + 7], S23, 0x676F02D9);
			b = GG(b, c, d, a, x[k + 12], S24, 0x8D2A4C8A);
			a = HH(a, b, c, d, x[k + 5], S31, 0xFFFA3942);
			d = HH(d, a, b, c, x[k + 8], S32, 0x8771F681);
			c = HH(c, d, a, b, x[k + 11], S33, 0x6D9D6122);
			b = HH(b, c, d, a, x[k + 14], S34, 0xFDE5380C);
			a = HH(a, b, c, d, x[k + 1], S31, 0xA4BEEA44);
			d = HH(d, a, b, c, x[k + 4], S32, 0x4BDECFA9);
			c = HH(c, d, a, b, x[k + 7], S33, 0xF6BB4B60);
			b = HH(b, c, d, a, x[k + 10], S34, 0xBEBFBC70);
			a = HH(a, b, c, d, x[k + 13], S31, 0x289B7EC6);
			d = HH(d, a, b, c, x[k + 0], S32, 0xEAA127FA);
			c = HH(c, d, a, b, x[k + 3], S33, 0xD4EF3085);
			b = HH(b, c, d, a, x[k + 6], S34, 0x4881D05);
			a = HH(a, b, c, d, x[k + 9], S31, 0xD9D4D039);
			d = HH(d, a, b, c, x[k + 12], S32, 0xE6DB99E5);
			c = HH(c, d, a, b, x[k + 15], S33, 0x1FA27CF8);
			b = HH(b, c, d, a, x[k + 2], S34, 0xC4AC5665);
			a = II(a, b, c, d, x[k + 0], S41, 0xF4292244);
			d = II(d, a, b, c, x[k + 7], S42, 0x432AFF97);
			c = II(c, d, a, b, x[k + 14], S43, 0xAB9423A7);
			b = II(b, c, d, a, x[k + 5], S44, 0xFC93A039);
			a = II(a, b, c, d, x[k + 12], S41, 0x655B59C3);
			d = II(d, a, b, c, x[k + 3], S42, 0x8F0CCC92);
			c = II(c, d, a, b, x[k + 10], S43, 0xFFEFF47D);
			b = II(b, c, d, a, x[k + 1], S44, 0x85845DD1);
			a = II(a, b, c, d, x[k + 8], S41, 0x6FA87E4F);
			d = II(d, a, b, c, x[k + 15], S42, 0xFE2CE6E0);
			c = II(c, d, a, b, x[k + 6], S43, 0xA3014314);
			b = II(b, c, d, a, x[k + 13], S44, 0x4E0811A1);
			a = II(a, b, c, d, x[k + 4], S41, 0xF7537E82);
			d = II(d, a, b, c, x[k + 11], S42, 0xBD3AF235);
			c = II(c, d, a, b, x[k + 2], S43, 0x2AD7D2BB);
			b = II(b, c, d, a, x[k + 9], S44, 0xEB86D391);
			a = addUnsigned(a, AA);
			b = addUnsigned(b, BB);
			c = addUnsigned(c, CC);
			d = addUnsigned(d, DD);
		}
		var tempValue = wordToHex(a) + wordToHex(b) + wordToHex(c) + wordToHex(d);
		return tempValue.toLowerCase();
	}

})(window.MD5 = {});

(function(mui, owner) {
	var CONNECTION_TYPE = {};
	mui.plusReady(function() {
		CONNECTION_TYPE[plus.networkinfo.CONNECTION_UNKNOW] = "UNKNOW";
		CONNECTION_TYPE[plus.networkinfo.CONNECTION_NONE] = "NONE";
		CONNECTION_TYPE[plus.networkinfo.CONNECTION_ETHERNET] = "ETHERNET";
		CONNECTION_TYPE[plus.networkinfo.CONNECTION_WIFI] = "WIFI";
		CONNECTION_TYPE[plus.networkinfo.CONNECTION_CELL2G] = "2G";
		CONNECTION_TYPE[plus.networkinfo.CONNECTION_CELL3G] = "3G";
		CONNECTION_TYPE[plus.networkinfo.CONNECTION_CELL4G] = "4G";
	});

	/**
	 * 生成随机串
	 */
	owner.randomUUID = function() {
		var s = [];
		var hexDigits = "0123456789abcdef";
		for(var i = 0; i < 36; i++) {
			s[i] = hexDigits.substr(Math.floor(Math.random() * 0x10), 1);
		}
		s[14] = "4"; // bits 12-15 of the time_hi_and_version field to 0010
		s[19] = hexDigits.substr((s[19] & 0x3) | 0x8, 1); // bits 6-7 of the clock_seq_hi_and_reserved to 01
		s[8] = s[13] = s[18] = s[23] = "-";
		return s.join("");
	};

	owner.getGesturePassword = function() {
		return localStorage.getItem("$gesture_password");
	};

	/**
	 * 返回登录客户端保存的登录方式、账号、密码、登录状态、最后登录时间等
	 */
	owner.getLoginInfo = function() {
		return JSON.parse(localStorage.getItem("$login_info") || "{}");
	};

	owner.setLoginInfo = function(loginInfo) {
		localStorage.setItem("$login_info", JSON.stringify(loginInfo));
	};
	owner.getStaffInfo = function() {
		return JSON.parse(localStorage.getItem("$staff_info") || "{}");
	};
	owner.setStaffInfo = function(staffInfo) {
		localStorage.setItem("$staff_info", JSON.stringify(staffInfo));
	};
	owner.getAccountInfo = function() {
		return JSON.parse(localStorage.getItem("$account_info") || "{}");
	};
	owner.setAccountInfo = function(account_info) {
		localStorage.setItem("$account_info", JSON.stringify(account_info));
	};
	owner.localStorageClear = function() {
		//localStorage.clear();
	}

	owner.showWaiting = function() {

		var html = [];
		html.push("<div id=\"loading\"><div id=\"loading-center\"><div id=\"loading-center-absolute\">");
		html.push("<div class=\"object\" id=\"object_one\"></div>");
		html.push("<div class=\"object\" id=\"object_two\"></div>");
		html.push("<div class=\"object\" id=\"object_three\"></div>");
		html.push("<div class=\"object\" id=\"object_four\"></div>");
		html.push("<div class=\"object\" id=\"object_five\"></div>");
		html.push("<div class=\"object\" id=\"object_six\"></div>");
		html.push("<div class=\"object\" id=\"object_seven\"></div>");
		html.push("<div class=\"object\" id=\"object_eight\"></div>");
		html.push("</div></div></div>");
		$("body").append(html.join(""));
	};

	owner.fullZero = function(s, l) {
		s = new String(s);
		while(s.length < l) {
			s = "0" + s;
		}
		return s;
	};

	app.alert = function(msg, _callback) {
		mui.toast(msg);
		if(_callback != null && _callback != undefined) _callback();
	}

	app.isNull = function isNull(value) {
		if(value == null || value == "" || value == "undefined" || value == undefined || value == "null" || value == "(null)" || value == 'NULL' || typeof(value) == 'undefined') {
			return "";
		} else {
			return value;
		}
	}

	owner.getCommonParameter = function() {
		var d = new Date();

		var timestamp = d.getFullYear() + "-" + this.fullZero((d.getMonth() + 1), 2) + "-" + this.fullZero(d.getDate(), 2) + " " + this.fullZero(d.getHours(), 2) + ":" + this.fullZero(d.getMinutes(), 2) + ":" + this.fullZero(d.getSeconds(), 2);
		var parameter = {};
		parameter["v"] = encodeURIComponent("1.0");
		parameter["timestamp"] = encodeURIComponent(timestamp);
		parameter["app_key"] = encodeURIComponent("sdpt0.1");

		var staffInfo = app.getStaffInfo() || {};
		parameter["USER_ID"] = encodeURIComponent(staffInfo.USER_ID || "");
		parameter["TOKEN"] = encodeURIComponent(staffInfo.TOKEN || "");
		parameter["OEM_ID"] = encodeURIComponent(staffInfo.OEM_ID || "");
		
		var clientInfo = {};
		clientInfo["NET"] = CONNECTION_TYPE[plus.networkinfo.getCurrentType()];
		clientInfo["IMEI"] = encodeURIComponent(plus.device.uuid);
		clientInfo["TERMINAL_TYPE"] = plus.os.name;
		clientInfo["APP_VERSION"] = plus.runtime.version;
		clientInfo["DEVICE_MODEL"] = plus.device.model;
		clientInfo["DEVICE_VERSION"] = plus.os.version;
		parameter["client_info"] = encodeURIComponent(JSON.stringify(clientInfo));

		return parameter;
	};

	owner.sign = function(para) {
		var arr = [];
		for(var p in para) {
			arr.push(p);
		}
		arr.sort();
		var s = new String();
		for(var i = 0; i < arr.length; i++) {
			s = s + arr[i] + para[arr[i]];
		}
		s = "f092a10dc2399f56ddea88cdad3241bd" + s + "f092a10dc2399f56ddea88cdad3241bd";
		s = MD5.md5(s);
		para["sign"] = s.toUpperCase();
		return para;
	};
//	owner.sServerAddress = "http://192.168.1.30:8011"; 
//	owner.sServerAddress = "http://192.168.1.246:8011";
   //owner.sServerAddress = "http://120.76.217.191:58080"; 
 
     owner.sServerAddress = "http://192.168.1.148:8080"; 
//http://120.76.217.191:18088/CREDIT_REPAYMENT/AppDownload.html
//http://192.168.1.246:8011/CREDIT_REPAYMENT/AppDownload.html
	owner.getData = function(uri, parameter, _fun, loading) {

		var data = this.getCommonParameter();

		if(parameter) {
			for(var p in parameter) {
				data[p] = encodeURIComponent(parameter[p]);
			}
		}

		data = this.sign(data);
		
		console.log(JSON.stringify(data));
		mui.ajax({
			"type": "post",  
			"url": owner.sServerAddress + "/CreditCardAPI/api/rest" + uri,
			"dataType": "json",
			"data": data,
			"success": function(json) {
//				$("body").find("#loading").remove();
				if(json.RESULT_CODE == 0) {
					if(_fun != null && _fun != undefined) _fun(json);
				} else {
					$("#CHECKS").val('');
					var meg = json.RESULT_MESSAGE;
					if(meg == "验证码错误" || meg == "验证码超时，请重新获取") {
						$("#checkCodeImg").click();
					} else if(meg == "用户名或密码错误!") {
						firsts = false;
						$("#yzm").show();
						$("#checkCodeImg").click();
					}
					plus.nativeUI.closeWaiting();
					console.log(uri);
					console.log(JSON.stringify(json));
					app.alert(json.RESULT_MESSAGE);
				}
			},
			"error": function() {
				plus.nativeUI.closeWaiting();
				$("body").find("#loading").remove();
			}
		});
	};

	owner.openWindow = function(id, url, extras) {
		mui.openWindow({
			url: url,
			id: id,
			extras: arguments[2] || {},
			show: {
				autoShow: true,
				aniShow: "slide-in-right"
			}
		});

	};
	owner.isSetGesturePassword = function(flag) {
		if(flag) {
			localStorage.setItem("$is_gesture_password", flag);
		} else {
			return localStorage.getItem("$is_gesture_password");
		}

	}

	owner.setGesturePassword = function(pwd) {
		localStorage.setItem("$gesture_password", pwd);
	}

	owner.getGesturePassword = function() {
		return localStorage.getItem("$gesture_password");
	};

	// 下载wgt文件

	owner.downWgt = function(UPDATE_URL, UPDATE_TYPE) {
		if(plus.os.name == "Android") {
			plus.nativeUI.showWaiting("正在下载更新...");
			plus.downloader.createDownload(UPDATE_URL, {
				filename: "_doc/update/"
			}, function(d, status) {
				if(status == 200) {
					owner.installWgt(d.filename); // 安装wgt包
				} else {
					plus.nativeUI.alert("下载更新失败！");
				}
				plus.nativeUI.closeWaiting();
			}).start();
		} else if(plus.os.name == "iOS") {
			if(UPDATE_TYPE == 1) {
				plus.nativeUI.showWaiting("正在下载更新...");
				plus.downloader.createDownload(UPDATE_URL, {
					filename: "_doc/update/"
				}, function(d, status) {
					if(status == 200) {
						owner.installWgt(d.filename); // 安装wgt包  
					} else {
						plus.nativeUI.alert("下载更新失败！");
					}
					plus.nativeUI.closeWaiting();
				}).start();
			} else {
				var options = {
					method: "GET"
				};
				document.location.href = UPDATE_URL;
				plus.downloader.createDownload(UPDATE_URL, options);
			}
		}
	}
	// 更新应用资源

	owner.installWgt = function(path) {
		plus.nativeUI.showWaiting("正在安装应用...");
		plus.runtime.install(path, {}, function() {
			plus.nativeUI.closeWaiting();
			plus.nativeUI.alert("更新完成！", function() {
				plus.runtime.restart();
			});
		}, function(e) {
			plus.nativeUI.closeWaiting();
			plus.nativeUI.alert("安装失败[" + e.code + "]：" + e.message);
		});
	}

	owner.requestPost = function(url, params, showWait, ajaxSuccess, ajaxError) {
		var w;
		if(showWait === "true") {
			w = plus.nativeUI.showWaiting("请等待...");
		}
		var data = this.getCommonParameter();

		if(params) {
			for(var p in params) {
				data[p] = encodeURIComponent(params[p]);
			}
		}

		data = this.sign(data);
		var _f_fun = function(json) {
			if(showWait === "true") {
				w.close();
			}
			console.log("resultBean====" + JSON.stringify(json));
			if(parseInt(json.RESULT_CODE) != 0) {
				ajaxError(json.RESULT_MESSAGE);
			} else {
				ajaxSuccess(json);
			}
		}

		var xhr = new XMLHttpRequest();
		mui.ajaxSettings.beforeSend = function(xhr, setting) {
			console.log('params:::' + params);
			console.log('beforeSend:1::' + JSON.stringify(setting.url));
		};
		//设置全局complete
		mui.ajaxSettings.complete = function(xhr, status) {
			console.log('complete:::' + status);
		}
		mui.ajax(owner.sServerAddress + "/GOM_API/api/rest" + url, {
			data: data,
			dataType: 'json', //服务器返回json格式数据
			type: 'post', //HTTP请求类型
			timeout: 30000, //超时时间设置为30秒；
			success: _f_fun,
			error: function(xhr, type, errorThrown) {
				//异常处理；
				if(showWait === "true") {
					w.close();
				}
				mui.alert("网络异常，请与管理员联系，或稍后再试", '提示', function() {});
			}
		});
	}

}(mui, window.app = {}));

mui("body").on("tap", ".openWindow", function() {
	app.openWindow(this.id, this.id, JSON.parse($(this).attr("param") || "{}"));
});