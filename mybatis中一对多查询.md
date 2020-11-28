## mybatis中一对多查询

### 一、例，动态与评论，一个动态有多个评论

#### 1、在动态实体中生成评论列表

```java
package com.liboy.entity;
import lombok.Data;
import javax.persistence.Table;
import java.util.List;

/**
* 描述：镜像模型
* @author DengLi
* @date 2020/10/11
*/
@Data
@Table(name = "dynamic")
public class DynamicEntity {
    private int id;
    private String userId;
    /**
    *标题
    */
    private String title;
    /**
    *描述
    */
    private String describle;
    /**
    *内容
    */
    private String content;

    /**
     * 评论
     */
    private List<DynamicTableEntity> dynamicTableEntityList;

}

```

#### 2、在Dynamic.xml中制造resultMap

##### 在这里面生成自己想拿到的字段

```xml
<!--    发布的动态，与评论相关联 ， look_dynamic_id 与 id -->
    <resultMap id="dynamicAndTableMap" type="com.liboy.entity.DynamicEntity">
        <result property="userId" column="user_id"/>
        <result property="title" column="title"/>
        <result property="describle" column="describle"/>
        <result property="content" column="content"/>
        <collection property="dynamicTableEntityList" ofType="com.liboy.entity.DynamicTableEntity">
            <result property="lookDynamicId" column="look_dynamic_id"/>
            <result property="userDoId" column="user_do_id"/>
            <result property="userText" column="user_text"/>
            <result property="doTime" column="do_time"/>
        </collection>
    </resultMap>
```

##### 联合查询语句

```xml
<!--    一对多，动态对评论-->
    <select id="getDynamicAndTable" resultMap="dynamicAndTableMap">
        SELECT a.user_id,a.title,a.describle,a.content,b.user_do_id,b.look_dynamic_id,b.user_text,b.do_time
        FROM dynamic a  left JOIN dynamic_table b ON a.id=b.look_dynamic_id
        <where>
            a.id = #{id}
        </where>
    </select>
```

#### 3、测试结果

```json
{
  "msg": "返回成功",
  "status": 200,
  "res": true,
  "errorCode": null,
  "data": [
    {
      "id": 0,
      "userId": "14",
      "title": "abc",
      "describle": "abc",
      "content": "<p>abc</p>",
      "dynamicTableEntityList": [
        {
          "id": 0,
          "lookDynamicId": 8,
          "userDoId": "as",
          "userText": "好帅",
          "doTime": "2020-10-21 19:44:36"
        },
        {
          "id": 0,
          "lookDynamicId": 8,
          "userDoId": "lijun",
          "userText": "还行",
          "doTime": "2020-10-21 22:02:44"
        }
      ]
    }
  ]
}
```

