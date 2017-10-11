## 移除响应头中的Server信息

如果使用的服务器是nginx，使用PHP代码`header_remove('Server');`移除是无效的，原因是nginx本身对这个字段做了操作；折中的办法是使用`header('Server:')`，将该值设置为空。

`header`和`header_remove`两个函数的使用方法，参考[http://php.net/manual/zh/function.header.php](http://php.net/manual/zh/function.header.php)和[http://php.net/manual/zh/function.header-remove.php](http://php.net/manual/zh/function.header-remove.php)

修改Server值的方法：

1. 在nginx配置中添加`server_tokens off;`，该配置只能隐藏nginx的版本号
2. 使用nginx插件，参考[How to disable Nginx server signature?](https://talk.plesk.com/threads/how-to-disable-nginx-server-signature.340216/)
3. 修改nginx源码，参考[How To Customize Your Nginx Server Name After Compiling From Source In CentOS](https://www.digitalocean.com/community/tutorials/how-to-customize-your-nginx-server-name-after-compiling-from-source-in-centos)

### 参考

- [How do you change the server header returned by nginx?](https://stackoverflow.com/questions/246227/how-do-you-change-the-server-header-returned-by-nginx)
- [How to disable Nginx server signature?](https://talk.plesk.com/threads/how-to-disable-nginx-server-signature.340216/)
- [How To Customize Your Nginx Server Name After Compiling From Source In CentOS](https://www.digitalocean.com/community/tutorials/how-to-customize-your-nginx-server-name-after-compiling-from-source-in-centos)