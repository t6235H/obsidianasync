#git 

---
==远程仓库直接用gitee，github推送网络问题太多，只使用git clone功能不需要登录github，有上传需求，上传到gitee==
==在本地写代码，用gitee  非要用github，可以手动上传，或者用github的codesace==

---
这些命令是一个完整的 Git 工作流程，用于在本地初始化一个新仓库，添加文件，提交更改，并将其推送到 GitHub 远程仓库。下面是分步解释：

1. **`echo "# re01-one" >> README.md`**  
    创建或追加内容到`README.md`文件，这是项目的说明文档。
    
2. **`git init`**  
    在当前目录初始化一个新的 Git 仓库。
    
3. **`git add README.md`**  
    将`README.md`文件添加到 Git 的暂存区（准备提交）。
    
4. **`git commit -m "first commit"`**  
    将暂存区的文件提交到本地仓库，并-m添加提交说明。
    
5. **`git branch -M main`**  
    将默认分支名从`master`重命名为`main`（现代 Git 推荐的分支名）。到这一步的所有操作，都是在本地做的，只有push是上传给远程仓库github的
    
6. **`git remote add origin https://github.com/t6235H/re01-one.git`**  
    将本地仓库与 GitHub 上的远程仓库（URL 为`origin`）关联。
    
7. **`git push -u origin main`**  
    将本地的`main`分支推送到 GitHub 的`origin`仓库，并设置上游追踪（后续可直接使用`git push/pull`）。执行这条命令，会把本地`main`分支上的内容推送到远程仓库`origin`的`main`分支。其中，`-u`参数的作用是建立追踪关系，这样在后续操作中，就可以直接使用`git push`或者`git pull`命令，而无需再次指定远程仓库和分支。
    
### 其他常用的提交命令：

- **提交所有已跟踪文件**（无需先 `git add`）：
    ```bash
    git commit -am "更新配置文件"
    ```
    
- **修改上一次提交**（追加文件或修改说明）：
    ```bash
    git commit --amend -m "新的提交说明"
    ```
    
### 注意事项：

- **首次推送需权限**：执行`git push`前，需要确保你有 GitHub 仓库的写入权限（可能需要输入账号密码或使用 SSH 密钥）。
- **仓库需提前创建**：在 GitHub 上需要先创建一个名为`re01-one`的空仓库（不要初始化 README，否则会冲突）。
- **避免重复初始化**：如果本地已有 Git 仓库，不需要再次执行`git init`。


---


在向 GitHub 推送代码时，身份验证是必不可少的环节。下面是两种主要的验证方式：

### 1. HTTPS 方式（需输入账号密码）

- **适用情况**：如果你使用的是类似`https://github.com/用户名/仓库名.git`这样的 HTTPS 格式 URL，在首次推送代码时，系统会提示你输入 GitHub 的账号和密码
- https://github.com/t6235H/excalidraw.git
- **注意要点**：
    - 从 2021 年 8 月开始，GitHub 不再支持使用账号密码进行推送操作，而是需要使用**个人访问令牌（Personal Access Token）**。你可以在 GitHub 的设置中生成这样的令牌，生成时记得勾选`repo`相关权限。
    - 为避免每次推送都输入令牌，你可以配置 Git 进行凭证缓存。以 Linux/macOS 系统为例，可以使用以下命令：
        ```bash
        git config --global credential.helper store
        ```
    - 对于 Windows 系统，你可以安装 Git Credential Manager 来管理凭证。

### 2. SSH 方式（使用密钥）

- **适用情况**：当你使用的是`git@github.com:用户名/仓库名.git`这种 SSH 格式 URL 时，就需要使用 SSH 密钥进行验证。
- git@github.com:t6235H/excalidraw.git
- ssh://git@ssh.github.com:443/t6235H/excalidraw.git
- **设置步骤**：
    1. **生成 SSH 密钥**（如果之前没有生成过）：
        ```
        ssh-keygen -t ed25519 -C "your_email@example.com"
        ```
    2. **将公钥添加到 GitHub 账户**：  

        你需要复制`~/.ssh/id_ed25519.pub`文件的内容，然后在 GitHub 的 Settings > SSH and GPG keys 中添加新的 SSH 密钥。
    3. **验证连接**：
        ```
        ssh -T git@github.com
        ```
  
        如果看到`Hi username! You've successfully authenticated...`这样的提示，就说明连接验证成功了。
