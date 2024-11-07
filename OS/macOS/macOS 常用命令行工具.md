# macOS 实用命令行工具

## 1. Keychain 访问

```bash
# 通过security命令访问钥匙串中的密码
security find-internet-password -s "https://example.com"

# 添加新密码到钥匙串
security add-generic-password -a "username" -s "service_name" -w "password"

# 删除密码
security delete-generic-password -a "username" -s "service_name"

# 列出所有钥匙串
security list-keychains

# 导出证书
security export -k login.keychain -t certs -f pkcs12 -o output.p12
```

## 2. 文件操作

```bash
# 使用默认应用打开文件
open file.txt

# 使用指定应用打开文件
open -a "Visual Studio Code" file.txt

# 打开当前目录
open .

# 打开URL
open https://www.google.com

# 显示文件信息
mdls file.txt

# 快速查看文件
qlmanage -p file.jpg

# 显示文件类型
file document.pdf

# 创建软链接
ln -s target_path link_path

# 批量重命名
for f in *.txt; do mv "$f" "${f%.txt}.md"; done

# 文件权限修改
chmod +x script.sh
chown user:group file.txt
```

## 3. 剪贴板操作

```bash
# 复制到剪贴板
echo "Hello, world!" | pbcopy

# 从剪贴板粘贴
pbpaste

# 复制文件内容到剪贴板
cat file.txt | pbcopy

# 将剪贴板内容保存到文件
pbpaste > output.txt

# 复制并计数
pbpaste | wc -l

# 复制并格式化 JSON
pbpaste | python -m json.tool | pbcopy
```

## 4. 时间日期操作

```bash
# 查看UTC时间
date -u

# 使用指定时区
TZ=UTC date
TZ=Asia/Shanghai date

# 格式化输出
date "+%Y-%m-%d %H:%M:%S"
date "+%A, %B %d, %Y"    # 星期几，月份 日期，年份

# 显示日历
cal
cal 2024    # 显示整年日历
cal -3      # 显示上月、当月和下月

# 设置系统时间（需要 sudo）
sudo date MMDDHHmm[[CC]YY][.ss]

# 显示 UNIX 时间戳
date +%s
```

## 5. 网络工具

```bash
# 测试网络质量
networkQuality  # 注意大写Q

# 查看网络接口信息
networksetup -listallhardwareports
networksetup -getinfo Wi-Fi
networksetup -getairportnetwork en0

# DNS查询
dig example.com
dig +short example.com
nslookup example.com

# 端口扫描
nc -zv localhost 80
lsof -i :8080

# 网络监控
sudo tcpdump -i any port 80
sudo tcpdump host example.com

# Wi-Fi 操作
networksetup -setairportpower en0 on
networksetup -setairportnetwork en0 "SSID" "password"

# 查看路由表
netstat -r
route get example.com

# HTTP 请求
curl -I https://example.com    # 只看头信息
curl -o output.txt https://example.com    # 下载到文件
wget https://example.com/file  # 需要先安装wget
```

## 6. 系统控制

```bash
# 防止Mac休眠
caffeinate
caffeinate -t 3600  # 指定时间(秒)
caffeinate -i       # 防止空闲休眠
caffeinate -d       # 防止显示器休眠
caffeinate -m       # 防止磁盘休眠

# 音量控制
osascript -e "set volume 5"    # 0-10
osascript -e "set volume output muted true"
osascript -e "set volume output volume 50"  # 0-100

# 系统通知
osascript -e 'display notification "Hello" with title "Title"'
osascript -e 'display alert "Warning" message "Low Battery"'

# 电源管理
pmset -g batt      # 电池状态
pmset -g ps        # 电源状态
sudo pmset sleep 0  # 禁用睡眠

# 系统信息
system_profiler SPHardwareDataType  # 硬件信息
system_profiler SPSoftwareDataType  # 软件信息
system_profiler SPUSBDataType       # USB设备信息

# 启动台管理
defaults write com.apple.dock ResetLaunchPad -bool true; killall Dock  # 重置启动台

# Spotlight 索引
sudo mdutil -i on /    # 开启索引
sudo mdutil -i off /   # 关闭索引
sudo mdutil -E /       # 重建索引
```

## 7. 开发工具

```bash
# 生成UUID
uuidgen
uuidgen | tr '[:upper:]' '[:lower:]' | pbcopy

# Base64编解码
echo "Hello" | base64
echo "SGVsbG8K" | base64 -d

# JSON处理
echo '{"name": "John"}' | plutil -convert json -o - -
echo '{"name": "John"}' | python -m json.tool

# 计算文件哈希
shasum -a 256 file.txt
md5 file.txt

# 代码统计
find . -name "*.swift" | xargs wc -l

# 编译
xcrun clang hello.c -o hello

# 代码签名
codesign -s "Developer ID" application.app

# 查看代码签名
codesign -vv -d application.app

# 证书管理
security find-identity -v -p codesigning
```

## 8. 文件搜索

```bash
# Spotlight搜索
mdfind -name "file.txt"
mdfind "kind:pdf date:yesterday"
mdfind "kMDItemContentType == 'com.adobe.pdf'"

# 按时间搜索
find . -mtime -1 -type f     # 最近24小时修改的文件
find . -atime -7 -type f     # 最近7天访问的文件
find . -ctime +30 -type f    # 30天前创建的文件

# 按大小搜索
find . -size +100M           # 大于100MB的文件
find . -size -1M             # 小于1MB的文件

# 按权限搜索
find . -perm 644            # 权限为644的文件

# 内容搜索
grep -r "search text" .
grep -l "pattern" *.txt     # 只显示文件名
ack "pattern"               # 更好的grep(需要安装)
```

