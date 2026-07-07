# Changelog

## v1.2.0 (2026-07-07)

### 中文

#### Breaking Changes

- 不再读取 `TAPTAP_*` 环境变量；请改用命令行参数或 `config.json`
- 工具上传必须提供 `launch_exe` 和 `launch_args`，缺失时会拒绝上传

#### Changes

- 新增 PC 包启动配置支持：`--launch-exe` / `--launch-args`
- 新增命令行参数：`--app-id` / `--client-id` / `--server-secret` / `--chunk-size` / `--launch-exe` (`-e`) / `--launch-args` (`-a`)
- 命令行参数会覆盖 `config.json` 中的同名配置

### English

#### Breaking Changes

- `TAPTAP_*` environment variables are no longer read; use CLI flags or `config.json` instead
- Uploads must provide `launch_exe` and `launch_args`; missing values are rejected before upload

#### Changes

- Added PC package launch config support: `--launch-exe` / `--launch-args`
- Added CLI flags: `--app-id` / `--client-id` / `--server-secret` / `--chunk-size` / `--launch-exe` (`-e`) / `--launch-args` (`-a`)
- CLI flags override matching values in `config.json`

## v1.0.0 (2025-06-17)

- Initial public release
