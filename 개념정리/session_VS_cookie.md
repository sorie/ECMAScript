
### 차이점
쿠키와 세션은 비슷한 역할을 하며, 동작 원리도 비슷하다. 그 이유는 세션도 결국 쿠키를 사용하기 때문이다. 큰차이점은 사용자의 정보가 저장되는 위치이다.
쿠키는 서버의 자원을 전혀 사용하지 않으며, 세션은 서버의 자원을 사용한다.

### 보안
쿠키는 클라이언트 로컬에 저장되기 때문에 변질되거나 request에서 스니핑 당할 우려가 있어서 보안에 취약하지만
세션은 쿠키를 이용해서 session id만 저장하고 그것으로 구분하여 서버에서 처리하기 때문에 비교적 보안성이 높다. 

### 저장 기간/기준
라이프 사이클은 쿠키도 만료기간이 있지만 파일로 저장되기 때문에 브라우저를 종료해도 정보가 유지될 수 있다.
만료기간을 따로 지정해 쿠키를 삭제할 때까지 유지할 수 있다. 
세션도 만료기간을 정할 수는 잇지만, 브라우저가 종료되면 만료기간에 상관없이 삭제된다. 

<pre>
<code>
// es6 script
const userCookie = UserCookieManager?.getInstance();
const { regist_dt, service_type } = response.message[0];
userCookie.setAll(mailValue,pwd,regist_dt,service_type);

// classes
class UserCookieManager {
	static #userInfo = new UserCookieManager();

	static getInstance() {
		if(this.#userInfo === null) {
			this.#userInfo = new UserCookieManager();
			return this.#userInfo;
		} else {
			return this.#userInfo;
		}
	}
	constructor() {
		this.u_email = this.getUserCookie('u_email');
		this.u_pwd = this.getUserCookie('u_pwd');
		this.reg_dt = this.getUserCookie('reg_dt');
		this.service_type = this.getUserCookie('service_type');
	}

	getEmail(){
		return this.u_email;
	}
	setAll(email,password,date,st){
		this.u_email = email;
		this.u_pwd = password;
		this.reg_dt = date;
		this.service_type = st;
		this.setUserCookie();
	}
	setEmail(email) {
		this.u_email = email;
		this.setUserCookie();
	}
	setPassword(pwd) {
		this.u_pwd = pwd;
		this.setUserCookie();
	}
	setRegistDate(date) {
		this.reg_dt = date;
		this.setUserCookie();
	}
	setServiceType(st) {
		this.service_type = st;
		this.setUserCookie();
	}

	setUserCookie() {
		var date = new Date();
		date.setTime(date.getTime() + (7 * 24 * 60 * 60 * 1000)); //7 day
		var expires = "; expires=" + date.toUTCString();
		document.cookie = "usr=" + (JSON.stringify(this) || "") + expires + "; path=/";
	}
	getUserCookie(key) {
		var name = "usr=";
		var ca = document.cookie.split(';');
		for (var i = 0; i < ca.length; i++) {
			var c = ca[i].trim();
			if (c.indexOf(name) == 0) {
				var u_item = c.substring(name.length, c.length);
				return JSON.parse(u_item)[key];
			}
		}
		return null
	}
}
</code>
</pre>

### 그외
속도 면에서 쿠키가 더 우수하다. 세션은 정보가 서버에 있기 때문에 비교적 느리다.

### 캐시
캐시는 웹 페이지 요소를 저장하기 위한 임시 저장소이고, 쿠키/세션은 정보를 저장하기 위해 사용된다. 
캐시는 웹 페이지를 빠르게 렌더링 할 수 있도록 도와주고, 쿠키/세션은 사용자의 인증을 도와준다.

캐시는 이미지, 비디오, 오디오, css, js 파일 등 데이터나 값을 미리 복사해 놓는 리소스 파일들의 임시 저장소이다.
저장 공간이 작고 비용이 비싼 대신 빠른 성능을 제공한다. 
같은 웹 페이지에 접속할 때 사용자의 PC에서 로드하므로 서버를 거치지 않아도 된다. 
이전에 사용된 데이터가 다시 사용될 가능성이 많으면 캐시 서버에 있는 데이터를 사용한다.
다시 사용될 가능성이 있는 데이터들이 빠르게 접근할 수 있다.


