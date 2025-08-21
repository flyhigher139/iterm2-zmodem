# iTerm2 ZModem 文件传输配置

在 macOS 下，通过配置 iTerm2 和 ZModem 协议，实现与远程服务器进行便捷的文件上传和下载操作。无需额外的 FTP 或 SCP 命令，直接在终端中使用 `rz` 和 `sz` 命令即可完成文件传输。

## 功能特性

- 🚀 **便捷传输**：直接在 SSH 会话中使用 `rz`/`sz` 命令
- 📁 **图形化选择**：通过 macOS 原生文件选择器选择文件/文件夹
- ⚡ **自动触发**：基于 iTerm2 的 Trigger 功能自动识别传输请求
- 🔄 **双向传输**：支持上传（sz）和下载（rz）操作
- 💾 **二进制安全**：支持各种文件类型的传输

## 安装步骤

### 1. 安装 lrzsz 工具

首先安装支持 `rz` 和 `sz` 命令的 lrzsz 包：

```bash
brew install lrzsz
```

> 💡 **提示**：首次安装可能需要较长时间，请耐心等待。

### 2. 复制脚本文件

将项目中的两个脚本文件复制到系统路径：

```bash
# 复制发送文件脚本
sudo cp iterm2-send-zmodem.sh /usr/local/bin/

# 复制接收文件脚本
sudo cp iterm2-recv-zmodem.sh /usr/local/bin/
```

### 3. 设置脚本权限

为脚本文件添加执行权限：

```bash
sudo chmod +x /usr/local/bin/iterm2-send-zmodem.sh
sudo chmod +x /usr/local/bin/iterm2-recv-zmodem.sh
```

> ⚠️ **安全提示**：建议使用 `chmod +x` 而不是 `chmod 777`，避免给予过多权限。

### 4. 配置 iTerm2 Triggers

在 iTerm2 中配置触发器以自动识别文件传输请求：

1. 打开 iTerm2 偏好设置：`iTerm2` → `Preferences` (或按 `⌘,`)
2. 选择 `Profiles` → `Default` → `Advanced`
3. 找到 `Triggers` 部分，点击 `Edit` 按钮
4. 添加以下两个触发器：

#### 触发器配置

**第一条触发器（文件上传 - sz 命令）：**
- **Regular Expression**: `rz waiting to receive.\*\*B0100`
- **Action**: `Run Silent Coprocess`
- **Parameters**: `/usr/local/bin/iterm2-send-zmodem.sh`
- **Instant**: ✅ 勾选

**第二条触发器（文件下载 - rz 命令）：**
- **Regular Expression**: `\*\*B00000000000000`
- **Action**: `Run Silent Coprocess`
- **Parameters**: `/usr/local/bin/iterm2-recv-zmodem.sh`
- **Instant**: ✅ 勾选

![配置示例](imgs/01.png)

> 📝 **说明**：这些触发器会监听终端输出，当检测到 ZModem 传输协议的特定字符串时，自动启动相应的脚本。

## 使用方法

配置完成后，即可在 SSH 连接的远程服务器上使用以下命令：

### 上传文件到服务器（sz 命令）

在远程服务器终端中输入：

```bash
sz filename
```

- 系统会自动弹出 macOS 文件选择器
- 选择要上传的文件
- 文件将自动传输到服务器当前目录

### 从服务器下载文件（rz 命令）

在远程服务器终端中输入：

```bash
rz
```

- 系统会自动弹出 macOS 文件夹选择器
- 选择要保存文件的本地目录
- 服务器上的文件将自动下载到指定目录

## 注意事项

- ⚠️ **服务器要求**：远程服务器必须安装 `lrzsz` 包（`yum install lrzsz` 或 `apt-get install lrzsz`）
- 🔒 **权限问题**：确保脚本文件具有执行权限
- 📁 **路径问题**：脚本路径必须为 `/usr/local/bin/`，否则需要修改触发器参数
- 🌐 **网络稳定**：大文件传输时建议保持网络连接稳定
- 📝 **文件名限制**：避免使用包含特殊字符的文件名

## 故障排除

### 问题：触发器不工作

**解决方案：**
1. 检查触发器配置是否正确
2. 确认脚本文件路径和权限
3. 重启 iTerm2 应用

### 问题：文件传输中断

**解决方案：**
1. 检查网络连接稳定性
2. 确认服务器端 lrzsz 已正确安装
3. 尝试传输较小的文件测试

### 问题：权限被拒绝

**解决方案：**
```bash
# 重新设置脚本权限
sudo chmod +x /usr/local/bin/iterm2-*.sh

# 检查脚本是否存在
ls -la /usr/local/bin/iterm2-*.sh
```

## 脚本说明

### iterm2-send-zmodem.sh
- **功能**：处理文件上传（sz 命令触发）
- **工作流程**：检测 iTerm 版本 → 弹出文件选择器 → 使用 sz 命令发送文件

### iterm2-recv-zmodem.sh
- **功能**：处理文件下载（rz 命令触发）
- **工作流程**：检测 iTerm 版本 → 弹出文件夹选择器 → 使用 rz 命令接收文件

## 兼容性

- ✅ **macOS**: 10.12+ 
- ✅ **iTerm2**: 3.0+
- ✅ **iTerm**: 2.0+ (旧版本)
- ✅ **服务器**: 支持 lrzsz 的 Linux/Unix 系统

## 许可证

本项目基于开源协议，脚本文件来源于 GitHub 社区贡献。

---

**享受便捷的文件传输体验！** 🎉