## 9. 多媒体工具

```bash
# 文字转语音
say "Hello World"
say -v "?"                     # 列出所有可用声音
say -v Ting-Ting "你好"        # 使用中文声音
say -f input.txt -o output.aiff  # 保存到文件
say -r 200 "Fast speaking"     # 调整语速

# 截图工具
screencapture -c              # 截图到剪贴板
screencapture image.jpg       # 截图保存为文件
screencapture -T 10 -x screenshot.jpg  # 延时10秒截图
screencapture -W             # 截取窗口
screencapture -i             # 交互式截图
screencapture -R "0,0,100,200" out.jpg  # 指定区域截图

# 图片处理
sips -z 100 100 image.jpg    # 调整图片大小
sips -r 90 image.jpg         # 旋转图片
sips --getProperty format image.jpg  # 获取图片格式
sips -s format jpeg image.png        # 转换格式
sips --resampleHeightWidth 100 100 image.jpg  # 重采样

# 音频控制
afplay sound.mp3            # 播放音频文件
afplay -t 10 music.mp3      # 播放10秒
afinfo audio.mp3            # 显示音频文件信息
```

## 10. 性能监控

```bash
# 进程管理
top                         # 实时系统状态
top -o cpu                  # 按CPU使用率排序
top -o rsize               # 按内存使用排序
ps aux | grep "process"    # 查找特定进程
kill -9 PID               # 强制结束进程

# 系统负载
w                          # 查看系统负载
uptime                     # 显示系统运行时间和负载
vm_stat                    # 虚拟内存统计
iostat                     # IO统计信息

# 内存使用
memory_pressure            # 内存压力测试
purge                      # 清除内存缓存

# 磁盘使用
df -h                      # 显示磁盘使用情况
du -sh *                   # 显示当前目录下各文件大小
du -h -d 1                 # 只显示一级目录大小
diskutil list              # 列出所有磁盘
diskutil info disk0s1      # 显示分区信息

# 网络监控
netstat -an | grep LISTEN  # 查看监听端口
nettop                     # 网络使用监控
```

## 11. 系统维护

```bash
# 软件更新
softwareupdate --list                  # 列出可用更新
softwareupdate --install --all         # 安装所有更新
softwareupdate --download --all        # 下载所有更新
softwareupdate --history               # 显示更新历史

# 磁盘维护
diskutil verifyVolume /                # 验证磁盘
diskutil repairVolume /                # 修复磁盘
diskutil secureErase freespace 0 /     # 安全擦除可用空间

# 系统清理
sudo periodic daily weekly monthly     # 运行系统维护脚本
sudo tmutil listlocalsnapshots /       # 列出本地Time Machine快照
sudo tmutil deletelocalsnapshots /     # 删除本地快照

# 系统日志
log show --last 1h                     # 显示最近1小时日志
log stream                             # 实时查看日志
log show --predicate 'process == "kernel"'  # 查看内核日志
```

## 12. 自动化工具

```bash
# Automator命令行
automator -i input.txt workflow.workflow  # 运行工作流

# launchctl服务管理
launchctl list                           # 列出所有服务
launchctl load ~/Library/LaunchAgents/com.example.app.plist   # 加载服务
launchctl unload ~/Library/LaunchAgents/com.example.app.plist # 卸载服务
launchctl start com.example.app          # 启动服务
launchctl stop com.example.app           # 停止服务

# cron任务
crontab -l                               # 列出定时任务
crontab -e                               # 编辑定时任务

# Shell脚本
defaults write com.apple.finder AppleShowAllFiles YES  # 显示隐藏文件
defaults write com.apple.dock persistent-apps -array   # 清空Dock
killall Finder                           # 重启Finder
```

## 13. 开发者工具

```bash
# Xcode命令行工具
xcode-select --install                   # 安装命令行工具
xcrun simctl list                        # 列出所有模拟器
xcrun simctl boot "iPhone 13"            # 启动模拟器

# 编译工具
gcc -o output input.c                    # 编译C程序
clang++ -o output input.cpp              # 编译C++程序

# 调试工具
lldb ./program                           # 启动调试器
sample PID                               # 进程采样分析

# 代码签名
codesign --verify --verbose /Applications/App.app  # 验证签名
```

## 14. 实用技巧

1. 命令组合

```bash
# 查找大文件并排序
du -sh * | sort -hr

# 监控日志变化
tail -f /var/log/system.log

# 批量重命名
for f in *.jpeg; do mv "$f" "${f%.jpeg}.jpg"; done
```

2. 别名设置

```bash
# 在 ~/.zshrc 或 ~/.bash_profile 中添加
alias ll='ls -la'
alias ip='curl ipinfo.io'
alias cleanup='sudo periodic daily weekly monthly'
```

3. 环境变量

```bash
# 查看所有环境变量
env

# 设置环境变量
export PATH=$PATH:/usr/local/bin
```

4. 快捷键

- Ctrl + A: 移动到行首
- Ctrl + E: 移动到行尾
- Ctrl + U: 清除当前行
- Ctrl + L: 清屏

## 15. 故障排除

1. 权限问题

```bash
sudo chown -R $(whoami) /usr/local
```

2. 进程问题

```bash
ps aux | grep "process"
sudo lsof -i :8080
```

3. 网络问题

```bash
ping example.com
traceroute example.com
```

4. 磁盘问题

```bash
diskutil verify /
fsck -fy /dev/disk0s1
```

## 参考资源

1. man 页面
2. Apple 开发者文档
3. Stack Overflow
4. GitHub 示例

## 注意事项

1. 使用 sudo 时要格外小心
2. 重要数据要备份
3. 测试命令建议先在非生产环境运行
4. 注意命令的版本差异
