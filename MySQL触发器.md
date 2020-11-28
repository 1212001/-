## MySQL触发器

### 定义

触发器是与表有关的数据库对象，在满足定义条件时触发，并执行触发器中定义的语句集合。触发器的这种特性可以协助应用在数据库端确保数据的完整性。

### 语法

```MySQL
CREATE TRIGGER trigger_name trigger_time trigger_event ON tb_name FOR EACH ROW trigger_stmt
trigger_name：触发器的名称
tirgger_time：触发时机，为BEFORE或者AFTER
trigger_event：触发事件，为INSERT、DELETE或者UPDATE
tb_name：表示建立触发器的表明，就是在哪张表上建立触发器
trigger_stmt：触发器的程序体，可以是一条SQL语句或者是用BEGIN和END包含的多条语句
所以可以说MySQL创建以下六种触发器：
BEFORE INSERT,BEFORE DELETE,BEFORE UPDATE
AFTER INSERT,AFTER DELETE,AFTER UPDATE
```

其中，触发器名参数指要创建的触发器的名字

BEFORE和AFTER参数指定了触发执行的时间，在事件之前或是之后

FOR EACH ROW表示任何一条记录上的操作满足触发事件都会触发该触发器

### 示例

```mysql
CREATE TRIGGER test_trigger BEFORE  INSERT
ON test FOR EACH ROW 
BEGIN
	INSERT INTO targger_text (id,msg) VALUES (2,"我是test表，insert之后发生的触发器");
END

#test_trigger 触发器的名字
#BEFORE  指定了触发执行的时间
#INSERT 插入时触发
#test	哪个表的触发器

```

### 结构

```mysql
CREATE TRIGGER 触发器名 BEFORE|AFTER 触发事件
ON 表名 FOR EACH ROW
BEGIN
    执行语句列表
END
```

### 注意事项

在BEGIN...END语句中也可以定义变量，但是只能在BEGIN...END内部使用：

```sql
DECLARE var_name var_type [DEFAULT value] #定义变量，可指定默认值
SET var_name = value  #给变量赋值
```

### 如果出现乱码

```sql
DELIMITER $
CREATE TRIGGER user_log AFTER INSERT ON users FOR EACH ROW
BEGIN
DECLARE s1 VARCHAR(40)character set utf8;
DECLARE s2 VARCHAR(20) character set utf8;#后面发现中文字符编码出现乱码，这里设置字符集
SET s2 = " is created";
SET s1 = CONCAT(NEW.name,s2);     #函数CONCAT可以将字符串连接
INSERT INTO logs(log) values(s1);
END $
DELIMITER ;
```

