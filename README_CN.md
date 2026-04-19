# PGD (Process Guardian Daemon)

**基於 Bash 的進程守護程序**

一個簡單但功能完整的 Bash 腳本，用於監控和管理系統進程。當被監控的進程意外終止時，PGD 會自動重啟它。

## 功能特色

- 🔄 **自動重啟**：監控指定進程，意外退出時自動重啟
- ⚙️ **動態配置**：支援 SIGHUP 信號熱更新配置文件
- 🛡️ **Systemd 整合**：可註冊為 systemd user service
- 📊 **狀態監控**：即時顯示 daemon 和所有監控任務的狀態
- 📝 **日誌記錄**：完整的運行日誌和錯誤記錄

## 安裝

### 方式一：安裝到系統 PATH

```bash
cd ~/Projects/pgd-git
./pgd install
```

安裝後可以在任何位置執行 `pgd` 指令。

### 方式二：作為 systemd 服務

```bash
cd ~/Projects/pgd-git
./pgd register
```

這會將 PGD 註冊為用戶級別的 systemd 服務，开机自启。

## 使用方法

### 基本操作

```bash
# 啟動守護進程
./pgd daemon

# 查看狀態
./pgd status

# 停止守護進程
./pgd stop

# 查看日誌
./pgd log
```

### 管理監控任務

```bash
# 新增監控任務
./pgd add <任務名稱> "<執行命令>" <檢查間隔秒數>

# 範例：監控 Node.js 應用（每 10 秒檢查一次）
./pgd add myapp "node /home/user/app.js" 10

# 監控 Redis 伺服器（每 5 秒檢查一次）
./pgd add redis "redis-server /etc/redis/redis.conf" 5

# 刪除監控任務
./pgd delete <任務名稱>

# 列出所有配置
./pgd list
```

### Systemd 服務管理

```bash
# 註冊為 systemd 服務
./pgd register

# 查看服務狀態
systemctl --user status pgd.service

# 停止服務
systemctl --user stop pgd.service

# 啟動服務
systemctl --user start pgd.service

# 取消註冊
./pgd unregister
```

## 命令說明

| 命令 | 說明 |
|------|------|
| `daemon` | 啟動監控守護進程 |
| `add <name> <cmd> <int>` | 新增監控任務 |
| `delete <name>` | 刪除監控任務 |
| `list` | 列出所有配置 |
| `log` | 查看運行日誌 |
| `status` | 顯示 daemon 和任務狀態 |
| `stop` | 停止 daemon |
| `register` | 註冊為 systemd 服務 |
| `unregister` | 取消註冊 systemd 服務 |
| `install` | 安裝到系統 PATH |

## 注意事項

- 配置文件使用 `:` 作為分隔符，命令中若包含 `:` 需特別處理
- 建議使用引號包裹命令，避免特殊字符問題
- Daemon 需要持續運行才能監控進程
- 首次使用前請確保有執行權限：`chmod +x pgd`
