 //新增
   window.onload=function(){
        document.documentElement.style.fontSize = document.documentElement.clientWidth/ 7.5 + 'px';
        
    }
	
	/**
	 * 关闭当前窗口
	 */
	function back_window() {
    	plus.webview.currentWebview().close();
    }
	/**
	 * 是否激活
	 */
	function isValidCert() {
		var isFlag = 1;
		var hasCode = app.getAccountInfo().HAS_CODE;
		if(hasCode != 0) {
			isFlag = 2; // 未激活，去激活
			return isFlag;
		}
		var isRealname = app.getAccountInfo().IS_REALNAME;
		if(isRealname != 1) {
			isFlag = 3; // 未实名，去实名认证
			return isFlag;
		}
		var hasPayPwd = app.getAccountInfo().HAS_PAY_PWD;
		if (hasPayPwd != 1) {
			isFlag = 4; // 未设置支付密码，去设置
			return isFlag; // 
		}
		return isFlag;
	}
	
	/**
	 * 去激活
	 */
	function goActive() {
		var btnArray = ['取消', '去激活'];
		mui.confirm('您还未激活，请先去激活' , '温馨提示', btnArray, function(e) {
            if (e.index == 1) { 
               app.openWindow('activation.html', 'activation.html'); //未激活
            }
        });
	}
	
	/**
	 * 去实名
	 */
	function goCert() {
		var btnArray = ['取消', '去认证'];
		mui.confirm('您还未进行身份认证，请先去认证' , '温馨提示', btnArray, function(e) {
            if (e.index == 1) { 
               app.openWindow('realnameNo.html', 'realnameNo.html'); //未实名
            }
        });
	}
	
	/**
	 * 设置支付密码
	 */
	function goSetPayPwd() {
		var btnArray = ['取消', '去设置'];
		mui.confirm('您还未设置支付密码，请先去设置' , '温馨提示', btnArray, function(e) {
            if (e.index == 1) { 
               app.openWindow('setpaypassword.html', 'setpaypassword.html'); //未设置支付密码
            }
        });
	}
