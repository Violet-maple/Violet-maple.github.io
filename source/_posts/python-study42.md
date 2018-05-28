---
title: python-study42
date: 2018-05-26 09:51:27
tags:
---

## 1. 项目原型设计:

### 1-1. 专业工具-画图

#### 1-1-1. Axure RP

  - 线框图
  - 高保真原型

<!--more-->

#### 1-1-2. Sketch 

#### 1-1-3. Breifs

### 1-2. 非专业工具-业余

#### 1-2-1. Mockups

#### 1-2-2. Mockplus

## 2. 跨站身份伪造/跨站攻击:

### 2-1. 利用浏览器存储的cookie 中的用户身份标识冒充用户执行操作

- 在浏览器里 --> 第一次访问网站  

- 然后就在浏览器里就生成了一个**cookie** (*身份标识*)

- 再次访问网站 --> 网站就会确认cookie 就知道是这个浏览器来访问 (*可以免输入**admin**密码*)

- 防范网站身份伪造最佳的做法就是在表单中放置随机令牌:

- 初次之外,通过设置令牌还可以防范表单的重复提交级重放攻击

- 在表单里放入令牌:

  ```html
  {% csrf_token %}  # 随机生成令牌
  ```

- 在源代码里生成这样的 value值 - 是随机的

  <input type='hidden' name='csrfmiddlewaretoken' value='0BbVp6MY7fzc2GD5wMvrSWfJ9RHXArOJ1jChiW9Kvf5ejBKc4GX7QseKK84OmDlA' />   

```python
# 可视化工具 -- 关于内存
http://pythontutor.com

import  copy
list1 = [1, 2, 3, [1, 2, 3], {'a': 1, 'b': 2}]   # 列表里引用列表 和字典
list2 = list1                                    # 增加一个引用
list3 = list1[:]                                 # 切片复制(浅复制 - 列表里的列表,字典引用没有复制) 
list4 = copy.copy(list1)                         # 浅复制
list5 = copy.deepcopy(list1)                     # 深度复制  列表里的列表也复制了
```

### 2-2. 第一个参数是要转换成JSON格式(序列化)的对象

- encoder参数要制定完成自定义对象序列化的编码器(JSONEncoder的子类型)
- safe 参数的值如果为True 那么传入第一个参数只能是字典,如果是False 传入什么格式都行

```python
# return HttpResponse(json.dumps(record_list), content_type='application/json; charset=utf-8')
# return JsonResponse(record_list, encoder=CarRecordEncoder, safe=False)
```

- 序列化/串行化/腌咸菜/冻结  - 把对象按照某种方式处理成字节或者字符的序列
- 反序列化/反串行化/解冻     - 把字符或者字节的序列重新还原成对象

### 2-3. python 实现 序列化和反序列化 的工具模块  

- json / pickle / shelve  
- 第三方的  jsonpickle  (要安装pip安装就可以了)
- django 自带的序列化 serialize('json', object)
- pip show 查第三方的模块
- f.is_valid 是否有效 -- 判断

最后又什么不清楚就      [百度一下](https://www.baidu.com)    