- **优点**：使用 SSH 方式无需每次推送都输入凭证，而且安全性更高。

### 切换远程 URL

如果你想在 HTTPS 和 SSH 两种验证方式之间进行切换，可以使用以下命令修改远程仓库的 URL：

```
# 切换到SSH
git remote set-url origin git@github.com:t6235H/re01-one.git

# 切换到HTTPS
git remote set-url origin https://github.com/t6235H/re01-one.git
```
---
  
下面两条命令用于配置 Git 的全局用户名和邮箱，这两个信息会被附加到你后续的每一个提交记录中。具体来说：

1. **`git config --global user.name "鄙????"`**  
    将你的用户名设置为**鄙????，这会显示在提交历史和贡献统计中。
    
2. **`git config --global user.email "zha？？？？@qq.com"`**  
    将你的邮箱设置为**zha????@qq.com**，GitHub 会用这个邮箱关联你的提交记录到账户。
    

### 注意事项：

- **邮箱一致性**：这里设置的邮箱需要和你 GitHub 账户绑定的邮箱一致，否则你的提交可能不会显示在 GitHub 的贡献图中。
- **作用范围**：
    - `--global` 选项会将配置写入 `~/.gitconfig`（Linux/macOS）或 `C:\Users\你的用户名\.gitconfig`（Windows），影响所有 Git 仓库。
    - 如果需要为特定仓库使用不同的身份，可以在仓库目录下去掉 `--global` 选项单独配置。

  

配置完成后，你可以通过以下命令查看当前设置：

  

bash

```
git config --global user.name
git config --global user.email
```

  

如果需要修改，重复执行上述命令即可。

要取消 Git 的全局用户名和邮箱设置，可以使用以下命令清除已配置的值：

```
git config --global --unset user.name
git config --global --unset user.email
```

### 取消设置后的影响：

1. **仍然可以使用 Git**：取消设置后，你仍能正常执行`git add/commit/push`等操作。
2. **提交时需要临时身份**：
    - 如果没有全局配置，Git 会在你提交时提示`Please tell me who you are`。
    - 此时你需要在每次提交前临时指定用户名和邮箱：
        ```
        git commit -m "message" --author="Your Name <your.email@example.com>"
        ```

3. **GitHub 贡献无法关联**：
    - 没有邮箱配置，GitHub 无法识别提交者身份，你的贡献将不会显示在个人主页的贡献图中。

### 更推荐的做法：

如果想避免全局配置的影响，**不建议直接取消设置**，而是：

1. **局部配置优先**：  
    在特定仓库中使用不带`--global`的命令单独配置，覆盖全局设置：
    ```
    git config user.name "仓库专用名称"
    git config user.email "仓库专用邮箱@example.com"
    ```
    
2. **使用空值占位**（不推荐，可能导致问题）：
    ```
    git config --global user.name ""
    git config --global user.email ""
    ```
	
	
### 总结：

取消设置后 Git 仍可使用，但每次提交都需要手动指定身份，否则会报错。建议保留全局配置（设置为你的常用身份），并在需要时为特定仓库单独配置。

---
### git的错误：SSL certificate problem: unable to get local issuer certificate

**原因**：
- Windows 系统缺少根证书更新 
- Git for Windows 使用了自带的 OpenSSL，而不是系统证书库

老版本 Windows（如 Windows 7 或未联网的 Windows 10）默认不自动更新根证书。这样，当 Git 通过 HTTPS 访问 GitHub 时，系统不认识 GitHub 使用的 SSL 证书颁发机构（CA），就会报错。

默认情况下，Git for Windows 使用 OpenSSL 来验证 HTTPS 连接。如果 OpenSSL 没有配置或包含完整的根证书链（如 `cacert.pem` 文件缺失或损坏），也会出现证书验证失败。

```
git config --global http.sslbackend schannel

```
让 Git 改为使用 Windows 系统的证书验证（`schannel`），这就避开了 OpenSSL 证书链的问题。