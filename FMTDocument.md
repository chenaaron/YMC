# FMTDocuments

## 总览

`FMTDocuments` 仓库是专门为无线 iOS 团队提供的文档库， 所有你需要的团队文档都应该存放于此， 以方便所有团队成员查找阅读。 

***如果 `FMTDocuments` 不能满足你的某些需求***：

+ 比如， 你发现文档中的某些疏漏， 错误

	你可以在仓库的 `issues` 页面创建一个 `labels` 为 `enhancement` 的 issue， 标题中简明扼要的描述文档的问题， 然后在内容中给出详细描述。 最后会由该文档的维护者来处理这个问题。

+ 比如， 你希望创建新的文档， 并将它合并到 FMTDocuments

	你可以在仓库的 `issues` 页面创建一个 `labels` 为 `documentation` 的 issue, 在标题中简明扼要的描述你需要创建的文档名称， 然后在内容中给出你所新建文档所主要涉及的领域， 然后在自己克隆的本地仓库中开出自己的新分支，撰写文档。 文档撰写完毕后通过 `Gitlab` 的 `merge request` 来合并到主分支， 以此完成新文档的创建。

## iOS 架构全景

iOS 团队项目从框架到业务目前为止如下图所示， 采用简单的分层架构， 对整个项目分为三层， 从下到上依次为 `FMTFoundation`, `FMTBusiness`, `各业务APP`


    ┌───────────────────────────────────┐   ┌───────────────────────────────────┐  ┌───────────────────────────────────┐
    │ ┌───────────────────────────────┐ │   │ ┌───────────────────────────────┐ │  │ ┌───────────────────────────────┐ │
    │ │┌────────┐┌────────┐ ┌────────┐│ │   │ │┌────────┐┌────────┐┌────────┐ │ │  │ │┌────────┐┌────────┐ ┌────────┐│ │
    │ ││ login  ││ custom │ │  ...   ││ │   │ ││  seek  ││ coupon ││  ...   │ │ │  │ ││ Agent  ││  Rush  │ │  ...   ││ │
    │ ││        ││ hybrid │ │        ││ │   │ ││  car   ││        ││        │ │ │  │ ││        ││        │ │        ││ │
    │ │└────────┘└────────┘ └────────┘│ │   │ │└────────┘└────────┘└────────┘ │ │  │ │└────────┘└────────┘ └────────┘│ │
    │ └───────────business────────────┘ │   │ └───────────business────────────┘ │  │ └───────────business────────────┘ │
    │ ┌ ─ ─ ─ ─  ┌ ─ ─ ─ ─   ┌ ─ ─ ─ ─  │   │ ┌ ─ ─ ─ ─  ┌ ─ ─ ─ ─  ┌ ─ ─ ─ ─   │  │ ┌ ─ ─ ─ ─  ┌ ─ ─ ─ ─  ┌ ─ ─ ─ ─   │
    │   Hybrid │    Pay   │     ...   │ │   │  Location│   Hybrid │    ...   │  │  │   Update │    Form  │    ...   │  │
    │ │          │           │          │   │ │          │          │           │  │ │          │          │           │
    │  ─ ─ ─ ─ ┘  ─ ─ ─ ─ ┘   ─ ─ ─ ─ ┘ │   │  ─ ─ ─ ─ ┘  ─ ─ ─ ─ ┘  ─ ─ ─ ─ ┘  │  │  ─ ─ ─ ─ ┘  ─ ─ ─ ─ ┘  ─ ─ ─ ─ ┘  │
    │                                   │   │                                   │  │                                   │
    └───────────yaomaiche-iOS───────────┘   └───────────carmaster-iOS───────────┘  └────────supplierspirit-iOS─────────┘
                                                                                                                        
     ┌────────┐┌────────┐┌────────┐┌────────┐┌────────┐┌────────┐┌────────┐┌────────┐┌────────┐┌────────┐┌────────┐     
    ┌┤        ├┤        ├┤        ├┤        ├┤        ├┤        ├┤        ├┤        ├┤        ├┤        ├┤        ├────┐
    ││        ││        ││        ││        ││        ││        ││        ││        ││        ││        ││        │    │
    ││        ││        ││        ││        ││        ││        ││        ││        ││        ││        ││        │    │
    ││        ││        ││        ││        ││        ││        ││        ││        ││        ││        ││        │    │
    ││        ││        ││        ││        ││        ││        ││        ││        ││        ││        ││        │    │
    ││        ││        ││        ││        ││        ││        ││        ││        ││        ││        ││        │    │
    ││Location││  Pay   ││  Form  ││  Page  ││  Scan  ││ Hybrid ││ Patch  ││  UBT   ││ Update ││  Push  ││   IM   │    │
    ││        ││        ││        ││        ││        ││        ││        ││        ││        ││        ││        │    │
    ││        ││        ││        ││        ││        ││        ││        ││        ││        ││        ││        │    │
    ││        ││        ││        ││        ││        ││        ││        ││        ││        ││        ││        │    │
    ││        ││        ││        ││        ││        ││        ││        ││        ││        ││        ││        │    │
    ││        ││        ││        ││        ││        ││        ││        ││        ││        ││        ││        │    │
    ││        ││        ││        ││        ││        ││        ││        ││        ││        ││        ││        │    │
    │└────────┘└────────┘└────────┘└────────┘└────────┘└────────┘└────────┘└────────┘└────────┘└────────┘└────────┘    │
    │                                                                                                                  │
    │   ┌───────────────────────────────────────────────────────────────────────────────────────────────────────────┐  │
    │   │                             ┌──────────┐┌──────────┐┌──────────┐┌──────────┐                              │  │
    │   │                             │Appcontext││  Route   ││ Network  ││  Debug   │                              │  │
    │   │                             │          ││          ││          ││          │                              │  │
    │   │                             └──────────┘└──────────┘└──────────┘└──────────┘                              │  │
    │   └────────────────────────────────────────────Weak Business Layer────────────────────────────────────────────┘  │
    │                                                                                                                  │
    └───────────────────────────────────────────────────FMTBusiness────────────────────────────────────────────────────┘
                                                                                                                        
    ┌──────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
    │                                                                                                                  │
    │   ┌───────────────────────────────────────────────────────────────────────────────────────────────────────────┐  │
    │   │                                                                                                           │  │
    │   │                                                                                                           │  │
    │   └─────────────────────────────────────────────Foundation Layer──────────────────────────────────────────────┘  │
    │                                                                                                                  │
    │   ┌───────────────────────────────────────────────────────────────────────────────────────────────────────────┐  │
    │   │                                                                                                           │  │
    │   │                                                                                                           │  │
    │   └────────────────────────────────────────────────Core Layer─────────────────────────────────────────────────┘  │
    │                                                                                                                  │
    └──────────────────────────────────────────────────FMTFoundation───────────────────────────────────────────────────┘


