# vue-response
vue请求响应拦截

//  VueResource
下边代码添加在main.js中
Vue.http.interceptors.push((request, next) => {
　console.log(this)//此处this为请求所在页面的Vue实例
  // continue to next interceptor
　　next((response) => {//在响应之后传给then之前对response进行修改和逻辑判断。对于token时候已过期的判断，就添加在此处，页面中任何一次http请求都会先调用此处方法

    　　response.body = '...';
　　　　return response;

  });

});


// vue-axios
// 添加一个响应拦截器
axios.interceptors.response.use(
  function (response) {
      if (response.data.resultCode === "4006") {
        Message({
          type: "error",
          message: "登录已失效，请重新登录！",
          duration: 3000,
        });
        localStorage.clear();
        router.push("/login");
      }
    return response;
  },
  function (error) {
    store.state.loading = false;
    if (error.response) {
      // if(error.response.data.status==500){

      // }else{
      Message({
        type: "error",
        message: error.response.data.resultMessage,
        duration: 3000,
      });
      // }
    } else {
      Message({
        type: "error",
        message: "请求出现异常",
        duration: 3000,
      });
    }
    // return Promise.reject(error);
  }
);
