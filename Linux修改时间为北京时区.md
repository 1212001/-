## Linux修改时间为北京时区

### tzselect 选择时区

#### 然后选择北京时区（按照 前面的数标进行逐步选择）

### 

### 选择完后会有如下提示信息

```shell
You can make this change permanent for yourself by appending the line
	TZ='Asia/Shanghai'; export TZ
to the file '.profile' in your home directory; then log out and log in again.
```

添加  TZ='Asia/Shanghai'; export TZ  这行到 /etc/profile

然后运行 source /etc/profile  立即使用





### timedatectl （查看当前时区）

