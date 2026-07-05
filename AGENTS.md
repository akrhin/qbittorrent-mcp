# AGENTS.md — инструкции для AI-ассистента

## О проекте

**qbittorrent-mcp** — MCP-сервер для управления qBittorrent. Форк [pickpppcc/qbittorrent-mcp](https://github.com/pickpppcc/qbittorrent-mcp).

**Деплой:** запущен как MCP-сервер через Hermes gateway. qBittorrent Web UI на `192.168.1.156:8080`.

**Hermes config:**
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

## Запуск

Сервер использует `uv` для запуска (не venv, не pip). Код в `main.py`.

## Инструменты

| Инструмент | Описание |
|-----------|----------|
| `get_torrent_list` | Список торрентов с прогрессом |
| `search_torrents` | Поиск по трекерам (с фильтром max_size_gb) |
| `add_torrent` | Добавить по .torrent файлу |
| `pause_torrent` / `resume_torrent` | Пауза/возобновление |
| `delete_torrent` | Удалить (с файлами или без) |
| `add_trackers_to_torrent` | Добавить трекеры |
| `add_torrent_tags` | Теги |
| `set_file_priority` | Приоритеты файлов (0=не качать, 1=норм, 6=высокий, 7=макс) |
| `set_*_limit` | Ограничения скорости (глобальные/на торрент) |

## Сеть

qBittorrent висит на другом хосте (hermes-server, 192.168.1.156). Убедись что хост доступен перед вызовом — `ping -c1 -W2 192.168.1.156`.

## Поиск

```python
search_torrents(pattern, category="all", plugins="all", max_size_gb=5)
```

Возвращает топ-10 по сидам, отфильтрованные по размеру. Плагин поиска — стандартные qBittorrent search plugins.
