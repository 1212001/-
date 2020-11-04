## mybatis动态sql

### 一、if标签

在where条件里面，可以选择条件是否满足，而丰富查询条件

```xml
<if test="userName != null and userName != ''">
	and user_name=#{userName}
</if>
```

### 二、trim标签

在下文有多个 and，or 连接，使用trim标签可以去除多余的,<where>标签也可以

```xml
<trim prefixOverrides="AND | OR">
<if test="userName != null and userName != ''">
	AND user_name=#{userName}
</if>
</trim>
```

### 三、where标签

如果查询条件不满足，就不生成where

```xml
<where>
<trim prefixOverrides="AND | OR">
<if test="userName != null and userName != ''">
	AND user_name=#{userName}
</if>
</trim>
</where>
```

### 四、choose,when, otherwise

choose相当于 switch,when相当于case，当when标签中不满足的条件下，会执行otherwise标签中的内容

```xml
<choose>
<when>
<if test="userName != null and userName != ''">
	AND user_name=#{userName}
</if>
</when>
<otherwise>
	id=5
</otherwise>
</choose>
```

### 五、foreach

遍历的能力

当使用可迭代对象或者数组时，index 是当前迭代的序号，item 的值是本次迭代获取到的元素。当使用 Map 对象（或者 Map.Entry 对象的集合）时，index 是键，item 是值。open开始 separator分割 close结束,collection传递的类型。

```xml
<select id="selectPostIn" resultType="domain.blog.Post">
  SELECT *
  FROM POST P
  WHERE ID in
<foreach item="item" index="index" collection="list" open="(" separator="," close=")">
	#{item}
</foreach>
</select>
```

