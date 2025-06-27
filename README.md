### 仓库名
`Community-Elderly-CareService-Platform`

### README 文档

# 社区养老服务平台

## 项目概述
本项目是一个社区养老服务平台，涵盖用户端和管理端。用户端为社区内的老人及家属提供便捷的养老服务查询、预约等功能；管理端则方便社区管理人员对服务信息、用户信息、活动信息等进行管理和维护。

## 技术栈
### 后端
- **框架**：Spring Boot，提供快速开发的基础框架，简化项目配置和部署。
- **数据库操作**：MyBatis-Plus，增强了 MyBatis 的功能，提高数据库操作的效率。
- **数据库**：MySQL，用于存储平台的各类数据，如用户信息、服务信息、活动信息等。
- **依赖管理**：Maven，用于管理项目的依赖库和构建项目。

### 前端
- **前端框架**：Vue.js，构建用户界面，提供良好的交互体验。
- **富文本编辑器**：TinyMCE，方便管理人员发布和编辑服务信息、活动公告等富文本内容。
- **前端组件库**：Layui，提供美观、易用的前端组件，加快界面开发速度。

## 功能模块

### 用户端
1. **服务查询**：用户可以根据服务类型、时间等条件查询社区提供的养老服务。
2. **服务预约**：用户可以选择心仪的服务进行预约，并填写相关信息。
3. **活动报名**：查看社区举办的各类活动，并进行报名参与。
4. **个人信息管理**：用户可以修改自己的个人信息、密码等。

### 管理端
1. **服务管理**：管理人员可以添加、修改、删除社区提供的养老服务信息。
2. **用户管理**：对用户信息进行管理，包括查看用户信息、重置用户密码等。
3. **活动管理**：发布、编辑、删除社区活动信息，查看活动报名情况。
4. **数据统计**：统计服务预约数量、活动参与人数等数据，为决策提供支持。

## 项目结构
```
springboot/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/
│   │   │       ├── controller/  # 控制器层，处理请求
│   │   │       ├── dao/         # 数据访问层，与数据库交互
│   │   │       ├── service/     # 服务层，处理业务逻辑
│   │   │       └── entity/      # 实体类，对应数据库表
│   │   ├── resources/
│   │   │   ├── front/           # 用户端前端代码
│   │   │   │   └── front/
│   │   │   │       └── modules/
│   │   │   │           └── tinymce/  # TinyMCE 富文本编辑器
│   │   │   ├── admin/           # 管理端前端代码
│   │   │   │   └── admin/
│   │   │   │       └── src/
│   │   │   │           └── utils/
│   │   │   └── application.yml  # 项目配置文件
│   └── test/
├── target/
└── README.md
```

## 快速开始
### 前置要求
- Java 8 或更高版本
- Maven
- MySQL 数据库

### 配置数据库
打开 `src/main/resources/application.yml` 文件，配置数据库连接信息：
```yaml
url: jdbc:mysql://127.0.0.1:3306/springboot654g2?useUnicode=true&characterEncoding=utf-8&useJDBCCompliantTimezoneShift=true&useLegacyDatetimeCode=false&serverTimezone=UTC
username: root
password: 123456
```

### 启动项目
使用 Maven 构建并启动项目：
```sh
mvn spring-boot:run
```

### 访问项目
- **管理端地址**：http://localhost:8080/springboot654g2/admin/dist/index.html
- **管理员账号**：abo  密码：abo
- **用户端地址**：http://localhost:8080/springboot654g2/front/index.html

## 贡献
欢迎贡献代码！如果你有任何改进建议或发现了问题，请提交 issue 或 pull request。

## 联系信息
如果你有任何问题或建议，请通过以下方式联系我们：
- 邮箱：[your-email@example.com]

## 致谢
感谢 TinyMCE 团队提供的优秀开源编辑器！感谢 Spring Boot、MyBatis-Plus、Vue.js、Layui 等开源项目的支持！
