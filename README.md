# 社区养老服务平台

## 项目概述
本社区养老服务平台旨在为社区内的老年人及其家属提供全面、便捷、高效的养老服务解决方案，同时为社区管理人员提供强大的管理工具。平台分为用户端和管理端，用户端聚焦于用户的实际需求，提供丰富的服务查询、预约和活动报名等功能；管理端则侧重于信息的管理和维护，方便社区工作人员对各类数据进行操作和统计分析，从而提升社区养老服务的质量和效率。

## 技术栈
### 后端
- **框架**：采用 Spring Boot 框架，其具备自动配置、快速开发等特性，极大地简化了项目的搭建和部署流程，使开发人员能够专注于业务逻辑的实现。
- **数据库操作**：使用 MyBatis-Plus，它是在 MyBatis 基础上进行增强的工具，提供了丰富的 CRUD 操作方法，减少了大量的样板代码，提高了数据库操作的效率和开发速度。
- **数据库**：选用 MySQL 作为数据存储的载体，它是一款成熟、稳定且广泛应用的关系型数据库，能够满足平台对数据存储和管理的需求，确保数据的安全性和一致性。
- **依赖管理**：借助 Maven 进行依赖管理，它可以自动下载和管理项目所需的各种依赖库，避免了手动管理依赖的繁琐和错误，保证项目的稳定性和可维护性。

### 前端
- **前端框架**：基于 Vue.js 构建用户界面，Vue.js 以其轻量级、响应式和易于上手的特点，能够快速构建出交互性强、性能优良的用户界面，为用户提供流畅的使用体验。
- **富文本编辑器**：集成 TinyMCE 富文本编辑器，它功能强大、易于定制，为管理人员发布和编辑服务信息、活动公告等富文本内容提供了便利，支持多种格式的文本排版和多媒体插入。
- **前端组件库**：使用 Layui 前端组件库，它提供了丰富多样的美观、易用的前端组件，如表格、表单、弹窗等，能够加快界面开发速度，使界面风格统一、美观。

## 功能模块

### 用户端
1. **服务查询**：用户可以根据服务类型（如医疗保健、生活照料、文化娱乐等）、时间（如本周、本月等）等条件进行灵活查询，精准定位自己所需的养老服务。同时，系统会根据用户的浏览历史和收藏记录，提供个性化的服务推荐。
2. **服务预约**：用户在查询到心仪的服务后，可以直接在线进行预约操作。填写相关信息，如预约时间、服务需求等，系统会实时反馈预约结果，并提供预约确认和提醒功能，确保用户不会错过预约的服务。
3. **活动报名**：用户可以查看社区举办的各类活动信息，包括活动主题、时间、地点、内容等。对于感兴趣的活动，用户可以一键报名参与，系统会记录用户的报名信息，并在活动开始前发送通知。
4. **个人信息管理**：用户可以随时修改自己的个人信息，如姓名、联系方式、家庭住址等。同时，用户还可以修改登录密码，确保账户的安全性。系统会对用户修改的信息进行实时更新和保存。

### 管理端
1. **服务管理**：管理人员可以方便地添加新的养老服务项目，包括服务名称、服务种类、工作时间、服务详情等信息。同时，对已有的服务信息进行修改和删除操作，确保服务信息的准确性和及时性。
2. **用户管理**：管理人员可以查看所有用户的详细信息，包括用户账号、姓名、联系方式、注册时间等。对于忘记密码的用户，管理人员可以进行密码重置操作。此外，还可以对异常用户进行封禁处理，维护平台的正常秩序。
3. **活动管理**：管理人员可以发布新的社区活动，上传活动图片、介绍活动内容、设置活动时间和地点等。对已发布的活动信息进行编辑和删除，查看活动的报名情况，如报名人数、报名用户信息等，以便对活动进行合理安排和调整。
4. **数据统计**：系统会对服务预约数量、活动参与人数等数据进行统计分析，生成直观的报表和图表。管理人员可以通过这些数据了解平台的使用情况和用户需求，为决策提供有力支持，如调整服务项目、优化活动安排等。

