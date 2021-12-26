# Notion+Next.js建站流程

Created: December 26, 2021 3:36 PM

# 0. 起因

最近想整一个个人主页玩玩, 可是平时时间紧张, 所以需要一个建立之后, 可以快速编写博文或直接从我的笔记转换为博文, 并且需要能够快速简单的上传方式. 

目前, 本人使用的笔记工具正是Notion, 并且有personal pro可以无限制上传文件. 于是使用Notion建站的话, 图床之类的问题也一并解决了(Github Pages显示1G的存贮, 长期写博文的话需要图床的支持). 另外, Notion的建站有两种方式, 本次介绍的**方式**和**需要购买独立域名并且配合Cloudflare DNS重定位服务的方式**. 后者需要购买独立域名(可能还需要备案), 并且部分域名可能需要修改DNS才能被Cloudflare识别, 试了两天还是放弃了. 本次介绍的方式无需考虑费用问题且建站调试也更快一些. 

- 本文面向对计算机和前端有一定知识同学.
- 原项目的说明文档为`readme_original.md`.

# 1. 涉及到的服务和项目

- [**Notion**](https://www.notion.so/): 笔记与写作工具. 由于是基于web开发, 所以在各平台都可以无缝切换. 购买`Personal Pro` 或使用学生邮箱可以不受限制的上传文件, 不过免费版也够用.
    - Notion入门一些时间成本, 另外需要一些Markdown知识.
- **[Vercel](https://vercel.com/dashboard)**: 免费的网站托管服务. 网页更新速度比 Github Pages 更快.
- **[nextjs-notion-starter-kit](https://github.com/transitive-bullshit/nextjs-notion-starter-kit)**: 使用Vercel和Next.js服务建站的项目.
    - 缺点: 建站和调试需要一些前端知识.
    - 优点: 建站和调试结束之后, 就可以使用Notion直接更新文章. 与Github Pages相比更加方便, 不过相对的, 能使用的模板也更少一些.
- **[Node.js](https://nodejs.org/en/)**: 使用和调试上述项目的基础工具.

# 2. 大致流程

## 2.1. 事前准备

- 下载
    - 下载**nextjs-notion-starter-kit**项目的[压缩包](https://github.com/transitive-bullshit/nextjs-notion-starter-kit/archive/refs/heads/main.zip).
        - 原作者制作的主页: [https://transitivebullsh.it/](https://transitivebullsh.it/).
        - 如果需要**本站**的样式, 可以使用这个连接下载这个[压缩包](https://github.com/sun510001/nextjs-notion-starter-kit/archive/refs/heads/main.zip).
    - 下载 [Node.js](https://nodejs.org/en/).
- 注册
    - 注册 [Notion](https://www.notion.so/), [Vercel](https://vercel.com/), [Github](https://github.com/).

## 2.2. 开始建站

1. 在本地安装Node.js, 一路默认设置点确定即可.
2. 解压**nextjs-notion-starter-kit**项目的[压缩包](https://github.com/transitive-bullshit/nextjs-notion-starter-kit/archive/refs/heads/main.zip).
3. 打开终端. 进入项目的根目录. 配置项目.
   
    ```bash
    # 安装npm, 如果报错的话使用: npm i --legacy-peer-deps
    npm install 
    
    # 安装vercel服务
    npm i -g vercel
    
    # 上传一次项目, 初次上传时需要选择进行一些配置, 
    # 中途需要将vercel和github连接一次, 其他情况一路回车就行.
    npm run deploy
    ```
    
    - 使用项目上传完成后, 登录 [https://vercel.com/](https://vercel.com/), 可以找到刚刚上传的项目. 点击Visit可以查看建立的网站.
      
        ![Untitled](Notion+Next%20js%E5%BB%BA%E7%AB%99%E6%B5%81%E7%A8%8B%20cfdd232803164a19b35161eac5259330/Untitled.png)
        
    - 使用默认的项目 **nextjs-notion-starter-kit** 上传的话会看到如下画面.
      
        ![Untitled](Notion+Next%20js%E5%BB%BA%E7%AB%99%E6%B5%81%E7%A8%8B%20cfdd232803164a19b35161eac5259330/Untitled%201.png)
        
    - 如果觉得这个网页样式不错, 可以打开[原项目作者的Notion主页](https://www.notion.so/TransitiveBullsh-it-78fc5a4b88d74b0e824e29407e9f1ec1), 点击右上角的Duplicate, 将他的主页复制到自己的Notion, 然后按需复制组件到自己的主页里(比如网页中的表格).

## 2.3. 开始调试

- 首先打开Notion, 新建一篇笔记作为自己的站点首页, 在右上角点击`Share` 将这一页设为公开. 按自己的需求可以允许被他人吐槽, 被搜索引擎发现或被人复制网站所有内容等功能.
  
    ![Untitled](Notion+Next%20js%E5%BB%BA%E7%AB%99%E6%B5%81%E7%A8%8B%20cfdd232803164a19b35161eac5259330/Untitled%202.png)
    
- 查看当前页面的URL(不是Share里的URL, 是浏览器里的URL)
  
    ```bash
    # URL例子:
    https://www.notion.so/travis-fischer/TransitiveBullsh-it-78fc5a4b88d74b0e824e29407e9f1ec1
    # 则需要记下后面的编号, 这个编号是之后需要的rootNotionPageId
    78fc5a4b88d74b0e824e29407e9f1ec1
    ```
    
- 回到本地, 打开项目中的`site.config.js`.
  
    ```jsx
      // 修改为自己的rootNotionPageId
      rootNotionPageId: '78fc5a4b88d74b0e824e29407e9f1ec1'
    
      // 以下按需修改############################################################
      // basic site info (required)
      name: 'Homepage',
      domain: 'sun',
      author: 'sun',
    
      // open graph metadata (optional)
      description: 'Example site description',
      socialImageTitle: 'sun',
      socialImageSubtitle: 'Hello World!',
      // 以上按需修改############################################################
    
      // social usernames (optional) 只填自己需要的, 填用户名. 
      // 这几个不留空的话, 会以图标的形式显示在页面的右侧.
      twitter: '',
      github: '',
      linkedin: '',
    ```
    
- 启动网页本地调试模式
    - 终端进入项目文件夹路径, 使用以下命令进入调试, 打开网址 [http://localhost:3000](http://localhost:3000/), 显示调试页面.
      
        ```jsx
        npm run dev
        ```
    
- 处理网页右上角的Github项目标志
  
    目前点击这个图标会进入, 项目原作者的Github项目.
    
    ![Untitled](Notion+Next%20js%E5%BB%BA%E7%AB%99%E6%B5%81%E7%A8%8B%20cfdd232803164a19b35161eac5259330/Untitled%203.png)
    
    - 如果需要去掉
      
        在项目中打开`components/NotionPage.tsx`, 找到`<GitHubShareButton />` 删除, 记得删除空行.
        
        同时删除上方的`import { GitHubShareButton } from './GitHubShareButton'` .
        
    - 如果需要将标志连接修改为自己的项目链接
      
        在项目中打开`components/GitHubShareButton.tsx`, 直接修改Github连接就行.
        
        ![Untitled](Notion+Next%20js%E5%BB%BA%E7%AB%99%E6%B5%81%E7%A8%8B%20cfdd232803164a19b35161eac5259330/Untitled%204.png)
    
- 各种网站的图标可以在`public`文件夹中替换修改.
- 进阶: 调式主页的页面样式
    - 在项目中打开`styles/notion.css`, 可以进行各种参数设置.
    - 一般样式都在styles和components这两个文件夹里.

# 3. 新建和发布博文

- 建立一个表格, 将`Name, Description, Public, Published, Tags`建立好 (或者直接复制[原项目作者的Notion主页](https://www.notion.so/TransitiveBullsh-it-78fc5a4b88d74b0e824e29407e9f1ec1)里的表格).
- 在表格中新建一行, 然后在Name列点击Open打开新的一页, 开始写文章.
- 点击`Public`即可公布想要公布的文章.

![Untitled](Notion+Next%20js%E5%BB%BA%E7%AB%99%E6%B5%81%E7%A8%8B%20cfdd232803164a19b35161eac5259330/Untitled%205.png)

# 4. 可能出现的报错

- 调试的时候出现以下警告:
  
    ```jsx
    (node:5656) [DEP0128] DeprecationWarning: Invalid 'main' field in 'D:\code_project\nextjs-notion-starter-kit\node_modules\react-icons\package.json' of 'lib'. Please either fix that or report it to the module author
    (Use `node --trace-deprecation ...` to show where the warning was created)
    ```
    
    解决方法: 打开`node_modules/react-icons/lib/package.json`. 删除并替换代码. (原代码不能注释, 一定要删除.)
    
    ```jsx
    // 原本的代码
      "main": "lib",
    
    // 替换为
      "main": "./lib/cjs/index.js",
      "module": "./lib/esm/index.js",
      "exports": {
        ".": {
          "require": "./lib/cjs/index.js",
          "default": "./lib/esm/index.js"
        }
      },
    ```