# xy10.1
标签文档

## 说明页调用方法

调用语法 ： 
```html

{% set test = app('section').id(28680) %} 
<li>
 <a href="{{ test.content.link }}" >
    <h3>{{ test.title}}</h3>
    <img src="{{ test.content.img }} " alt="{{ test.title}">
    <h4>{{ test.content.summary |truncate(200,'...') }} </h4>
    <h5>{{ test.content.content | raw }}</h5>
 </a>
</li>

```

参数说明：

```php

{% set test = app('section').id(28680) %} 
{{ test.title}                   // 调用名称
{{ test.content.img }}           // 调用图片
{{ test.content.link }}                   // 调用链接
{{ test.content.summary }}       // 调用描述
{{ test.content.content | raw }}  // 调用内容

```


注意：  
    语法中 <font color=#FF0000 >test</font> 参数 可以自定义命名




## 广告图调用方法

调用语法 ：

```html

{% for value in app('section').id('45528') %}
<li>
    <a href="{{ value.url }} " >
        <img src="{{ value.img }}" alt="{{ value.name }}">
        <h3>{{ value.name }}</h3>
        <p>{{ value.summary }}</p>
    </a>
</li>
{% endfor %}  

<!--单个广告图调取-->
<img src="{{ app('section').id('45528')[0].img }}" alt="{{ app('section').id('45528')[0].name }}">

```  




参数说明：
```php
{% for value in app('section').id('45528') %}

{{ value.img }}           // 调用图片路径
{{ value.name }}          // 调用标题
{{ value.url }}           // 调用链接
{{ value.summary }}      // 调用描述

{% endfor %}  
```  

注意：
通过<font color=#FF0000 >id</font>调取不同的广告图分类





## 导航调用方法

调用语法 ：
```php
{% set nav = app('section').id(62630) %}
{% if nav is not empty %}
    <ul class="x-menu clearfix">
        {% for first in nav %}
            <li>
                <a href="{{ first.url }}" {% if first.target == '_blank' %}target='_blank'{% endif %}>{{ first.name }}{% if first.child is not empty %}<span class="creat"></span>{% endif %}</a>
                {% if first.content_model is not empty %}
                    {% if first.content_model == 'page' %}
                        {% if app('page').lists(first.content_model_id) is empty %}{% else %}
                            <ul class="x-sub-menu">
                                {% for second in app('page').lists(first.content_model_id)%}
                                    <li><a href="{{ second.url }}" {% if second.target == '_blank' %}target='_blank'{% endif %}>{{ second.title }}</a></li>
                                {% endfor %}
                            </ul>
                        {% endif %}
                    {% else %}
                        {% if app('category').lists(first.content_model, first.content_model_id) is empty %}{% else %}
                            <ul class="x-sub-menu">
                                {% for second in app('category').lists(first.content_model, first.content_model_id) %}
                                    <li><a href="{{ second.url }}" {% if second.target == '_blank' %}target='_blank'{% endif %}>{{ second.cname }}</a></li>
                                {% endfor %}
                            </ul>
                        {% endif %}
                    {% endif %}
                {% else %}
                    {% if first.child is not empty %}
                        <ul class="x-sub-menu">
                            {% for second in first.child %}
                                <li><a href="{{ second.url }}" {% if second.target == '_blank' %}target='_blank'{% endif %}>{{ second.name }}</a></li>
                            {% endfor %}
                        </ul>
                    {% endif %}
                {% endif %}
            </li>
        {% endfor %}
    </ul>
{% endif %}
```

注意：  
{% set nav = app('section').id(62630) %} 语法中set后面的参数 nav 可以自定义  但是下面的变量要一致
如下图中的三个参数要一致   
<img :src="$withBase('/imgs/nav.png')" >



推荐使用通过调取代码片段进行书写导航


<font color=#FF0000 >推荐语法：</font>
```php
{% set nav = app('section').id(62630) %}
{% snippet "db_head" data=nav %}
``` 

在snippets文件夹下 新建一个导航名称，然后把原来 snippets文件下的 nav.html 文件内容全部复制到新建的 导航名称调取 


## 图文调用方法

调用语法 ：


```html
{% for value in app('section').id('20066') %}
<li>
    <a href="{{ value.url }} " >
        <img src="{{ value.img[0] }}" alt="{{ value.name }}">
        <h3>{{ value.name }}</h3>
        <h4>{{ value.fname }}</h4>
        <dd>{{ value.text1 }}</dd>
        <dt>{{ value.text2 }}</dt>
        <dl>{{ value.text3 }}</dl>
        <p>{{ value.summary }}</p>
    </a>
</li>
{% endfor %}
```

参数说明：

