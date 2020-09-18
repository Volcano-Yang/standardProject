# standardProject
配置好gitignore、editorconfig、eslint、prettier、husky、lint-staged的比较标准的前端项目启动模板

## 各个配置的目的

- gitignore：协同开发一定会用到git，gitignore的配置可以让一些非必要的过程代码过滤掉，不上传到github
- editorconfig: 保证协同开发中，不同的人使用不同的编辑器，产出的代码的一致性
- prettier、eslint：保证大家编码的风格一致性，同时有利于在编码时期就发现一些错误
- husky、lint-staged：可以在git commit或者git push前执行一些命令，我们可以在这个阶段调用ESLint等进行修改代码的检查，如果不通过，就不允许commit或者 push。

## 部分配置过程

### 项目安装prettier
1. npm install --save-dev --save-exact prettier
2. 同时需要在安装目录下创建.prettier.json配置文件
3. 安装好prettier插件

### 项目安装husky、lint-staged

#### husky
1. 安装依赖：npm install -D husky
2. 修改 package.json，增加配置：

复杂版：
```json
"husky": {
        "hooks": {
            "commit-msg": "repoflow attach .git/COMMIT_EDITMSG --strict && commitlint -E HUSKY_GIT_PARAMS",
            "pre-commit": "lint-staged && npm test",
            "pre-push": "git tag -l | xargs git tag -d"
        }
    }
```

简单版：
```json
"husky": {
        "hooks": {
            "pre-commit": "lint-staged",
        }
    }
```

#### lint-staged
lint-staged 可以让我们只 lint 本地提交所修改的文件，无需检查所有文件。

1. 安装依赖：npm install -D lint-staged
2. 修改 package.json，增加配置：
```json
 "lint-staged": {
        "**/*.{js,vue}": [
            "eslint --quiet --fix",
            "prettier --write",
            "git add"
        ]
    },
```

## 其他
### 要留意npm install -save 和 -save-dev

> devDependencies节点下的模块是我们在开发时需要用的，比如项目中使用的 gulp ，压缩css、js的模块。这些模块在我们的项目部署后是不需要的，所以我们可以使用 -save-dev 的形式安装。
>
> dependencies节点的模块是我们开发和项目运行是都需要用的. 比如 express 这些模块是项目运行必备的，应该安装在dependencies节点下，所以我们应该使用 -save 的形式安装。
