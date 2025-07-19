#python #环境 


用==pyenv==管理python版本，用python自带的==venv==管理虚拟环境

pyenv要用==pip install pyenv-win==来安装，`pyenv` 是一个 ​Python 版本管理工具​（类似于 `rbenv` 或 `nvm`），它通常需要通过系统级别的安装方式（如 `brew`、`apt`、手动编译等）来安装，而不是通过 `pip`  按照文档配置环境变量 # 参考：https://github.com/pyenv-win/pyenv-win

D:\Program Files\python\Lib\site-packages\pyenv-win\shims  这个要install版本后才有`shims` 路径要==排在前面==（优先调用虚拟环境或 pyenv 管理的 Python 版本）
D:\Program Files\python\Lib\site-packages\pyenv-win\bin

pyenv --version
```
pyenv install --list  # 查看可安装的python版本

pyenv install 3.7.9  # 安装 Python 3.7.9

pyenv versions  #查看已安装的所有版本

pyenv global 3.7.9  # 默认使用 3.7.9

pyenv local 3.8.12  # 在当前文件夹使用 3.8.12

pyenv which python  # 查看命令实际调用的 Python 路径

python -m venv myenv # 这将在当前目录创建一个名为 myenv 的虚拟环境

myenv\Scripts\activate.bat #  激活虚拟环境 激活后，提示符会(myenv) C:\Your\Project\>

pip freeze > requirements.txt # 生成依赖文件

pip install -r requirements.txt # 从文件安装全部依赖

```




###  **`pyenv install` 失败（如缺少 Visual C++）​**​

- Python 编译需要 ​**​Visual C++ Build Tools​**​，安装：
    - 下载 Microsoft Visual C++ Build Tools 并安装。
    - 或安装 ​**​Python 官方预编译版本​**​（`pyenv` 也支持直接下载二进制包）
### **安装速度慢​**​

- `pyenv` 默认从官方源下载 Python，可以换国内镜像（如清华源）：
    
	```powershell
	# 设置镜像（临时生效） 
	$env:PYENV_MIRROR="https://mirrors.tuna.tsinghua.edu.cn/python/"  pyenv install 3.7.9
	```







