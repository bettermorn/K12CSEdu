# 高中生学习Django完整路线图

## 一、学习前的准备工作

### 1. 心态建设
- 强调编程是"搭积木"，Django只是帮你更快搭出网站的工具
- 允许犯错，报错信息是"提示"不是"惩罚"
- 建议每天学习30-60分钟，保持规律比长时间突击更有效

### 2. 环境准备
```bash
# 检查Python版本（建议3.9+）
python --version

# 安装Django
pip install django

# 验证安装
python -m django --version
```



## 二、第一阶段：补充Web基础知识（3-5天）

在学Django之前，必须先理解网站是怎么工作的，否则学Django会很懵。

### 核心概念（用生活化比喻理解）
1. **客户端-服务器模型**：把浏览器比作"顾客"，服务器比作"餐厅厨房"
2. **HTTP请求/响应**：类比"点菜"和"上菜"的过程
3. **URL的作用**：像"地址"，告诉浏览器去哪里找东西

### 需要了解的HTML/CSS基础
不需要精通，但要认识：
```html




    
标题

    
这是一段文字




```

**实践任务**：用记事本写一个简单的HTML文件，双击用浏览器打开，建立"代码变成网页"的直观感受。



## 三、第二阶段：Django基础概念（1周）

根据Django的官方文档，系统学习基础概念。**切记不能碎片化学习**。

文档汇总
- https://docs.djangoproject.com/en/6.1/

快速学习
- https://docs.djangoproject.com/en/6.1/intro/tutorial01/



### 1. 理解MVT架构（Django的核心思想）
用"餐厅"比喻理解：
- **Model（模型）**：仓库管理员，负责存取食材（数据）
- **View（视图）**：厨师，负责处理订单做菜（业务逻辑）
- **Template（模板）**：菜单/摆盘，负责好看地呈现给顾客（页面展示）

### 2. 创建第一个项目
```bash
# 创建项目
django-admin startproject myfirstsite
cd myfirstsite

# 启动服务器
python manage.py runserver
```

打开浏览器访问 `http://127.0.0.1:8000`，看到Django欢迎页面时，**一定要庆祝这个里程碑！**

### 3. 理解项目文件结构
```
myfirstsite/
├── manage.py          # 项目的"遥控器"
├── myfirstsite/
│   ├── settings.py    # 项目设置
│   ├── urls.py        # 网址路由表
│   └── wsgi.py
```



## 四、第三阶段：创建第一个App（1周）
参考官方文档  Writing your first Django app, part 1  https://docs.djangoproject.com/en/6.1/intro/tutorial01/

### 1. 创建应用
```bash
python manage.py startapp blog
```

### 2. 编写第一个视图（View）
```python
# blog/views.py
from django.http import HttpResponse

def home(request):
    return HttpResponse("你好，这是我的第一个Django网页！")
```

### 3. 配置URL路由
```python
# blog/urls.py（需要新建）
from django.urls import path
from . import views

urlpatterns = [
    path('', views.home, name='home'),
]
```

```python
# myfirstsite/urls.py（修改主路由）
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('blog.urls')),
]
```

### 4. 注册App
```python
# settings.py 中的 INSTALLED_APPS 添加
INSTALLED_APPS = [
    ...
    'blog',
]
```

**关键学习点**：理解请求的完整流程：
浏览器输入网址 → urls.py找到对应函数 → views.py执行逻辑 → 返回结果给浏览器



## 五、第四阶段：模板系统（Templates）（3-5天）

https://docs.djangoproject.com/en/6.1/intro/tutorial03/

### 1. 创建模板文件
```
blog/
├── templates/
│   └── blog/
│       └── home.html
```

```html





    
欢迎来到{{ name }}的博客！



```

### 2. 修改视图使用模板
```python
# blog/views.py
from django.shortcuts import render

def home(request):
    return render(request, 'blog/home.html', {'name': '小明'})
```

**学习重点**：理解`{{ }}`语法是Django的"变量插槽"，让Python数据能显示在网页上。



## 六、第五阶段：模型与数据库（1-2周）

这是Django最强大也最难的部分，需要认真仔细学习。