## 项目结构
```
springboot/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/
│   │   │       ├── controller/  # 控制器层，处理请求，负责接收前端的请求并调用相应的服务进行处理
│   │   │       ├── dao/         # 数据访问层，与数据库交互，使用 MyBatis-Plus 提供的方法进行数据库操作
│   │   │       ├── service/     # 服务层，处理业务逻辑，协调控制器和数据访问层之间的交互
│   │   │       └── entity/      # 实体类，对应数据库表，定义了数据库表的结构和字段
│   │   ├── resources/
│   │   │   ├── front/           # 用户端前端代码，包含用户界面的 HTML、CSS、JavaScript 文件
│   │   │   │   └── front/
│   │   │   │       └── modules/
│   │   │   │           └── tinymce/  # TinyMCE 富文本编辑器，用于编辑富文本内容
│   │   │   ├── admin/           # 管理端前端代码，包含管理界面的 HTML、CSS、JavaScript 文件
│   │   │   │   └── admin/
│   │   │   │       └── src/
│   │   │   │           └── utils/
│   │   │   └── application.yml  # 项目配置文件，包含数据库连接信息、服务器端口等配置项
│   └── test/                    # 测试代码，用于对项目的功能进行单元测试和集成测试
├── target/                      # 项目编译后的输出目录，包含生成的可执行文件和依赖库
└── README.md                    # 项目说明文档，提供项目的概述、技术栈、功能模块、使用方法等信息
```

## 快速开始
### 前置要求
- Java 8 或更高版本，确保 JDK 已正确安装并配置好环境变量。
- Maven，用于项目的依赖管理和构建，需确保 Maven 已正确安装并配置好镜像源，以提高依赖下载速度。
- MySQL 数据库，创建一个名为 `springboot654g2` 的数据库，用于存储平台的数据。

### 配置数据库
打开 `src/main/resources/application.yml` 文件，根据实际情况配置数据库连接信息：
```yaml
spring:
  datasource:
    url: jdbc:mysql://127.0.0.1:3306/springboot654g2?useUnicode=true&characterEncoding=utf-8&useJDBCCompliantTimezoneShift=true&useLegacyDatetimeCode=false&serverTimezone=UTC
    username: root
    password: 123456
    driver-class-name: com.mysql.cj.jdbc.Driver
```

### 启动项目
在项目根目录下，使用 Maven 构建并启动项目：
```sh
mvn spring-boot:run
```
等待项目启动完成，控制台会输出启动成功的信息。

### 访问项目
- **管理端地址**：打开浏览器，访问 http://localhost:8080/springboot654g2/admin/dist/index.html ，使用管理员账号 `abo` 和密码 `abo` 登录管理端。
- **用户端地址**：在浏览器中输入 http://localhost:8080/springboot654g2/front/index.html ，即可访问用户端。

## 代码示例
以下是一个简单的社区服务查询接口的代码示例，展示了控制器层、服务层和实体类的使用：

### 实体类 `ShequfuwuEntity.java`
```java
import com.baomidou.mybatisplus.annotation.TableId;
import com.baomidou.mybatisplus.annotation.TableName;
import java.io.Serializable;
import java.util.Date;

@TableName("shequfuwu")
public class ShequfuwuEntity implements Serializable {
    private static final long serialVersionUID = 1L;

    @TableId
    private Long id;
    private String fuwumingcheng;
    private String fuwuzhonglei;
    private String gongzuoshijian;
    private String fuwuxiangqing;
    private Integer thumbsupnum;
    private Integer crazilynum;
    private Date clicktime;
    private Integer clicknum;
    private Date addtime;

    // 省略 getter 和 setter 方法
}
```

### 服务层接口 `ShequfuwuService.java`
```java
import com.baomidou.mybatisplus.extension.service.IService;
import com.entity.ShequfuwuEntity;
import com.utils.PageUtils;
import java.util.Map;

public interface ShequfuwuService extends IService<ShequfuwuEntity> {
    PageUtils queryPage(Map<String, Object> params);
}
```

### 控制器层 `ShequfuwuController.java`
```java
import com.service.ShequfuwuService;
import com.utils.PageUtils;
import com.utils.R;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;
import java.util.Map;

@RestController
@RequestMapping("/shequfuwu")
public class ShequfuwuController {
    @Autowired
    private ShequfuwuService shequfuwuService;

    @RequestMapping("/page")
    public R page(@RequestParam Map<String, Object> params) {
        PageUtils page = shequfuwuService.queryPage(params);
        return R.ok().put("data", page);
    }
}
```

## 贡献
欢迎广大开发者参与项目的开发和维护！如果你有任何改进建议或发现了问题，请按照以下步骤进行操作：
1. **提交 issue**：在项目的 GitHub 仓库中提交 issue，详细描述问题的现象、复现步骤和期望的结果。
2. **提交 pull request**：如果你有代码改进或新功能添加，可以 Fork 项目仓库，在自己的分支上进行开发，完成后提交 pull request，我们会及时进行审核和合并。

## 联系信息
如果你有任何问题或建议，请通过以下方式联系我们：
- 邮箱：[1732446549@qq.com]

## 致谢
感谢 TinyMCE 团队提供的优秀开源编辑器！感谢 Spring Boot、MyBatis-Plus、Vue.js、Layui 等开源项目的支持，为项目的开发提供了强大的技术基础！同时，感谢所有参与项目开发和测试的人员，你们的努力和贡献使得项目不断完善和进步！
