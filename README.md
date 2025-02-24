### 在中柏EZpad 5SE上安装Ubuntu 24、解决BCM43430无线网卡驱动问题、添加必要软件、实现屏幕开关控制及通过NetworkManager连接WiFi

**日期**：2025年2月24日

#### 准备工作
- **下载资源**：获取Ubuntu 24的Live版本ISO镜像。
- **制作启动U盘**：使用Ventoy工具将U盘制作成可启动U盘，以便于安装Ubuntu。

#### 进入BIOS设置U盘启动
- 在开机出现第一屏启动信息界面时按键盘上的DEL键直接进入BIOS设置。
- 在BIOS中设置U盘为首选启动设备，确保可以从U盘启动进行系统安装。

#### 安装Ubuntu 24
- 使用制作好的启动U盘成功启动，并按照提示完成Ubuntu 24系统的安装过程。

#### 解决WiFi驱动问题（针对BCM43430）
详细步骤见前文。通过手动加载和配置正确的固件文件解决了BCM43430无线网卡的驱动问题，并且为了更好地管理和连接WiFi，额外安装了一系列网络相关软件包。

#### 添加软件以增强网络连接功能
为了更好地管理和连接WiFi，在Ubuntu 24中额外安装了以下软件包：
- `libbluetooth3_5.72-0ubuntu5_amd64`
- `libndp0_1.8-1fakesync1build1_amd64`
- `libnm0_1.46.0-1ubuntu2_amd64`
- `libpcsclite1_2.0.3-1build1_amd64`
- `libteamdctl0_1.31-1build3_amd64`
- `network-manager_1.46.0-1ubuntu2_amd64`
- `wpasupplicant_2.10-21build4_amd64`

这些软件包有助于提升系统的网络管理能力和安全性。

#### 实现屏幕的开关控制

为了实现屏幕的关闭与开启操作，使用以下命令（需要root权限）：

- **切换到root用户**：
  ```bash
  su
  ```

- **关闭屏幕命令**：
  ```bash
  setterm --blank force --term linux </dev/tty1
  ```
  执行此命令后，屏幕将被强制关闭。

- **打开屏幕命令**：
  ```bash
  setterm --blank poke --term linux < /dev/tty1
  ```
  使用该命令可以重新点亮屏幕。

**注意**：一旦屏幕被关闭，必须通过上述打开命令才能重新点亮屏幕。移动鼠标或点击键盘按键均无法自动恢复屏幕显示。

#### 使用NetworkManager连接WiFi

为了简化WiFi连接过程，使用`NetworkManager`来管理你的网络连接是一个不错的选择。以下是具体步骤：

1. **检查NetworkManager服务状态**：
   确保`NetworkManager`服务正在运行。你可以使用以下命令检查其状态：
   ```bash
   sudo systemctl status NetworkManager
   ```
   如果服务未运行，可以通过以下命令启动它：
   ```bash
   sudo systemctl start NetworkManager
   ```

2. **使用nmtui连接WiFi**：
   - 打开终端并输入`nmtui`进入文本用户界面(TUI)。
   - 使用方向键选择“激活连接”并按下回车键。
   - 选择你的WiFi网络SSID并按下回车键。
   - 输入WiFi密码，然后选择“确定”以连接。

3. **或者使用nmcli命令行工具连接WiFi**：
   首先，列出所有可用的WiFi网络：
   ```bash
   nmcli dev wifi list
   ```
   接着，连接到指定的WiFi网络（假设SSID是"MyHomeNetwork"，密码是"password123"）：
   ```bash
   nmcli dev wifi connect "MyHomeNetwork" password "password123"
   ```

4. **验证连接**：
   可以使用以下命令检查网络连接是否成功：
   ```bash
   ping qq.com
   ```



# 参考

[驱动bcm43430 sdio无线网卡 亲测ubuntu18.04可行_brcmfmac43430-CSDN博客](https://blog.csdn.net/anzhen9/article/details/80461772)

[linux 通过命令行(包括ssh)关闭屏幕 - dirgo - 博客园](https://www.cnblogs.com/dirgo/p/17376210.html)