```php
{% for value in app('section').id('20066') %}

{{ value.name }}     // 调用主标题
{{ value.fname }}    // 调用副标题
{{ value.text1 }}    // 调用文字1
{{ value.text2 }}    // 调用文字2
{{ value.text3 }}    // 调用文字3
{{ value.img[0] }}      // 调用多图路径      第一个默认为0 第二个为1 以此类推...     
{{ value.url }}      // 调用链接地址     
{{ value.summary }}  // 调用备注

{% endfor %}
```


延伸：通过增加参数达到遍历循环

```php
{% for key,value in app('section').id('20066') %}

<li class="<!--{$key/4}-->">

</li>

{% endfor %}
```


## 模块（产品、新闻、案例....）调用方法

产品、案例

```html

{% for value in app('section').id('47248') %}

<li class="news_list"> 
    <a href="{{ value.url }}">
        <img src="{{ app('content').thumb(value.uploadfiles[0],400,400) }}" alt="{{ value.title }}">
        <h3>{{ value.title }}</h3>
        <h4>{{ value.description }} </h4>
        <h5>{{ value.summary }}</h5>
        <p>{{ value.timeline|date("Y-m-d") }} </p>
        <span>{{ value.author }}</span>
    </a>
</li>

{% endfor %}

```

参数说明：

```php

{% for value in app('section').id('47248') %}
    {{ value.url }}     // 调用链接地址     
    {{ value.title }}   // 调用标题
    {{ value.uploadfiles }}    // 调用原图
    {{ app('content').thumb(value.uploadfiles[0],400,400) }}   // 调用缩略图
    {{ value.description }}    // 调用描述
    {{ value.summary }}  // 调用内容
    {{ value.timeline|date("Y-m-d") }}  // 调用时间
    {{ value.author }}   // 调用新闻作者

{% endfor %}

```



## 默认调取产品、新闻、案例...

语法：

```html

<ul class="product_list">
    {% for value in news.show(0,6,0,1) %}
    <li>
        <a href="{{ value.url }}">
            <img src="{{ app('content').thumb(value.uploadfiles[0],400,400) }}" alt="{{ value.title }}">
            <h3>{{ value.title }}</h3>
            <h4>{{ value.description }} </h4>
            <h5>{{ value.summary }}</h5>
            <p>{{ value.timeline|date("Y-m-d") }} </p>
            <span>{{ value.author }}</span>
        </a>
    </li>
    {% endfor %}
</ul>

```

参数说明：

```php
// 产品调用
{% for value in products.show(0,6,0,1) %}
    ...
{% endfor %}

 // 案例调用
{% for value in cases.show(0,6,0,1) %}
    ...
{% endfor %}

 // 新闻调用
{% for value in news.show(0,6,0,1) %}
    ...
{% endfor %}


// (0,6,0,1)
//第一个参数 调取分类
//第二个参数 调取显示个数
//第三个参数 0（默认），1（推荐），2（置顶）
//第四个参数 0（正序），1（倒序）
```


## 产品、新闻、案例等分类的调取

语法：

```html

{% for value in app('category').lists('news') %}
    <li>
        <a href="{{ value.url }}">
            <h3>{{ value.cname }}</h3>
            <img src="{{ value.img }}" alt="{{ value.cname }}">
            <img src="{{ value.banner }}" alt="{{ value.cname }}">
            <h4>{{ value.intro }}</h4>
        </a>
    </li>
{% endfor %}
 ```   

 参数说明：

 ```php

{% for value in app('category').lists('news') %}

    {{ value.url }}    // 调取链接地址
    {{ value.cname }}  // 调取分类名称
    {{ value.img }}    // 调取缩略图
    {{ value.banner }} // 调取栏目图片（一般为当前分类banner图）
    {{ value.intro }}  // 调取缩略图备注

{% endfor %}
 ```  


 ## 拆分

 ```php
{% block menus %}
{% snippet "category" data=app('category').lists('product',first_cid) %}
{% endblock %}
 ``` 


## 友情链接调取方法

语法：

```html

<!--0 调取所有  1 调取文字链接 2 调取图片链接-->
{% for value in app('links').lists(1) %}
   <a href='{{ value.linkurl }}' target='_blank'>
        <h3>{{ value.linktitle }}</h3>
        <img src="{{ value.logoimg }}">
    </a>
{% endfor %}

 ``` 


参数说明：


 ```php

{% for value in app('links').lists(1) %}

    {{ value.linkurl }}    // 调取网站URL
    {{ value.linktitle }}  // 调取网站名称
    {{ value.logoimg }}    // 调取LOGO图片
    {{ value.intro }}     // 调取网站描述

{% endfor %}

 ```

商盟链接调取：

```html

	{% for value in  app('links').league %}
		<a href='{{ value.linkurl }}' target='_blank'>
			{{ value.linktitle }}
		</a>
	{% endfor %}

 ``` 

## 单页判断

 ```php

 {% if page.id ==96 %}
 
 {{ page.content | raw }}  

 {% else if page.id ==99 %}

    {{ page.content | raw }}  

   {% else %}

       {{ page.content | raw }}  
       
    {% endif %}

```
