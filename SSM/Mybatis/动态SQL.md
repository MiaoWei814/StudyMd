# 1.基础标签

## 1.1 SQL标签

经常用于定义常用的SQL语句片段,俗称SQL常量

```mysql
<sql id="selectParam">    id, username, password, sex, birthday, address </sql>
```

## 1.2 include标签

用于跟常量搭配一起使用的,它定义我来搬。

```mysql
<select>
    select <include refid="selectParam"></include>
    from user
</select>
```

## 1.3 if标签

我们在做业务场景时必用到的标签。要注意如果是Integer类型如果为0那么就会默认为''

```mysql
<if test="item != null and item != ''"></if>

<if test="item != null"></if>
<if test="item != null and item != '' or item == 0"></if>
```

## 1.4 别名

经常使用的类型可以定义别名，方便使用，mybatis也注册了很多别名方便我们使用

```mysql
<typeAliases>
     <typeAlias type="com.it.bean.User" alias="User"/>
</typeAliases>
```

# 2.foreach标签

- 属性:
  -  **collection** :循环的集合。传的是集合为list，数组为array, 如果是map为java.util.HashMap
  - **item**:循环的key
  - **index** :循环的下表顺序
  - **open** :循环的开头
  - **close** :循环结束
  - **separator** :循环的分隔符

demo:

<!-- Mysql的批量插入，主键自增 -->

```mysql
<insert id="insertBatchMysql" parameterType="list">
    insert into question_answer ( <include refid="base_column" /> ) 
    values 
        <foreach collection="list" item="item" open="(" separator="union all" close=")">
            #{item.id}, #{item.questionId}, #{item.answer}
        </foreach>
</insert>
```

# 3.where标签

where用来去掉多条件查询时，开头多余的and,等同于1=1

```sql
<select id="selectUserList" parameterType="com.it.bean.User" resultType="com.it.bean.User">
    <!-- 引用Sql片段 -->
    select <include refid="selectParam"> from user u
    <where>
        <!--where 可以自动去掉条件中的第一个and-->
        <if test="id != null">
            and u.id = #{id}
        </if>
        <if test="name != null and name != ''">
            and u.name = #{name}
        </if>
    </where>
</select>
```

# 4.set标签

set是mybatis提供的一个智能标记，当在update语句中使用if标签时，如果前面的if没有执行，则或导致逗号多余错误。使用set标签可以将动态的配置SET 关键字，和剔除追加到条件末尾的任何不相关的逗号。
没有使用if标签时，如果有一个参数为null，都会导致错误，如下示例：

```mysql
    <update id="updateUser" parameterType="com.it.bean.user">
        update user
        <set>
            <if test="username != null and username != ''">
                username = #{username},
            </if>
            <if test="sex != null and sex == 0 or sex == 1">
                sex = #{sex},
            </if>
            <if test="birthday != null ">  
                birthday = #{birthday},
            </if >  
            <if test="address != null and address != ''">
                address = #{address},
            </if>
            <if test="lastModifiedBy != null and lastModifiedBy != ''">
                last_modified_by = #{lastModifiedBy},
                last_modified_date = SYSDATE,
            </if>
        </set>
        <where>
            id = #{id}
        </where>
    </update>
```

# 4.trim标签

trim标记是一个格式化的标记，可以完成set或者是where标记的功能

- 标签属性
  - **prefix**、**suffix** 示再trim标签包裹部分的前面或后面添加内容（注意：是没有prefixOverrides，suffixOverrides的情况下）
  - **prefixOverrides**、**suffixOverrides** 表示覆盖内容，如果只有这两个属性表示删除内容

```mysql
<update id="test" parameterType="com.it.bean.User">
    update user
        <!-- 开头加上set，结尾去除最后一个逗号 -->
        <trim prefix="set" suffixOverrides=",">
            <if test="username!=null and username != ''">
                name= #{username},
            </if>

            <if test="password!=null and password != ''">
                password= #{password},
            </if>

        </trim>
        <where>
            id = #{id}
        </where>
    </update>

```

# 5.choose、when、otherwise标签

有时候我们并不想应用所有的条件，而只是想从多个选项中选择一个。MyBatis提供了choose 元素，按顺序判断when中的条件出否成立，**如果有一个成立，则choose结束**。当choose中所有when的条件都不满则时，则执行 otherwise中的sql。类似于Java 的switch 语句，choose为switch，when为case，otherwise则为default。if是与(and)的关系，而choose是或（or）的关系

```mysql
<select id="getUserList" resultType="com.it.bean.User" parameterType="com.it.bean.User">  
    SELECT <include refid="resultParam"></include> FROM User u   
    <where>  
        <choose>  
            <when test="username !=null and username != ''">  
                u.username LIKE CONCAT(CONCAT('%', #{username}),'%')  
            </when >  
            <when test="sex != null">  
                AND u.sex = #{sex}  
            </when >  
            <when test="birthday != null ">  
                AND u.birthday = #{birthday}  
            </when >  
            <otherwise>
                and u.id=1
            </otherwise>  
        </choose>  
    </where>    
</select>  
```

# 6. `<![CDATA[]]>`标签

在使用mybatis 时我们sql是写在xml 映射文件中，如果写的sql中有一些特殊的字符的话，在解析xml文件的时候会被转义，但我们不希望他被转义，所以我们要使用`<![CDATA[ ]]>`来解决。

如:

```mysql
select
        id as 'id',
        `title` as 'title',
        work_command as 'command',
        case when
                `status`=4 then 1
                else 0 end as 'status'
from
        yq_item
where
        `type`=#{itemType} and report_time <![CDATA[ <= ]]> #{endTime} and report_time <![CDATA[ >= ]]> #{startTime} and `status`!=-1
```

对于mybatis特殊转义还有另一种写法:

1. ```xml
   原符号       <        <=      >       >=       &        '        "
   ```

   

2. ```xml
   1. 替换符号    &lt;    &lt;=   &gt;    &gt;=   &amp;   &apos;  &quot;
   ```

# 7.bind标签

bind标签可以使用OGNL表达式创建一个变量并将其绑定到上下文中。使用bind标签拼接字符串不仅可以避免因更换数据库而修改SQL，也能预防SQL注入。

`<bind>`属性:

	- name:为绑定到上下文的变量名
	- value:为OGNL表达式

```mysql
<!--原代码-->
    <if test="username!=null and userName!=''">
        and user_name like concat('%',#{userName},'%')
    </if>
<!--改进后代码-->
    <if test="username!=null and userName!=''">
        <bind name="userNameLike" value="'%'+userName+'%'"/>
        and user_name like #{userNameLike}
    </if>
```

其中value参数我做了以下测试都可以:

传入JAVABean:

```java
Post post = new Post();
post.setTitle("y");
return yqItemMapper.select(post);

xml:
<select id="select" resultType="com.abitware.project.module.domain.User">
        <bind name="name" value="'%'+title+'%'"/>
        select u.id,u.mobile,u.username as 'username'from `user` u where username like #{name}
</select>
    //这里title为对象参数
```

传入单个参数

```java
 @Override
    public List<User> demo() {
        String x="y";
        return yqItemMapper.select(x);
    }

xml:
<select id="select" resultType="com.abitware.project.module.domain.User">
        <bind name="name" value="'%'+x+'%'"/>
        select u.id,u.mobile,u.username as 'username'from `user` u where username like #{name}
</select>
    //单个参数直接取就可以了
```