+ FMTFoundation

	主要依次负责处理面向 `Objective-C` 语言, `Cocoa Touch` 开发框架的增强， 提供网络层， 模型层， 持久化层和一些工具分类
	
+ FMTBusiness

	主要首先提供一层若业务层， 提供路由， 应用上下文， 推送， 更新， 热修复， UBT和调试功能； 
	在弱业务之上， 提供一层强业务层， 并以 `cocoapods` 的 `subspec` 向外暴露， 业务 APP 根据自己的选择定制引用。 目前主要提供混合应用开发， 支付， 推送， 表单， 定制页面， 扫描等功能
	
+ 各业务APP

	基于 `FMTBusiness`， `FMTFoundation` 开发， 实现自身业务模块

## 约定文档

+ [工程配置](configuration/project-configuration.md)

+ [工程源码管理分支模型] (branch-model/git-branch-model.md)

+ [Objective-C 规范指南](objective-c-style-guide.md)

+ [代码注释规范](comments-formatting-style.md)

## 业务文档

+ [要买车]

+ [车达人]

+ [车商精灵](http://gitlab.private.yaomaiche.app/yuntu-wireless/supplierspirit-iOS)

  [API接口文档](supplierspirit/api-interface.md)

  [领域模型](supplierspirit/domain-model.md)

## 相关服务

+ [JSPatchConvertor](http://10.16.45.29:32771)

+ [JSPatch usage](https://github.com/bang590/JSPatch/wiki)

+ [Jenkins for iOS](http://10.16.45.29:8080/view/iOS/)
