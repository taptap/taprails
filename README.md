# TapRails - TapTap PC 游戏包体上传工具

TapRails 是为 TapTap 平台上传 PC 游戏包体的工具，提供命令行版本和图形界面版本。

## 🚀 快速开始

### 环境要求

- Go 1.23+
- Node.js & npm (仅 GUI 版本)
- Wails 3.0 CLI (仅 GUI 版本): `go install -v github.com/wailsapp/wails/v3/cmd/wails3@latest`

### CLI 版本

```bash
# 构建
make build-cli

# 使用
./build/taprails-cli upload --file game.zip --package-version 1.0.0 --config config.json --verbose
```

### GUI 版本

```bash
# 开发模式
wails3 task dev

# 生产构建
wails3 task build

# 运行构建后的应用
wails3 task run
```

## ⚙️ 配置

**必须** 创建 `config.json` 配置文件：

```json
{
    "app_id": 12345,
    "client_id": "从开发者中心获取的Client_ID",
    "server_secret": "从API管理获取的server_secret",
    "chunk_size": 104857600
}
```

或使用环境变量：

```bash
export TAPTAP_APP_ID="12345"
export TAPTAP_CLIENT_ID="从开发者中心获取的Client_ID"
export TAPTAP_SERVER_SECRET="从API管理获取的server_secret"
```

### 📦 支持的文件格式

- **支持格式**: `.7z`、`.zip`
- **命名规则**: 文件名只能包含字母、数字、下划线(_)、中横线(-)和点号(.)
- **分片上传**: 支持大文件分片上传，默认分片大小 100MB，可在配置文件中调整

### 📝 版本号规范

版本号必须符合 [语义化版本控制 (semver)](https://semver.org/lang/zh-CN/) 规范：

## 📋 命令参数

| 参数                | 缩写 | 必传 | 默认值  | 说明                                |
| ------------------- | ---- | ---- | ------- | ----------------------------------- |
| `--file`            | `-f` | ✅    | 无      | 要上传的文件路径 (.7z/.zip)         |
| `--package-version` | `-p` | ✅    | 无      | 游戏包体版本号 (须符合 semver 规范) |
| `--config`          | `-c` | ✅    | 无      | 配置文件路径 (必须指定)             |
| `--verbose`         | `-v` | ❌    | `false` | 详细输出                            |

## 🔄 上传功能特性

- **智能上传模式**: 根据文件大小自动选择简单上传或分片上传
- **实时进度条**: 分片上传时显示实时进度条和上传状态
- **断点续传**: 上传失败时自动清理，避免资源泄漏
- **回调支持**: 完整支持 TapTap 平台的上传回调机制
- **CI 友好**: 错误输出包含明确的 ERROR 标识，便于 CI 集成

## 🔧 构建命令

### CLI 版本

- `make build-cli` - 构建CLI版本 ✅ **推荐**
- `make clean` - 清理构建文件

### GUI 版本 (Wails 3.0)

- `wails3 task build` - 构建GUI版本
- `wails3 task dev` - GUI开发模式
- `wails3 task run` - 运行构建后的应用
- `wails3 task package` - 打包应用程序

## 📁 项目结构

```
taprails/
├── cmd/cli/main.go        # CLI 入口
├── main-gui.go            # GUI 入口 (Wails 3.0)
├── app-gui.go             # GUI 应用逻辑
├── pkg/                   # 共享业务逻辑
│   ├── config/            # 配置管理
│   ├── taptap/            # TapTap API 客户端
│   └── uploader/          # 文件上传器
├── frontend/              # GUI 前端 (Vue 3 + TypeScript)
├── build/                 # Wails 3.0 构建配置
│   ├── config.yml         # 应用配置
│   ├── Taskfile.yml       # 通用构建任务
│   └── darwin/            # macOS 特定配置
├── Taskfile.yml           # 主构建任务文件
├── release/               # 发布文档
│   ├── README.md          # 用户文档
│   └── USAGE.md           # 使用说明
└── bin/                   # 构建输出
```

## 🆕 Wails 3.0 升级说明

项目已升级到 Wails 3.0 Alpha，主要变化：

### 环境要求

- **Go 版本**: 1.23+ (必需)
- **Wails CLI**: 3.0 Alpha (`wails3` 命令)
- **前端技术栈**: Vue 3 + TypeScript + Vite

## 📤 输出示例

### 成功上传（分片上传）
```
Uploading large-game.7z (250.25 MB) for app 12345...
Getting upload token...
Uploading file...
Using multipart upload (file size: 250.25 MB, chunk size: 100.00 MB)
Uploading chunks [████████████████████████████████████░░░░░░░░░░░░░░] 66.7% (2/3)
Completing multipart upload...
File uploaded successfully
```

## ❗ 常见问题

### Go 版本兼容性
如果遇到编译错误：
```bash
go clean -cache && go clean -modcache
go mod download && go mod tidy
```

### Wails 3.0 相关

确保安装了正确的 Wails 版本：

```bash
# 安装 Wails 3.0 Alpha
go install -v github.com/wailsapp/wails/v3/cmd/wails3@latest

# 检查版本
wails3 doctor
```

### IDE 配置

如果 IDE 显示 `undefined: NewApp` 错误，需要配置构建标签：

- VS Code: 在设置中添加 `"go.buildTags": "wails"`
- 其他 IDE: 在 Go 语言设置中添加构建标签 `wails`

## 🚀 发布流程

1. **构建所有平台版本**:

   ```bash
   # CLI 版本
   make build-cli
   
   # GUI 版本 (各平台)
   wails3 task build
   wails3 task package
   ```

2. **准备发布文件**:
   - 将构建产物打包为对应平台的 zip 文件
   - 复制 `release/` 目录下的文档到发布包

3. **GitHub Release**:
   - 创建新的 Release
   - 上传各平台的二进制包
   - 包含 `release/README.md` 和 `release/USAGE.md` 作为发布说明
