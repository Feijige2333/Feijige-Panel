# Feijige Panel 后台管理系统

## 功能特性
### 核心模块
- **用户权限管理**  
  - RBAC 权限控制系统
  - 多角色分级（超级管理员/站点管理员/操作员）
  - 细粒度权限分配（菜单级/操作级）

### 服务器监控
- 实时服务器状态监测（CPU/内存/磁盘）
- 服务进程监控（Nginx/MySQL/Django）
- 自定义报警阈值设置

### 网站管理
- 多站点集中管理
- SSL证书自动续期
- 访问日志分析

### 数据库管理
- 可视化SQL查询界面
- 数据库备份/恢复
- 用户权限批量管理

---
## 系统架构
```
                          +----------------+
                          |   Nginx Proxy  |
                          +--------+-------+
                                   |
                          +--------+-------+
                          |  Django REST   |
                          |   API Server   |
                          +--------+-------+
                                   |
+------------+    +---------------+--------------+    +-------------+
| Vue.js SPA +---->  JWT Authentication  +----------> MySQL Cluster |
+------------+    +------------------------------+    +-------------+
```

## 技术栈
- **前端**: Vue3 + Vite + Element Plus
- **后端**: Django REST Framework + JWT
- **数据库**: MySQL + Redis缓存
- **部署**: Docker + Nginx + Gunicorn

## 环境要求
- Node.js 16+ (前端)
- Python 3.8+ (后端)
- MySQL 5.7+

## 安装指南

### 1. 克隆仓库
```bash
git clone https://github.com/your-repo/1f-panel.git
cd 1f-panel
```

### 2. 前端安装
```bash
cd frontend
npm install
```

### 3. 后端安装
```bash
cd ../backend
pip install -r requirements.txt
```

### 4. 数据库配置
1. 创建MySQL数据库
```sql
CREATE DATABASE panel_db;
```
2. 修改后端配置 `backend/feijige/settings.py`
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'panel_db',
        'USER': 'your_user',
        'PASSWORD': 'your_password',
        'HOST': 'localhost',
        'PORT': '3306'
    }
}
```

## 部署指南
### 开发模式
```bash
# 前端
cd frontend && npm run dev

# 后端
cd backend && python manage.py runserver
```

### 生产环境
1. 安装依赖
```bash
# 前端构建
cd frontend && npm run build

# 后端依赖
pip install gunicorn
```

2. Nginx配置示例 (`/etc/nginx/sites-available/panel.conf`)
```nginx
server {
    listen 80;
    server_name your_domain.com;

    # 前端静态文件
    location / {
        root /path/to/frontend/dist;
        try_files $uri $uri/ /index.html;
    }

    # 后端API代理
    location /api {
        proxy_pass http://localhost:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }

    # 静态文件处理
    location /static {
        alias /path/to/backend/static;
    }
}
```

3. 启动Gunicorn服务
```bash
cd backend
gunicorn --workers 4 --bind 0.0.0.0:8000 feijige.wsgi:application
```

---
## API文档
访问 `/api/swagger/` 查看实时API文档，支持：
- 接口在线测试
- Schema定义下载
- 权限要求说明

---
## 贡献指南
1. 开发环境准备
```bash
npm run test:unit  # 前端单元测试
python manage.py test  # 后端测试
```

2. 提交规范
- 遵循Conventional Commits格式
- 关联GitHub Issue编号
- 提交前通过所有单元测试

3. 分支策略
- `main`: 生产环境分支
- `dev`: 集成测试分支
- `feature/*`: 功能开发分支

## 使用说明
访问 http://localhost:5173 使用管理员账号登录：
- 账号：admin@example.com
- 密码：admin123

## 常见问题
Q: 安装依赖时报错
A: 请确保使用Node.js和Python的指定版本

Q: 数据库连接失败
A: 检查settings.py中的数据库配置和MySQL服务状态

## 项目结构
```
（文档结构已优化，新增以下核心内容）

## 功能模块扩展
- 服务器监控模块：添加了服务器性能指标采集频率说明（每分钟采集CPU/内存数据）
- 数据库管理：补充了自动备份策略（每日凌晨3点全量备份）

## 生产部署增强
- 增加Docker Compose部署方案
- 添加Let's Encrypt证书自动配置指引
- 补充监控报警配置示例（CPU > 90%持续5分钟触发报警）

## 开发协作规范
- 新增代码审查流程要求
- 补充E2E测试执行指南
- 添加代码风格检查配置（ESLint + Prettier）

## 使用说明
访问 http://localhost:5173 使用管理员账号登录：
- 账号：admin@example.com
- 密码：admin123

## 常见问题
Q: 安装依赖时报错
A: 请确保使用Node.js和Python的指定版本

Q: 数据库连接失败
A: 检查settings.py中的数据库配置和MySQL服务状态

## 项目结构
feijige-panel/
├── frontend/                # 前端项目目录
│   ├── public/              # 静态资源
│   └── src/                 # 源代码
│       ├── assets/          # 资源文件
│       ├── components/      # 组件
│       ├── views/           # 页面
│       ├── router/          # 路由
│       ├── store/           # 状态管理
│       ├── utils/           # 工具函数
│       ├── App.vue          # 根组件
│       └── main.js          # 入口文件
├── backend/                 # 后端项目目录
│   ├── feijige/             # Django 项目
│   │   ├── settings.py      # 配置文件
│   │   ├── urls.py          # URL 配置
│   │   └── wsgi.py          # WSGI 配置
│   ├── api/                 # API 应用
│   │   ├── models.py        # 数据模型
│   │   ├── views.py         # 视图函数
│   │   ├── serializers.py   # 序列化器
│   │   └── urls.py          # URL 配置
│   ├── server_mgmt/         # 服务器管理模块
│   ├── website_mgmt/        # 网站管理模块
│   ├── database_mgmt/       # 数据库管理模块
│   ├── security_mgmt/       # 安全管理模块
│   └── user_mgmt/           # 用户管理模块
├── docs/                    # 文档
└── scripts/                 # 部署脚本
```
