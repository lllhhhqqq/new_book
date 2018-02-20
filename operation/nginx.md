## Nginx

### 使用

- 拒绝某个User Agent的请求：

```nginx
# 这个配置需要放在server或location模块内
if ($http_user_agent ~* "xxx") {
	return 403;
}
```

### 推荐阅读

- [Nginx开发从入门到精通](http://tengine.taobao.org/book/)
- [开源软件架构-Nginx](http://www.ituring.com.cn/article/4436)
- [Nginx模块开发入门](http://blog.codinglabs.org/articles/intro-of-nginx-module-development.html)
- [服务器系统架构分析日志](http://www.sudone.com/)
- [Nginx Resources](https://github.com/fcambus/nginx-resources)
