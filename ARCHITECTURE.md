# qBittorrent MCP — Architecture

## Overview

MCP-сервер для управления qBittorrent Web UI. Использует qBittorrent API (v2). Запущен через uv, код в `main.py`.

## Components

```
Hermes ──→ qbittorrent-mcp (MCP) ──→ qBittorrent Web UI (192.168.1.156:8080)
```

## Инструменты

| Инструмент | Описание |
|-----------|----------|
| `get_torrent_list` | Список торрентов с прогрессом |
| `search_torrents` | Поиск по трекерам (с фильтром `max_size_gb`) |
| `add_torrent` | Добавить по `.torrent` файлу |
| `pause_torrent` / `resume_torrent` | Пауза/возобновление |
| `delete_torrent` | Удалить (с файлами или без) |
| `add_trackers_to_torrent` | Добавить трекеры |
| `add_torrent_tags` | Теги |
| `set_file_priority` | Приоритеты файлов (0=не качать, 1=норм, 6=высокий, 7=макс) |
| `set_*_limit` | Ограничения скорости (глобальные/на торрент) |

## Hermes config

```yaml
mcp_servers:
  qbittorrent:
    command: uv
    args:
    - --directory
    - /home/sintez/git/qbittorrent-mcp
    - run
    - main.py
    env:
      QBITTORRENT_HOST: http://192.168.1.156:8080
      QBITTORRENT_USERNAME: sintez
      QBITTORRENT_PASSWORD: <redacted>
    enabled: true
```

## Network

qBittorrent на отдельном хосте (hermes-server, 192.168.1.156). Доступен по HTTP — без TLS.

## Fork info

Форк [pickpppcc/qbittorrent-mcp](https://github.com/pickpppcc/qbittorrent-mcp). Добавлены теги, приоритеты файлов, ограничения скорости.
