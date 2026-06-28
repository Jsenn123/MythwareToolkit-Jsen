# MythwareToolkit

> **[更新日志](CHANGELOG.md)** | **[开发者文档](DEV.md)**

>有问题请加Q群：828869154

极域工具包，支持极域以及学生机房管理助手的工具。

## 界面展示


<img src="https://github.com/Jsenn123/Picture_bed/blob/main/img/20260628151116867.png" width="380" align="left">
<img src="https://github.com/Jsenn123/Picture_bed/blob/main/img/20260628151116866.png" width="180" align="left">
<img src="https://github.com/Jsenn123/Picture_bed/blob/main/img/20260628151116868.png" width="180" align="left">

<br clear="left">
*主界面（左） · 任务栏托盘（中） · 悬浮窗右键菜单（右）*

**当前版本：v1.3.0**

---

## 目录

- [功能](#功能)
- [使用教程](#使用教程)
- [附录](#附录)

---

## 功能

### v1.3.0

- **圆形悬浮窗**：始终置顶，左键切换主面板，中键一键广播窗口化，右键快捷菜单（防截屏），支持拖拽
- **退出黑屏**：主界面按钮 / 悬浮窗右键，4 级递进（隐藏 → 最小化 → ESC → 确认杀进程）
- **全面防截屏**：主窗口、悬浮窗、所有弹窗和菜单对教师端监控不可见
- **UAC 提权**：网络限制解除、MeltdownDFC 点击即弹提权窗口
- **崩溃诊断**：完整寄存器 + 栈回溯写入崩溃日志，运行日志记录每一步操作
- **日志系统**：`MythwareToolkit_run.log` + `MythwareToolkit_crash.log`，追加模式

### 极域控制

- 支持不依赖`taskkill`、`ntsd`等工具杀掉极域。极域未运行时可启动极域，降权到登录用户（路径来自注册表）
- 显示极域存活状态：未运行/正常运行/已挂起/无响应 + PID + 版本号
- 解除极域网络限制（黑/白名单或直接禁用，2016版测试通过，可验证至2021版）
- 解除极域U盘限制，两种方式（软解禁 / 硬解禁）
- 窗口化/全屏化极域广播（悬浮窗右键菜单或中键一键操作）
- 挂起（冻结）/恢复极域
- 退出黑屏安静：4 级递进（隐藏 → 最小化 → ESC → 确认杀进程）
- 解鼠标限制、键盘锁（启动默认开启）
- 防止截屏（Win7+），防止教师端看到本程序

### 学生机房管理助手控制

- 支持关闭 v6.8 ~ v13.1 版本的学生机房管理助手
- 计算 v9.x ~ v12.0 临时密码（动态密码计算器）
- 一键解禁系统程序：CMD、注册表编辑器、任务管理器、浏览器下载限制、小游戏等
- 重启资源管理器（explorer.exe）

### 通用功能

- 内置 MeltdownDFC、crdisk 两个解除硬盘保护的工具
- 快捷键：
  - <kbd>Alt</kbd>+双击<kbd>C</kbd> — 强制结束当前程序
  - <kbd>Alt</kbd>+<kbd>B</kbd> — 唤起主界面
  - <kbd>Alt</kbd>+<kbd>W</kbd> — 最小化当前窗口
- 支持超级置顶（UIAccess），覆盖任务管理器和放大镜
- 托盘图标常驻，最小化不占任务栏

---

## 使用教程

#### 两个版本怎么选

- 优先尝试 `MythwareToolkit.zip` 
- 如果直接双击里面的 `MythwareToolkit.exe` 不可行
- 尝试双击 `deploy.bat` 安装证书
- 如果安装证书后还不可行，安装 `MythwareToolkit_Portable.exe`

### 便携版

下载 `MythwareToolkit_Portable.exe`，**双击运行**即可。

- ✅ 免安装、免签名
- ✅ 悬浮窗 + 完整功能
- ⚠️ 不能覆盖任务管理器（普通置顶）

### 超级置顶版（可覆盖任务管理器）

> 下载的文件包应包含：`MythwareToolkit.exe` + `deploy.bat` + `mythware.cer`
> ⚠️如果双击 `MythwareToolkit.exe` 可直接打开
> ⚠️则不需要下列几个步骤

**1. 首先**，双击 `deploy.bat` 。它会自动：

- 安装证书到系统受信任根
- 复制 `MythwareToolkit.exe` 到 `C:\Program Files\MythwareToolkit\`
- 创建桌面快捷方式

**2. 之后**从桌面快捷方式启动即可。

> **为什么必须用 deploy.bat？** UIAccess 是 Windows 安全机制：exe 必须已签名、证书必须受信任、**且 exe 必须在 `C:\Program Files\` 下**。放桌面或别处会一直弹窗，即使装了证书也没用。`deploy.bat` 一步搞定这三个条件。

> 证书在非自动恢复电脑上只需装一次
>正常机房可能本身已经关闭 UAC，不需要装证书



---

## 附录

<details>

### 防止教师端强制关机

删除极域目录下的 `Shutdown.exe` 即可。

### 命令行解除极域U盘限制

CMD：
```powershell
sc stop TDFileFilter
sc delete TDFileFilter
```

PowerShell（CMD被禁用时）：
```powershell
cd C:\Windows\System32\
.\sc.exe stop TDFileFilter
.\sc.exe delete TDFileFilter
```

### 学生机房管理助手的软件黑名单（v10.1）

进程名包含这些词就会蓝屏（加粗的匹配进程名+窗口名）：

vmware、VirtualBox、Virtual PC、**虚拟机**、**电子教室**、ProcView、IceSword、Procmast.exe、**toolkit_32-bits.exe**、rstray.exe、PFW.exe、FTCleaner.exe、Wsyscheck.exe、XueTr.exe、prom.exe、ProcessX.exe、pchunter、**Killer.exe**、procmgr.exe、ProcessHacker.exe、killcontrol、PowerTool32.exe、360taskmgr、YtWinAst、KVFWMain.exe、ECQ-PS.exe、SnipeSword、procexp、**MsgFlood.exe**、ProcessOVER、procdeal、**多桌面**、**任务管理**、**进程**、Prayaya、dexpot.exe、vdeskman.exe、mdesk.exe、**virtualdesk**、multideskt.exe、VirDsk.exe、IDesktop.exe、YtMDesk.exe、coon.exe、zmqh.exe、DexpotProPortable.exe、Desktops.exe、wisedesktop.exe、DESKTOP.exe、Vdesktop.exe、MagicDesktop.exe、multidesktop.exe、**weRs0cqa**（蓝屏时还会自动杀掉该名进程）、RegWX64.exe、QQPCNetFlow.exe、BDMANetLimiter.exe、netmon.exe、360netman.exe、HelloTeacher.exe、EHacker.exe、PowerTool64.exe、zydesk.exe、perfmon.exe、**吾爱破解**、**极域**、prcview.exe、processlasso.exe、netfuke.exe、**去除控制**、**课堂狂欢器**、**课堂工具**、fuckmythware、SpecialSet.exe、JiYuTrainer.exe、skieskiller、WindowsKernelExplorer.exe、msconfig.exe、iu杀毒、**窗口拓印**

此外任务管理器会触发独特的锁定蓝屏界面。

### 助手临时密码算法（v9.x ~ v12.0）

1. 10.0 前：首位为 8，后面为 `16 × (年 × 91 + 月 × 13 + 日 × 57)`
2. 10.0 ~ 11.0：上面结果 +11
3. 11.0 ~ 11.06 首个发布版：`年 × 789 + 月 × 123 + 日 × 456 + 111`
4. 11.06 第三版 ~ 12.0：`(月 × 159 + 日 × 357 + 计算机名末位 ASCII × 258)` 转 7 进制

使用本程序的"动态密码计算器"可直接计算。

### 在线生成机房助手密文（v7.2 ~ v9.98）

访问 [try.dot.net](https://try.dot.net/)，运行以下 C# 代码，将输出写入注册表 `HKEY_CURRENT_USER\Software:n`（REG_SZ），机房助手密码立即更改：

```csharp
using System;
using System.Security.Cryptography;
using System.Text;
using System.IO;

public class Program
{
    public static void Main()
    {
        string string_3 = "12345678"; // 改成你的密码
        string value = "C:\\WINDOWS";
        string s = value.Substring(0, 8);
        string s2 = value.Substring(1, 8);
        DESCryptoServiceProvider desc = new DESCryptoServiceProvider();
        desc.Key = Encoding.UTF8.GetBytes(s);
        desc.IV = Encoding.UTF8.GetBytes(s2);
        MemoryStream ms = new MemoryStream();
        CryptoStream cs = new CryptoStream(ms, desc.CreateEncryptor(), CryptoStreamMode.Write);
        StreamWriter sw = new StreamWriter(cs);
        sw.Write(string_3); sw.Flush(); cs.FlushFinalBlock(); ms.Flush();
        string enc = Convert.ToBase64String(ms.GetBuffer(), 0, (int)ms.Length);
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < enc.Length; i++) sb.Append((char)(enc[i] - 10));
        MD5CryptoServiceProvider md5 = new MD5CryptoServiceProvider();
        byte[] hash = md5.ComputeHash(Encoding.Default.GetBytes(sb.ToString()));
        sb.Clear();
        for (int i = 0; i < hash.Length; i++) sb.Append(hash[i].ToString("x2"));
        Console.WriteLine(sb.ToString().Substring(10));
    }
}
// 期望输出：8a29cc29f5951530ac69f4（v9.99 以上为 8a29cc29f5951530ac69）
```

### 重要提醒

若出现"从服务器返回了一个参照"弹窗，说明 EXE 未签名或证书未安装。运行 `scripts\sign.bat` 一步解决。或手动导入 `cert\RootCA.reg`（原版证书）/ `cert\mythware.cer`（本版证书）。详见 `cert\从服务器返回一个参数解决方法.txt`。机房环境建议关闭 UAC。

</details>
