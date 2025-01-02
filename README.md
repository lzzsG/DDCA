《Digital Design and Computer Architecture: RISC-V Edition》是一部深入探讨数字设计与计算机架构的经典教材，覆盖了从基础概念到硬件描述语言以及关键数字构建模块的广泛内容。本书针对RISC-V架构，通过详细的理论与实践相结合，为读者提供扎实的计算机设计与数字电路知识。内容适合工程师、学生及所有对数字系统设计感兴趣的学习者。

#### **Chapter 1: From Zero to One**
本章从计算机设计的基础入手，解析数字设计的基本原理与计算机系统的核心架构。标题“从零到一”不仅象征从基础概念到复杂系统设计的递进过程，更体现了计算机运算的二进制本质。本章内容为后续学习打下坚实的理论基础。

#### **Chapter 2: Combinational Logic Design**
本章深入解析组合逻辑设计的核心知识。结合布尔代数、逻辑门、卡诺图等概念，全面探讨组合逻辑电路的组成与功能特性。章节还涵盖时间和功能规范，使读者能够理解无存储单元电路的设计及其广泛应用。

#### **Chapter 3: Sequential Logic Design**
时序逻辑设计是现代计算系统的核心工作原理，本章全面解析顺序逻辑与组合逻辑的区别，阐述状态元件的功能与同步时序逻辑设计的方法。本章还深入探讨有限状态机（FSM）的实现，并分析时序逻辑中的时钟偏斜与并行性问题。

#### **Chapter 4: Hardware Description Languages (HDL)**
硬件描述语言（HDL）是数字系统设计的关键工具。本章深入介绍HDL的基础知识与实际应用，通过高层次代码定义硬件电路功能，并利用仿真工具验证设计的正确性。本章为读者理解复杂硬件系统设计提供了全面支持。

#### **Chapter 5: Digital Building Blocks**
本章聚焦于数字设计中的核心构建模块，包括算术电路、时序电路、存储器阵列及逻辑阵列。内容涵盖加法器、触发器、计数器及存储设备等关键组件，深入探讨这些模块在现代计算设备中的基础性作用，为后续学习奠定实践基础。

---

## just-the-docs-template 模板简介


此项目使用 Jekyll 的 "just-the-docs" 主题来创建一个简洁的文档网站并自动化部署。主要内容放置在 `docs` 文件夹内，网站首页通过根目录下的 `index.md` 文件进行配置。额外进行了主题的修改并添加了主题切换按钮。

## 环境设置

### 必需的软件

- **Ruby** (包括 RubyGems, GCC, 和 Make)
- **Jekyll**
- **Bundler**

#### 安装指南

#### Windows

1. 安装 Ruby:

   - 下载并安装 Ruby+Devkit 版本从 [RubyInstaller](https://rubyinstaller.org/downloads/)。
   - 安装过程中，确保勾选 "Add Ruby to your PATH" 选项。

2. 安装 Jekyll 和 Bundler:

   ```bash
   gem install jekyll bundler
   ```

#### Linux

1. 安装 Ruby (使用 rbenv 或 rvm):

   - 安装 rbenv 和 ruby-build:

     ```bash
     sudo apt-get update
     sudo apt-get install -y git curl libssl-dev libreadline-dev zlib1g-dev autoconf bison build-essential libyaml-dev libreadline-dev libncurses5-dev libffi-dev libgdbm-dev
     curl -fsSL https://github.com/rbenv/rbenv-installer/raw/main/bin/rbenv-installer | bash
     ```

   - 添加 rbenv 到 bash:

     ```bash
     echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
     echo 'eval "$(rbenv init -)"' >> ~/.bashrc
     exec $SHELL
     ```

   - 安装 Ruby:

     ```bash
     rbenv install 2.7.2
     rbenv global 2.7.2
     ```

   - 检查 Ruby 安装:

     ```bash
     ruby -v
     ```

2. 安装 Jekyll 和 Bundler:

   ```bash
   gem install jekyll bundler
   ```

## 配置项目

1. 获取项目代码：

- **克隆仓库**：

  ```bash
   git clone https://github.com/lzzsG/just-the-docs-template.git
   cd just-the-docs-template
  ```

- **使用模板**： 访问项目的 GitHub 页面，点击右上角的 “Use this template” 按钮。

- **直接 Fork**： 访问项目的 GitHub 页面，点击右上角的 “Fork” 按钮，然后克隆你 Fork 的仓库


2. 安装依赖：

   ```bash
   bundle install
   ```

3. 修改 `_config.yml` 文件以自定义网站设置（如标题、描述、URL 等）。

4. 编辑 `index.md` 以设置首页内容。

## 运行项目

- 运行以下命令来启动 Jekyll 服务器，并启用实时重载功能：

  ```bash
  bundle exec jekyll serve --livereload
  ```

  访问 `http://localhost:4000` 在浏览器中查看网站。

## 更多配置

- 请参考 [just-the-docs官方文档](https://just-the-docs.com/) 来了解更多高级配置和自定义选项。
