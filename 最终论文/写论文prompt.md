我正在写本科生毕业论文，论文题目是**融合知识图谱和大模型的操作系统学习平台**，

请你帮我把这个翻译成英语，要求比较专业







数据库结构：

/*
 Navicat Premium Dump SQL

 Source Server         : dwa
 Source Server Type    : MySQL
 Source Server Version : 50737 (5.7.37)
 Source Host           : localhost:3306
 Source Schema         : osgraph

 Target Server Type    : MySQL
 Target Server Version : 50737 (5.7.37)
 File Encoding         : 65001

 Date: 09/04/2025 09:16:24
*/

SET NAMES utf8mb4;
SET FOREIGN_KEY_CHECKS = 0;

-- ----------------------------
-- Table structure for user_info
-- ----------------------------
DROP TABLE IF EXISTS `user_info`;
CREATE TABLE `user_info`  (
  `user_id` varchar(15) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NOT NULL COMMENT '用户ID',
  `nick_name` varchar(20) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL COMMENT '昵称',
  `email` varchar(150) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL COMMENT '邮箱',
  `password` varchar(50) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL COMMENT '密码',
  `sex` tinyint(1) NULL DEFAULT NULL COMMENT '0:女 1:男',
  `is_admin` int(11) NULL DEFAULT NULL,
  `image` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL,
  PRIMARY KEY (`user_id`) USING BTREE,
  UNIQUE INDEX `key_email`(`email`) USING BTREE,
  UNIQUE INDEX `key_nick_name`(`nick_name`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8mb4 COLLATE = utf8mb4_general_ci COMMENT = '用户信息' ROW_FORMAT = DYNAMIC;

-- ----------------------------
-- Table structure for master_chat
-- ----------------------------
DROP TABLE IF EXISTS `master_chat`;
CREATE TABLE `master_chat`  (
  `id` int(11) NOT NULL AUTO_INCREMENT COMMENT 'Primary key ID',
  `chat_id` varchar(50) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NOT NULL COMMENT 'Chat identifier',
  `title` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL COMMENT 'Chat title',
  `user_id` varchar(15) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NOT NULL COMMENT 'User ID',
  `created_time` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT 'Creation time',
  PRIMARY KEY (`id`) USING BTREE,
  UNIQUE INDEX `idx_chat_id`(`chat_id`) USING BTREE,
  INDEX `idx_user_id`(`user_id`) USING BTREE,
  CONSTRAINT `master_chat_ibfk_1` FOREIGN KEY (`user_id`) REFERENCES `user_info` (`user_id`) ON DELETE RESTRICT ON UPDATE RESTRICT
) ENGINE = InnoDB AUTO_INCREMENT = 31 CHARACTER SET = utf8mb4 COLLATE = utf8mb4_general_ci COMMENT = 'Master chat information' ROW_FORMAT = DYNAMIC;

-- ----------------------------
-- Table structure for master_message
-- ----------------------------
DROP TABLE IF EXISTS `master_message`;
CREATE TABLE `master_message`  (
  `id` int(11) NOT NULL AUTO_INCREMENT COMMENT 'Primary key ID',
  `role` varchar(20) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NOT NULL COMMENT 'Message role (user/assistant)',
  `content` text CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL COMMENT 'Message content',
  `web_reference` text CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL COMMENT 'Web references',
  `gene_reference` longtext CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL,
  `relevant_topics` text CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL COMMENT 'Relevant topics',
  `user_id` varchar(15) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NOT NULL COMMENT 'User ID',
  `chat_id` varchar(50) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NOT NULL COMMENT 'Chat ID',
  `created_time` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT 'Creation time',
  PRIMARY KEY (`id`) USING BTREE,
  INDEX `idx_chat_id`(`chat_id`) USING BTREE,
  INDEX `idx_user_id`(`user_id`) USING BTREE,
  CONSTRAINT `master_message_ibfk_1` FOREIGN KEY (`user_id`) REFERENCES `user_info` (`user_id`) ON DELETE RESTRICT ON UPDATE RESTRICT
) ENGINE = InnoDB AUTO_INCREMENT = 77 CHARACTER SET = utf8mb4 COLLATE = utf8mb4_general_ci COMMENT = 'Master message information' ROW_FORMAT = DYNAMIC;

SET FOREIGN_KEY_CHECKS = 1;



中饱和度、高明度的蓝色(#dae8fc/#b1ddf0)、绿色(#d5e8d4/#b0e3bc)、橙色(#ffe6cc/#ffcc99)和紫色(#e1d5e7/#d5b9d6)系列

边框颜色采用比填充色深一级的同色系颜色(如#6c8ebf, #82b366)

画图模板：

请帮我创建一个draw.io的[图表类型]，请提供完整的xml代码，我将直接导入到draw.io中进行编辑或进一步调整。遵循以下设计要求：

1. 配色方案：
   - 主要使用  **#dae3f3**，**#e2f0d9**，**#e1d5e7****#bac8d3**，**#fff2cc**，**#f2f2f2****#d0cee2**
   - 对比色搭配，相邻模块使用不同色系，增强可读性
   - 同一类别内部使用同色系不同深浅，体现层次感
   - 避免使用过于鲜艳或低饱和度的颜色
   - 边框颜色采用比填充色深一级的同色系颜色
2. 格式规范：
   - 顶部添加粗体标题，字号18pt
   - 每个主要部分添加小标题，字号14pt，粗体
   - 核心组件使用粗体标示
   - 使用圆角矩形(rounded=1)表示模块，圆柱体表示数据库
   - 使用半透明背景(opacity=50)分组相关元素
   - 添加清晰的连接线，搭配简洁的文字说明
   - 在角落添加图例说明各类元素的含义
3. 布局要求：
   - 采用分层结构，从上到下或从左到右表示数据/控制流
   - 相关元素应靠近放置，保持适当间距
   - 整体结构清晰均衡，避免视觉拥挤
   - 图表大小控制在一页内，适合论文排版
   - 确保文字大小适中，易于阅读
4. 专业性要求：
   - 使用领域内标准术语和图示符号
   - 标注关键技术名称和版本号
   - 简明描述核心功能
   - 适当添加简短说明文字增强可理解性
5. 技术要求：
   - 必须且只能使用draw.io格式（.drawio或XML格式）
   - 提供完整的draw.io源文件代码，确保可直接导入draw.io编辑
   - 确保导出后适合在学术论文中使用，可转换为高质量PNG









```
│[(:ConceptNode {name: "ConceptNode",indexes: [],constraints: []}), (:T│[[:HAS_CONCEPT {name: "HAS_CONCEPT"}], [:HAS_SUBCHAPTER {name: "HAS_SU│
│opicNode {name: "TopicNode",indexes: [],constraints: []}), (:ChapterNo│BCHAPTER"}], [:HAS_TOPIC {name: "HAS_TOPIC"}], [:HAS_VIDEO {name: "HAS│
│de {name: "ChapterNode",indexes: [],constraints: []}), (:SubChapterNod│_VIDEO"}]]                                                            │
│e {name: "SubChapterNode",indexes: [],constraints: []}), (:VideoNode {│                                                                      │
│name: "VideoNode",indexes: [],constraints: []})]                      │                 
```





请修改输出，我写的是大四计算机专业毕业设计论文，需求是不要让老师看出来是ai生成的，后期会有ai查重，请不要用太过口语化的句式，要正式但不像ai生成的













# 融合知识图谱与大模型的操作系统学习平台

## 中期答辩演讲稿

尊敬的各位老师、同学们，大家好！

今天我向大家汇报的是"融合知识图谱与大模型的操作系统学习平台"项目的中期进展。本项目旨在融合知识图谱技术与大语言模型，为操作系统学习提供智能化的辅助工具。

## 一、课题背景与意义

操作系统作为计算机专业的核心课程，具有概念抽象、内容庞杂的特点，学生在学习过程中常常面临知识点碎片化、难以建立系统认知的挑战。传统的教学模式往往难以满足学生个性化学习需求，也缺乏即时反馈与互动性。随着知识图谱与大语言模型技术的快速发展，将这两种技术结合应用于教育领域具有重要意义。知识图谱能够构建结构化的知识体系，而大语言模型则具备强大的自然语言理解和生成能力。二者结合，可以实现知识的精准检索、智能问答以及个性化学习路径规划，从而显著提升操作系统课程的学习效率与体验。

## 二、具体工作与技术实现



### 知识图谱构建

**根据项目需要，首先构建了操作系统知识图谱，知识图谱由五种核心节点构成：章节节点、子章节节点、概念节点、题目节点和视频节点，它们共同形成了一个完整的知识结构体系。**

**章节节点存储各个章节的总体概况信息，例如"操作系统概述"、"进程与线程"等，它们通过指向关系连接到对应的子章节节点。子章节节点则包含更细化的内容，如"线程管理"、"进程调度方式"等，这些节点进一步指向各个概念节点。**

**概念节点是整个系统的核心，它们存储了知识点的详细信息。视频节点和题目节点都是基于与概念节点的相似度来构建的。**

当用户提出问题时，系统首先调用我们自主开发的知识图谱检索算法，该算法第一步会根据用户问题与知识图谱中的概念节点（知识点节点）与用户问题进行相似度计算，筛选出相关的概念节点，然后查找与这些概念节点直接相连的子章节节点，从子章节节点中抽取所有概念节点，以及与这些概念节点相关的视频节点和题目节点，并把这些节点组织成图的形式，并从抽离出的所有概念节点取其中的语料作为相关信息辅助大模型生成回答，组织成的图作为可溯源的图谱。，取，精准匹配图谱中的相关概念节点。随后，系统从这些节点提取相关知识点语料，将其作为背景知识与用户问题一起输入给大模型。

### 智能问答系统

**知识增强问答是本系统的核心功能。传统大模型在回答用户问题时仅依赖于其自身训练的知识，由于无法溯源，容易产生"幻觉"问题。为解决这一难题，我们系统基于操作系统知识图谱实现了可溯源的问答功能。**

**具体实现算法如下：当用户提出问题时，系统首先调用我们自主开发的知识图谱检索算法，精准匹配图谱中的相关概念节点。随后，系统从这些节点提取相关知识点语料，将其作为背景知识与用户问题一起输入给大模型。**

**为进一步丰富回答内容，系统还会调用Bing Search API从互联网实时检索相关信息，作为补充知识来源，从而显著增强回答的准确性和全面性。**

### 系统总体架构

**本项目采用前后端分离架构。前端基于Vue3框架实现，后端使用FastAPI，通过Axios和WebSocket实现前后端通信。数据存储方面，用户数据和聊天历史记录存储在MySQL数据库中，而操作系统知识图谱则存储在Neo4j图数据库中。系统还集成了大语言模型API和Bing Search API，用于问答功能的实现。**



## 三、成果展示

1. 在问答页面，用户可选择多种检索功能增强回答质量，包括知识图谱检索、互联网检索和学习资料检索。选择知识图谱检索后，系统会从知识库中提取相关信息；启用互联网检索功能则调用Bing搜索API获取最新网络资源；而学习资料检索则为用户匹配相关题目和教学视频，提供全面的学习支持。
2. 系统已实现聊天记录功能，所有用户对话内容均存储在MySQL数据库中，确保信息可追溯与回顾。
3. 基于WebSocket连接的流式传输已成功实现，回答内容以Markdown格式实时展示在页面上，提升用户体验。
4. 在系统生成回答后，用户可通过多个功能按钮深入探索，如"知识图谱可视化"、"相关题目"等。页面底部还提供视频参考链接和互联网信息来源，点击即可跳转至详细内容，方便用户进一步学习。
5. 点击"知识图谱可视化"按钮后，问题相关的知识节点会清晰展示在页面中，使用户能够直观理解知识结构，实现回答的可溯源性。
6. "相关题目"功能展示与当前问题相关的练习题，并支持一键下载为Word文件，便于离线学习与复习。
7. 系统的第二个核心功能是操作系统知识图谱可视化，用户可根据章节和子章节筛选查看相关知识结构，全面把握学科体系。
8. 知识图谱界面支持节点交互，用户可通过双击节点查看详细信息，深入了解具体知识点内容。





# 知识图谱结构表格

## 节点标签 (Node Labels) (595)

| 节点类型       | 描述       |
| -------------- | ---------- |
| ChapterNode    | 章节节点   |
| ConceptNode    | 概念节点   |
| SubChapterNode | 子章节节点 |
| TopicNode      | 题目节点   |
| VideoNode      | 视频节点   |

## 关系类型 (Relationship Types) (598)

| 关系类型       | 描述           |
| -------------- | -------------- |
| HAS_CONCEPT    | 拥有概念关系   |
| HAS_SUBCHAPTER | 拥有子章节关系 |
| HAS_TOPIC      | 拥有题目关系   |
| HAS_VIDEO      | 拥有视频关系   |

## 属性键 (Property Keys)

| 属性键              | 可能所属节点   | 描述         |
| ------------------- | -------------- | ------------ |
| chapter_name        | ChapterNode    | 章节名称     |
| color               | 多种节点       | 显示颜色     |
| concept_name        | ConceptNode    | 概念名称     |
| description         | 多种节点       | 描述信息     |
| name                | 多种节点       | 名称         |
| subchapter_name     | SubChapterNode | 子章节名称   |
| title               | 多种节点       | 标题         |
| topic_answer        | TopicNode      | 题目答案     |
| topic_answer_reason | TopicNode      | 题目答案原因 |
| topic_description   | TopicNode      | 题目描述     |
| topic_name          | TopicNode      | 题目名称     |
| topic_type          | TopicNode      | 题目类型     |
| url                 | VideoNode      | 视频链接     |
| video_name          | VideoNode      | 视频名称     |

## 节点关系结构图

```
ChapterNode
  ├── HAS_SUBCHAPTER → SubChapterNode
  │     ├── HAS_TOPIC → TopicNode
  │     │     └── HAS_VIDEO → VideoNode
  │     └── HAS_CONCEPT → ConceptNode
  └── HAS_CONCEPT → ConceptNode
```

## 目录

1 绪论----------------------------------------------------------------------------------------------- 1 1.1 研究背景与意义---------------------------------------------------------------------------- 1 1.2 研究现状与问题---------------------------------------------------------------------------- 1 1.3 研究内容------------------------------------------------------------------------------------ 2 1.4 论文章节安排------------------------------------------------------------------------------- 3

2 系统总体设计----------------------------------------------------------------------------------- 4 2.1 需求分析------------------------------------------------------------------------------------ 4 2.2 系统设计原则------------------------------------------------------------------------------- 4 2.3 系统技术架构------------------------------------------------------------------------------- 5 2.4 系统功能架构------------------------------------------------------------------------------- 6 2.5 系统开发环境与工具简介----------------------------------------------------------------- 7 2.6 数据库设计---------------------------------------------------------------------------------- 8 2.7 本章小结------------------------------------------------------------------------------------ 10

3 操作系统知识图谱的设计与实现-------------------------------------------------------------- 11 3.1 知识图谱详细设计------------------------------------------------------------------------- 11 3.2 知识图谱开发和运行环境----------------------------------------------------------------- 13 3.3 操作系统知识图谱实现-------------------------------------------------------------------- 13 3.3.1 章节与子章节节点构建------------------------------------------------------------- 13 3.3.2 概念节点构建和关系映射---------------------------------------------------------- 14 3.3.3 题目与视频节点关联---------------------------------------------------------------- 17 3.4 知识图谱功能测试------------------------------------------------------------------------- 18 3.5 本章小结------------------------------------------------------------------------------------ 19

4 知识增强问答系统的设计与实现-------------------------------------------------------------- 20 4.1 问答系统详细设计------------------------------------------------------------------------- 20 4.2 系统开发和运行环境---------------------------------------------------------------------- 22 4.3 知识增强问答系统实现-------------------------------------------------------------------- 22 4.3.1 知识图谱检索算法实现------------------------------------------------------------ 22 4.3.2 概念节点匹配与知识点提取------------------------------------------------------- 24 4.3.3 大模型问答增强机制--------------------------------------------------------------- 26 4.3.4 Bing Search API集成与互联网知识获取------------------------------------------ 30 4.3.5 可溯源问答结果生成--------------------------------------------------------------- 32 4.4 系统功能测试------------------------------------------------------------------------------ 33 4.5 本章小结----------------------------------------------------------------------------------- 34

5 总结与展望-------------------------------------------------------------------------------------- 35

参考文献----------------------------------------------------------------------------------------------- 36

致谢---------------------------------------------------------------------------------------------------- 38





# 第三章 基于大模型的操作系统知识图谱设计与实现

## ***\*3.1 操作系统知识图谱系统设计\****





## 4.2 系统整体架构与模块交互

操作系统知识图谱智能问答系统采用了模块化设计，各个功能模块之间通过清晰的接口进行交互，形成了一个有机的整体。系统的整体架构可以概括为前端交互层、后端服务层和知识资源层三部分。

前端交互层负责与用户直接交互，接收用户问题，展示回答结果和知识可视化图形。它采用了响应式设计，能够自适应不同设备的屏幕尺寸，提供良好的用户体验。

后端服务层是系统的核心，它包含了知识图谱检索、互联网搜索、大模型问答和结果生成四个主要模块。这些模块之间通过异步通信方式进行协作，保证了系统的高效运行。

知识资源层包括Neo4j图数据库存储的操作系统知识图谱、Bing搜索API接口以及大模型知识库。这三种知识资源相互补充，共同构成了系统的知识基础。

各模块之间的交互流程可以概括为：前端接收用户问题并传递给后端；后端根据用户设置的知识来源选项，并行调用知识图谱检索模块和互联网搜索模块；两个模块的检索结果被整合成提示词，输入到大模型问答模块；大模型生成初步答案；结果生成模块对答案进行格式化和溯源处理；最终结果返回给前端展示。整个过程充分利用了异步并行处理技术，保证了系统的响应速度。

## 4.3 系统实现中的关键技术

在系统实现过程中，我们采用了多种先进技术来保证系统的性能和可用性。首先是异步编程技术，它贯穿于整个系统，从前端请求处理、知识图谱检索到互联网搜索，都采用了异步方式，有效避免了阻塞操作对系统响应速度的影响。

向量相似度计算是另一个关键技术，它使系统能够理解问题的语义，而不仅限于关键词匹配。我们使用了预训练的向量模型，将文本转化为高维向量，通过计算向量间的余弦相似度，找出与问题语义最相关的知识点。

图数据库查询优化是支持高效知识图谱检索的基础。我们针对Neo4j图数据库的特性，设计了高效的查询语句和索引策略，并采用了并行查询和结果缓存技术，显著提升了检索速度。

提示词工程是发挥大模型能力的关键。我们通过精心设计的提示词模板，引导大模型理解任务要求，合理利用外部知识，生成高质量的专业回答。提示词模板的设计经过了多次迭代优化，既考虑了不同知识来源的特点，又兼顾了回答的专业性和可读性。

## 4.4 本章小结

本章详细介绍了操作系统知识图谱智能问答系统的设计与实现。通过知识图谱检索、互联网搜索与大模型问答的有机结合，系统实现了高质量的操作系统领域问答功能。特别是知识图谱的多层次检索算法和异步并行处理机制，大大提升了系统的响应速度和知识覆盖广度。

知识增强的问答机制使系统能够提供比传统问答系统更准确、更专业的回答。通过灵活的知识来源选择和精心设计的提示词模板，系统能够根据不同问题类型和用户需求，动态调整知识增强策略。可溯源机制的实现，则进一步提高了系统回答的可信度和透明度，使用户能够追溯信息来源，深入了解相关知识网络。

这套系统不仅能够满足操作系统领域学习者的知识需求，还为知识图谱与大模型融合应用提供了一个实用范例。未来工作将着重于知识图谱的持续扩充、检索算法的进一步优化以及用户交互体验的提升，使系统能够支持更广泛的应用场景和知识需求。



























我之前已经写好了一部分背景与意义，研究现状，但是引用的论文太少了，我下面又找了一些新的论文，请你帮我修改这些内容，希望你写的更好，并在最后给出引用：





**1、背景介绍**

随着信息技术的飞速发展，教育领域正经历深刻的变革。传统的教学模式正逐步向数字化、智能化方向转型，以满足个性化、精准化的学习需求[1]。这种转型推动了人工智能、大数据等技术与教育领域的深度融合，为教学方法的创新和教育资源的高效利用带来了新的契机。其中，知识图谱作为一种强大的知识表示与组织方式，凭借其直观的图形化表达与语义关联能力，成为教育技术领域的研究热点，广泛应用于知识管理、教学辅助、学习分析等场景。

知识图谱能够将知识点及其相互关系以图形化的形式呈现，帮助学习者直观地理解知识结构，探索知识之间的关联，从而促进深度学习的发生。在教育场景中，特别是在复杂的学科知识教学中，知识图谱的作用尤为显著。例如，操作系统课程作为计算机科学与技术专业的核心课程，涵盖了进程管理、内存管理、文件系统、输入输出系统等诸多知识点，同时知识点之间存在复杂的逻辑联系和依赖关系。学生在学习过程中，常常面临知识点繁杂且难以系统掌握的困难。基于知识图谱的学习系统能够为学生提供清晰的知识结构，通过关联性、层次化的图形化展示，帮助学生构建完整的知识体系，并加深对复杂概念的理解，从而显著提升学习效率与效果。

此外，随着人工智能技术的快速发展，大语言模型（如GPT-4）展现出强大的自然语言处理与知识推理能力，为教育智能化提供了更多可能性[2]。通过将大语言模型与知识图谱相结合，可以进一步增强教育系统的智能化水平。例如，大语言模型能够通过自然语言理解实现智能问答、自动化题库管理、智能化知识点推理、以及个性化学习路径推荐等功能。这些功能不仅满足了学生个性化学习需求，还能根据学习行为数据提供实时的学习反馈与建议，显著提升学生的学习体验。同时，教师也可以借助这些工具进行教学资源的智能生成与优化，推动教学质量的全面提升[3]。

在操作系统课程的教学场景中，通过引入大语言模型与知识图谱的融合技术，不仅可以实现知识点的图形化表示，还能够提供交互式学习功能，让学生以自然语言的方式与知识系统进行实时交互。例如，王芳等[3]构建了C++语言的知识图谱方便学生学习，学生可以提出问题并获得基于知识图谱的智能回答，也可以在学习中发现知识点的关联性与层次关系，从而优化学习路径，弥补知识盲区。此外，通过对学习行为数据的分析，还可以生成个性化学习报告，为教师和学生提供针对性的教学建议。这种智能化学习系统，不仅具有重要的理论研究价值，还在教学实践中展现出广阔的应用前景。

因此，设计并实现一个基于知识图谱的操作系统学习系统，融合大语言模型的能力，不仅能够解决传统教学中的诸多痛点，还可以推动智能教育技术的发展。该系统将结合知识图谱的结构化知识表示能力与大语言模型的智能化交互能力，为学生提供一个功能丰富、高效智能的学习平台，同时为教育研究者探索人工智能与教育深度融合提供一个典型的应用场景。

**2、研究现状**

当前在教育领域，知识图谱（Knowledge Graph, KG）作为一种强大的知识表示与组织方式，已成为研究热点，特别是在个性化教育和智能化教学系统的设计中。知识图谱能够帮助学生直观地理解复杂知识结构，实现学习路径推荐、动态知识查询和个性化评估，为教育领域带来了深刻变革。

知识图谱的构建是核心技术之一。传统方法多依赖人工规则和手动输入，虽然在部分领域仍有应用，但存在更新缓慢和成本高的问题。近年来，基于自然语言处理（NLP）和深度学习的自动化构建方法逐渐主流。Zhong等[4]综述了自动构建的研究进展，提出基于文本挖掘和关系抽取的方法，取得了良好效果。Liang等[5]研究了大语言模型（LLM）增强的知识图谱构建技术，在复杂产品设计领域展现了其高效性和准确性。Su等[6]进一步提出了一种基于关系数据的自动化方法，实现多领域高效应用。

基于深度学习的技术展现出显著潜力。Meyer等[7]利用大语言模型（如ChatGPT）辅助知识图谱工程，证明LLM在构建与推理中提升了效率和质量。Zhu等[8]详细分析了LLM在知识图谱生成、更新及推理中的应用，并探讨其未来发展趋势。

在教育领域，知识图谱的应用集中于知识管理、学习辅助与教学优化。知识图谱能帮助学生理解知识点间关系，尤其适用于操作系统和人工智能等复杂学科。Lin等[9]提出基于知识图谱的学习路径推荐方法，而Chen等[10]设计了基于知识推理的学习模型，通过智能问答和个性化推荐为学生提供精准支持。

知识图谱与LLM的结合成为教育智能化的重要趋势。Abu-Salih等[11]系统综述了这一方向，强调其在智能答疑、内容生成及学习路径推荐中的潜力。随着LLM技术发展，其在教育领域的应用前景愈加广阔，可为教师和学生提供实时支持。

然而，这一领域仍面临挑战，包括知识点提取、关系抽取、图谱更新等技术问题，以及确保图谱准确性和推理链有效性的难点。未来，随着技术进步和数据积累，知识图谱与LLM的结合将为个性化教育和自适应学习系统提供更智能化支持。因此本项目拟采用知识图谱与大语言模型的融合技术，针对操作系统教学中知识点繁杂、难以系统掌握的痛点，通过构建操作系统知识图谱，结合深度学习、自然语言处理与图数据库，实现知识结构的可视化、动态问答与个性化学习路径推荐等功能。项目将使用Neo4j进行知识存储，结合Vue3和FastAPI开发交互式学习平台，显著提升学习效率与效果，并推动教育智能化发展。





[1]郑永和, 王一岩. 教育与信息科技交叉研究：现状、问题与趋势[J]. 中国电化教育, 2021, (07): 97-106.

[2]Kasneci E, Sessler K, Küchemann S, et al. ChatGPT for good? On opportunities and challenges of large language models for education. Learn Individ Differ, 2023, 103: 102274. 

[3]王芳,孔维琦,齐浩哲,等.基于知识图谱的C++程序设计知识体系构建[J].计算机教育,2024,(12):194-199.DOI:10.16512/j.cnki.jsjjy.2024.12.038.

[4]Zhong L, Wu J, Li Q, et al. A comprehensive survey on automatic knowledge graph construction[J]. ACM Computing Surveys, 2023, 56(4): 1-62.

[5]Liang X, Wang Z, Li M, et al. A survey of LLM-augmented knowledge graph construction and application in complex product design[J]. Procedia CIRP, 2024, 128: 870-875.

[6]Su Z, Hao M, Zhang Q, et al. Automatic knowledge graph construction based on relational data of power terminal equipment[C]//2020 5th International Conference on Computer and Communication Systems (ICCCS). IEEE, 2020: 761-765.

[7]Meyer L P, Stadler C, Frey J, et al. Llm-assisted knowledge graph engineering: Experiments with chatgpt[C]//Working conference on Artificial Intelligence Development for a Resilient and Sustainable Tomorrow. Wiesbaden: Springer Fachmedien Wiesbaden, 2023: 103-115.

[8]Zhu Y, Wang X, Chen J, et al. Llms for knowledge graph construction and reasoning: Recent capabilities and future opportunities[J]. World Wide Web, 2024, 27(5): 58.

[9]Lin J, Zhao Y, Huang W, et al. Domain knowledge graph-based research progress of knowledge representation[J]. Neural Computing and Applications, 2021, 33: 681-690.

[10]Chen X, Jia S, Xiang Y. A review: Knowledge reasoning over knowledge graph[J]. Expert systems with applications, 2020, 141: 112948.

[11]Abu-Salih B, Alotaibi S. A systematic literature review of knowledge graph construction and application in education[J]. Heliyon, 2024.

[12]刘敏.基于深度学习的多源信息融合知识图谱智能化构建技术[J/OL].自动化技术与应用,1-5[2024-12-27].http://kns.cnki.net/kcms/detail/23.1474.TP.20241223.1 32 2.066.html.

[14]Jiang J H A. Knowledge Graph of Thoughts: An LLM Using a Knowledge Graph to Reason[D]. ETH Zurich, 2024.

[15]Huang L, Xiao X. CTIKG: LLM-Powered Knowledge Graph Construction from Cyber Threat Intelligence[C]//First Conference on Language Modeling. 2024.

















我又新找的这些论文，我会把标题，作者和摘要以及引用格式发你：

1.Enhancing Knowledge Graph Construction Using Large Language Models

[Milena Trajanoska](https://arxiv.org/search/cs?searchtype=author&query=Trajanoska,+M) (1), [Riste Stojanov](https://arxiv.org/search/cs?searchtype=author&query=Stojanov,+R) (2), [Dimitar Trajanov](https://arxiv.org/search/cs?searchtype=author&query=Trajanov,+D) (3) 

The growing trend of Large Language Models (LLM) development has attracted significant attention, with models for various applications emerging consistently. However, the combined application of Large Language Models with semantic technologies for reasoning and inference is still a challenging task. This paper analyzes how the current advances in foundational LLM, like ChatGPT, can be compared with the specialized pretrained models, like REBEL, for joint entity and relation extraction. To evaluate this approach, we conducted several experiments using sustainability-related text as our use case. We created pipelines for the automatic creation of Knowledge Graphs from raw texts, and our findings indicate that using advanced LLM models can improve the accuracy of the process of creating these graphs from unstructured text. Furthermore, we explored the potential of automatic ontology creation using foundation LLM models, which resulted in even more relevant and accurate knowledge graphs.

Trajanoska M, Stojanov R, Trajanov D. Enhancing knowledge graph construction using large language models[J]. arXiv preprint arXiv:2305.04676, 2023.





# Traditional Chinese Medicine Knowledge Graph Construction Based on Large Language Models

by 

Yichong Zhang

 and

Yongtao Hao

 *

This study explores the use of large language models in constructing a knowledge graph for Traditional Chinese Medicine (TCM) to improve the representation, storage, and application of TCM knowledge. The knowledge graph, based on a graph structure, effectively organizes entities, attributes, and relationships within the TCM domain. By leveraging large language models, we collected and embedded substantial TCM–related data, generating precise representations transformed into a knowledge graph format. Experimental evaluations confirmed the accuracy and effectiveness of the constructed graph, extracting various entities and their relationships, providing a solid foundation for TCM learning, research, and application. The knowledge graph has significant potential in TCM, aiding in teaching, disease diagnosis, treatment decisions, and contributing to TCM modernization. In conclusion, this paper utilizes large language models to construct a knowledge graph for TCM, offering a vital foundation for knowledge representation and application in the field, with potential for future expansion and refinement.

Zhang Y, Hao Y. Traditional Chinese medicine knowledge graph construction based on large language models[J]. Electronics, 2024, 13(7): 1395.





# A medical question answering system using large language models and knowledge graphs



[Quan Guo](https://onlinelibrary.wiley.com/authored-by/Guo/Quan), [Shuai Cao](https://onlinelibrary.wiley.com/authored-by/Cao/Shuai), [Zhang Yi](https://onlinelibrary.wiley.com/authored-by/Yi/Zhang)

Question answering systems have become prominent in all areas, while in the medical domain it has been challenging because of the abundant domain knowledge. Retrieval based approach has become promising as large pretrained language models come forth. This study focuses on building a retrieval-based medical question answering system, tackling the challenge with large language models and knowledge extensions via graphs. We first retrieve an extensive but coarse set of answers via Elasticsearch efficiently. Then, we utilize semantic matching with pretrained language models to achieve a fine-grained ranking enhanced with named entity recognition and knowledge graphs to exploit the relation of the entities in question and answer. A new architecture based on siamese structures for answer selection is proposed. To evaluate the approach, we train and test the model on two Chinese data sets, NLPCC2017 and cMedQA. We also conduct experiments on two English data sets, TREC-QA and WikiQA. Our model achieves consistent improvement as compared to strong baselines on all data sets. Qualification studies with cMedQA and our in-house data set show that our system gains highly competitive performance. The proposed medical question answering system outperforms baseline models and systems in quantification and qualification evaluations.

Guo Q, Cao S, Yi Z. A medical question answering system using large language models and knowledge graphs[J]. International Journal of Intelligent Systems, 2022, 37(11): 8548-8564.



## [Knowledge Graph-augmented Language Models for Complex Question Answering](https://aclanthology.org/2023.nlrse-1.1.pdf)

[Priyanka Sen](https://aclanthology.org/people/p/priyanka-sen/), [Sandeep Mavadia](https://aclanthology.org/people/s/sandeep-mavadia/), [Amir Saffari](https://aclanthology.org/people/a/amir-saffari/)

Large language models have shown impressive abilities to reason over input text, however, they are prone to hallucinations. On the other hand, end-to-end knowledge graph question answering (KGQA) models output responses grounded in facts, but they still struggle with complex reasoning, such as comparison or ordinal questions. In this paper, we propose a new method for complex question answering where we combine a knowledge graph retriever based on an end-to-end KGQA model with a language model that reasons over the retrieved facts to return an answer. We observe that augmenting language model prompts with retrieved KG facts improves performance over using a language model alone by an average of 83%. In particular, we see improvements on complex questions requiring count, intersection, or multi-hop reasoning operations.

Sen P, Mavadia S, Saffari A. Knowledge graph-augmented language models for complex question answering[C]//Proceedings of the 1st Workshop on Natural Language Reasoning and Structured Explanations (NLRSE). 2023: 1-8.





# Knowledge-Augmented Language Model Prompting for Zero-Shot Knowledge Graph Question Answering

[Jinheon Baek](https://arxiv.org/search/cs?searchtype=author&query=Baek,+J), [Alham Fikri Aji](https://arxiv.org/search/cs?searchtype=author&query=Aji,+A+F), [Amir Saffari](https://arxiv.org/search/cs?searchtype=author&query=Saffari,+A)

> Large Language Models (LLMs) are capable of performing zero-shot closed-book question answering tasks, based on their internal knowledge stored in parameters during pre-training. However, such internalized knowledge might be insufficient and incorrect, which could lead LLMs to generate factually wrong answers. Furthermore, fine-tuning LLMs to update their knowledge is expensive. To this end, we propose to augment the knowledge directly in the input of LLMs. Specifically, we first retrieve the relevant facts to the input question from the knowledge graph based on semantic similarities between the question and its associated facts. After that, we prepend the retrieved facts to the input question in the form of the prompt, which is then forwarded to LLMs to generate the answer. Our framework, Knowledge-Augmented language model PromptING (KAPING), requires no model training, thus completely zero-shot. We validate the performance of our KAPING framework on the knowledge graph question answering task, that aims to answer the user's question based on facts over a knowledge graph, on which ours outperforms relevant zero-shot baselines by up to 48% in average, across multiple LLMs of various sizes.





Baek J, Aji A F, Saffari A. Knowledge-augmented language model prompting for zero-shot knowledge graph question answering[J]. arXiv preprint arXiv:2306.04136, 2023.
