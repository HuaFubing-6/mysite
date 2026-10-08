# mysite

基于 **Django 6.1** 的一个学习型 Web 项目，通过一个简单的示例实践了 Django 的 URL 路由、视图函数、模板渲染、ORM 数据库操作、表单提交与重定向、调用第三方 API 等核心知识点。

> 本项目为学习练习用途，代码中保留了较多测试性内容，可直接本地运行体验。

## 功能概览

| 路由 | 功能 | 说明 |
| --- | --- | --- |
| `/index/` | 欢迎页 | 返回纯文本“欢迎使用” |
| `/user/list/` / `/user/add/` | 用户页 | 静态模板练习 |
| `/login/` | 登录 | POST 提交，账号密码校验（root / 123），登录成功跳转 |
| `/info/list/` | 用户信息列表 | 展示数据库中的用户数据 |
| `/info/add/` | 添加用户 | 表单提交写入数据库 |
| `/info/delete/` | 删除用户 | 按 nid 删除记录 |
| `/weather` | 成都实时天气 | 调用 Open-Meteo API 获取并展示 |
| `/orm/` | ORM 练习 | 展示 Django ORM 增删改查用法（部分为注释示例） |

## 技术栈

- **后端框架**：Django 6.1
- **数据库**：MySQL（通过 Django ORM 访问）
- **外部 API**：Open-Meteo（天气数据，需 `pip install requests`）
- **前端**：Django 模板（HTML + 原生表单）

## 项目结构

```
mysite/
├── manage.py              # Django 管理入口
├── mysite/                # 项目配置
│   ├── settings.py        # 配置（数据库、应用、静态文件等）
│   ├── urls.py            # 总路由
│   ├── wsgi.py / asgi.py  # 部署入口
├── myapp/                 # 应用
│   ├── views.py           # 视图函数（页面逻辑）
│   ├── models.py          # 数据模型
│   ├── admin.py           # 后台管理（当前未启用）
│   ├── urls/              # 模板文件
│   │   ├── login.html     # 登录页
│   │   ├── info_list.html # 信息列表
│   │   ├── info_add.html  # 添加信息
│   │   ├── user_list.html # 用户列表
│   │   ├── weather.html   # 天气页
│   │   └── ...
│   ├── static/            # 静态资源（css/js/img）
│   └── migrations/        # 数据库迁移文件
└── db.sqlite3             # SQLite 数据库（当前为空）
```

## 快速开始

### 1. 环境要求

- Python 3.10+
- MySQL（本地或远程均可）
- pip

### 2. 安装依赖

```bash
pip install django requests
```

### 3. 配置数据库

`mysite/settings.py` 中已配置为 MySQL，请按本机环境修改：

```python
DATABASES = {
    "default": {
        "ENGINE": "django.db.backends.mysql",
        "NAME": "test1",           # 数据库名，需先在 MySQL 中创建
        "USER": "root",
        "PASSWORD": "123456",
        "HOST": "127.0.0.1",
        "PORT": 3306,
    }
}
```

> 如果本地没有 MySQL，也可切换为 SQLite：
> ```python
> DATABASES = {
>     "default": {
>         "ENGINE": "django.db.backends.sqlite3",
>         "NAME": BASE_DIR / "db.sqlite3",
>     }
> }
> ```

### 4. 执行数据库迁移

```bash
python manage.py makemigrations myapp
python manage.py migrate
```

### 5. 启动服务

```bash
python manage.py runserver
```

浏览器访问 http://127.0.0.1:8000/index/

## 使用说明

- 登录账号：`root`，密码：`123`（见 `views.py` 的 `login` 函数）
- 天气功能依赖外网访问 `api.open-meteo.com`，首次使用请确认网络可达并已安装 `requests`
- `/info/*` 系列页面读写的是 `UserInfo` 表，可通过页面直接增删数据

## 数据库模型

`myapp/models.py` 定义了两个模型：

- **UserInfo**：`name`（用户名）、`password`（密码）、`age`（年龄）
- **Department**：`title`（部门名称）

## 已知说明

- 数据库配置中的账号密码（root / 123456）为本地开发示例，**上传到 GitHub 前请务必修改或移除**，避免泄露。
- `SECRET_KEY` 为 Django 生成的开发密钥，生产环境需更换并妥善保管。
- 项目中 `test`、`test1` 文件为调试遗留文件，可忽略或删除。

## License

本项目仅供学习交流使用。
