1. [安装Chocolatey](https://chocolatey.org/install)

2. 安装Windows Terminal

   - `choco install microsoft-windows-terminal --pre `

   - [BUG FIX](https://github.com/mkevenaar/chocolatey-packages/issues/228)

     - ```
       我也遇到同样的错误。从日志来看，似乎是路径错误，因为脚本放在了 lib-bad 目录下，但 chocolatey 却试图从 lib 目录运行它。
       
       如果您尝试安装，并且在安装失败后在管理员 powershell 中运行此命令，它将起作用：
       “C:\ProgramData\chocolatey\lib-bad\microsoft-windows-terminal\1.20.11781\tools\chocolateyInstall.ps1”
       ```

3. [安装PowerShell](https://learn.microsoft.com/zh-cn/powershell/scripting/install/installing-powershell-on-windows?view=powershell-7.5)

   1. 创建PowerShell配置文件

       ```
       if (!(Test-Path -Path $PROFILE)) {
           New-Item -ItemType File -Path $PROFILE -Force
       }
       
       # 保存到了C:\Users\你的用户名\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1
       
       #或者New-Item -Path $PROFILE -Type File -Force
       ```

   2. 编辑配置文件

       ```
       code $PROFILE
       . $PROFILE
       ```

       

   3. PowerShell设置代理

       ```
       $env:http_proxy = "http://127.0.0.1:7890"
       $env:https_proxy = "http://127.0.0.1:7890"
       ```

   4. PowerShell设置变量路径

       ```
       # 用户环境变量
       # 导出用户环境变量为 PowerShell 代码
       #[Environment]::GetEnvironmentVariables('User').GetEnumerator() | ForEach-Object {
       #    '$env:{0} = "{1}"' -f $_.Key, $_.Value
       #} | Set-Content -Encoding UTF8 env_user.ps1
       
       # 系统环境变量
       # 导出系统环境变量为 PowerShell 代码
       #[Environment]::GetEnvironmentVariables('Machine').GetEnumerator() | ForEach-Object {
       #    '$env:{0} = "{1}"' -f $_.Key, $_.Value
       #} | Set-Content -Encoding UTF8 env_machine.ps1
       ```

   5. 安装[oh my posh](https://ohmyposh.dev/)

   6. 安装[powerlevel10k](https://github.com/romkatv/powerlevel10k)

4. [安装Miniforge](https://github.com/conda-forge/miniforge/releases)

   - 为PowerShell配置文件添加Miniforge路径

     ```
     # 设置 Miniforge PATH 环境变量
     $env:PATH = "C:\Users\你的用户名\AppData\Local\miniforge3"
     $env:PATH = "C:\Users\你的用户名\AppData\Local\miniforge3\Scripts"
     $env:PATH = "C:\Users\你的用户名\AppData\Local\miniforge3\Library\bin"
     ```
     
   - 更新conda`conda update -n base -c conda-forge conda`

   - 环境配置

     ```
     # 使用配置文件配置环境
     conda env create -f environment.yml 
     pip install -r requirements.txt
     
     # 生成环境配置
     pip install pipreqs
     pipreqs .   # 当前目录是 Git 代码库
     
     # 设置环境变量
     conda env config vars set {name}={key}
     
     
     ```

     

5. 输入法

   - [安装RIME + 雾凇拼音](https://sspai.com/post/89281)
   - [雾凇拼音改双拼](https://lbqaq.top/p/rime-sync/)

6. Caps键切换大小写

    ```
    patch:
      schema_list:
        - schema: double_pinyin_flypy
      menu/page_size: 9
      ascii_composer/good_old_caps_lock: false
      ascii_composer/switch_key/Caps_Lock: commit_code
      ascii_composer/switch_key/Shift_L: noop
    ```

7. [更改ALT与Ctrl，速览](https://sspai.com/post/90212)

    ```
    Ctrl & Tab:: AltTab
    
    $^Space:: Send "#^{Space}"
    
    #HotIf WinActive("ahk_exe explorer.exe")
    Space::
    {
    if ((A_PriorKey == "UP") or (A_PriorKey == "Right") or (A_PriorKey == "Down") or (A_PriorKey == "Left") or (A_PriorKey == "LButton") or (A_PriorKey == "Enter") or (A_PriorKey == "Escape") or (A_PriorKey == "Tab") or (A_PriorKey == "Space"))
    {
    Send "^+{p}"
    }
    else
    {
    Send "{Space}"
    }
    }
    
    #HotIf WinActive("ahk_exe PowerToys.Peek.UI.exe")
    Space:: Send "^+{p}"
    ^w:: Send "^+{p}"
    ```

    8. GetStoreApp
       - [获取商店应用](https://github.com/Gaoyifei1011/GetStoreApp)