### 1. 定义模型
```python
# blog/models.py
from django.db import models

class Article(models.Model):
    title = models.CharField(max_length=100)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    
    def __str__(self):
        return self.title
```

### 2. 数据库迁移（重要概念）
```bash
python manage.py makemigrations  # 生成迁移文件
python manage.py migrate          # 应用到数据库
```

用比喻理解：`makemigrations`是"画设计图"，`migrate`是"真正施工建仓库"

### 3. 使用Django后台管理系统（超级适合初学者的亮点功能）
```python
# blog/admin.py
from django.contrib import admin
from .models import Article

admin.site.register(Article)
```

```bash
python manage.py createsuperuser  # 创建管理员账号
```

访问 `http://127.0.0.1:8000/admin`，添加几篇"文章"，**体会成就感**，因为不写代码就能管理数据。

### 4. 在视图中查询数据
```python
def article_list(request):
    articles = Article.objects.all()
    return render(request, 'blog/list.html', {'articles': articles})
```

```html

{% for article in articles %}
    
{{ article.title }}

    
{{ article.content }}


{% endfor %}
```



## 七、第六阶段：综合项目实践（2-3周）

### 推荐项目（按兴趣选择，越贴近生活越有动力）

1. **个人博客系统**（最经典，适合入门）
   - 发布文章、查看文章列表、文章详情页

2. **简易待办事项(Todo List)**
   - 添加/删除/标记完成任务

3. **游戏成绩记录网站**（如果喜欢游戏）
   - 记录自己玩的游戏、评分、心得

4. **班级小工具**（实用性强，有真实使用场景）
   - 作业提醒、班级通讯录

### 项目开发建议流程
1. 先画出页面草图（纸上或用原型工具）
2. 确定需要哪些数据（设计Model）
3. 一个功能一个功能地实现，不要贪多
4. 每完成一个小功能就测试运行，及时看到效果



## 八、学习方法建议

### 1. 番茄工作法
- 每25分钟专注编码，休息5分钟
- 避免久坐疲劳导致效率下降

### 2. 学习资源推荐
- **官方文档**：http://docs.djangoproject.com/en/6.1/intro/
- **视频**：B站
- **中文教程**：菜鸟教程 https://www.runoob.com/django/django-intro.html

### 3. 培养Debug能力（非常重要）
- 学会看懂报错信息的关键部分（通常在最后几行）
- 学会用搜索引擎或者AI工具搜索/报错信息并且解决问题
- 使用日志语句排查问题


```python
import logging

logger = logging.getLogger(__name__)

logger.debug("调试信息")
logger.info("普通运行信息")
logger.warning("警告信息")
logger.error("错误信息")
```

记录变量：

```python
logger.info("用户 %s 创建了文章 %s", username, article_id)
```

记录异常：

```python
try:
    do_something()
except Exception:
    logger.exception("执行操作失败")
```


### 4. 建立成就感系统
- 每完成一个阶段，可以给同学/家人展示成果
- 使用GitHub保存代码，看到commit记录会有满足感



## 九、常见问题与注意事项

| 问题 | 解决建议 |
|------|---------|
| 环境配置报错 | 建议用VS Code + 虚拟环境(venv)，减少环境冲突 |
| 概念太抽象难理解 | 多用生活化比喻，画图理解请求流程 |
| 学了就忘 | 每周复习一次之前的项目代码 |
| 想要炫酷效果但基础不牢 | 先夯实基础，效果可以后期用CSS框架（如Bootstrap）快速实现 |
| 容易畏难放弃 | 拆分任务粒度要小，保证每天都有"能跑起来"的小成果 |



## 十、总结：完整学习路径时间表

| 周数 | 内容 | 目标 |
|------|------|------|
| 第1周 | Web基础+HTML/CSS | 理解网站原理 |
| 第2周 | Django安装+MVT概念 | 跑通第一个页面 |
| 第3周 | URL路由+View | 理解请求处理流程 |
| 第4周 | Template模板 | 学会展示动态数据 |
| 第5-6周 | Model+数据库 | 掌握数据存取 |
| 第7-9周 | 综合项目实践 | 独立完成一个小网站 |

**最重要的一点**：不要追求速度，高中正是打基础的黄金年龄，宁可慢一点把每个概念吃透，也不要囫囵吞枣。
