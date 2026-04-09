# Docker — Backup der Container-Konfigurationen

Dieses Verzeichnis dient als **Backup** aller Docker-Konfigurationen.
Die primaeren Dateien liegen jeweils unter `<Projekt>/docker/` im jeweiligen Projekt-Repo.

## Uebersicht

| Projekt | Primaerer Ort | Container | Ports | Beschreibung |
|---------|---------------|-----------|-------|--------------|
| **activiti-process** | `activiti-process/docker/` | TestSupportClient-activiti6, -activiti5 + je DB | 9090, 9091, 2525, 2526 | Activiti 5 + 6 BPMN-Engines fuer testsupport_client-Tests |
| **EmployeeManagement** | `EmployeeManagement/` (Projekt-Root) | postgres, backend, frontend | 5432, 8082, 3000 | Spring Boot + React App (LIVE: em.cavdar.de) |
| **TemplateGUI** | `TemplateGUI/docker/` | template-postgres | 5433 | PostgreSQL fuer TemplateGUI-Entwicklung |
| **ITSQ-Explorer** | `ITSQ-Explorer/docker/` | template-postgres | 5432 | PostgreSQL fuer ITSQ-Explorer |
| **CTE-Dashboard-Spring** | `CTE-Dashboard-Spring/docker/` | cte-dashboard-db, cte-dashboard-app | 5432, 8081 | Spring Boot Batch-App + PostgreSQL |
| **StandardMDIGUI** | `StandardMDIGUI/docker/` | standardmdi-db, standardmdi-app | 5432 | Java Swing MDI + PostgreSQL |
| **CTE-Dashboard** | `CTE-Dashboard/docker/` | cte-dashboard-db, backend, frontend | 5432, 4000, 3000 | Node.js Backend + Next.js Frontend |

## Port-Konflikte beachten!

Mehrere Projekte nutzen Port **5432** (PostgreSQL). Nicht gleichzeitig starten:
- EmployeeManagement (5432)
- ITSQ-Explorer (5432)
- CTE-Dashboard-Spring (5432)
- StandardMDIGUI (5432)
- CTE-Dashboard (5432)

**Ausnahmen:**
- TemplateGUI nutzt Port **5433** (kein Konflikt mit EmployeeManagement)
- activiti-process nutzt **keine Host-Ports** fuer die DBs (nur intern)

## Schnellstart

Alle Befehle werden aus dem jeweiligen **Projekt-Verzeichnis** ausgefuehrt:

```bash
# Activiti 5 + 6 (fuer testsupport_client-Tests)
cd activiti-process/docker
docker compose up -d
docker compose -f docker-compose-a5.yml up -d

# EmployeeManagement (Entwicklung)
cd EmployeeManagement
docker compose up -d

# EmployeeManagement (Produktion, auch auf VPS)
cd EmployeeManagement
docker compose -f docker-compose.yml -f docker-compose.prod.yml up -d

# TemplateGUI (nur Postgres)
cd TemplateGUI/docker
docker compose up -d

# ITSQ-Explorer (nur Postgres)
cd ITSQ-Explorer/docker
docker compose up -d

# CTE-Dashboard-Spring
cd CTE-Dashboard-Spring/docker
docker compose up -d

# StandardMDIGUI
cd StandardMDIGUI/docker
docker compose up -d

# CTE-Dashboard (Node.js)
cd CTE-Dashboard/docker
docker compose up -d
```

## Hinweise

- Die Docker-Konfigurationen liegen primaer in den jeweiligen Projekt-Repos unter `docker/`.
  Ausnahme: **EmployeeManagement** hat die Compose-Dateien im Projekt-Root (VPS braucht sie dort).
- Dieses Verzeichnis (`E:\Projekte\ClaudeCode\docker\`) ist ein **Backup** — bei Aenderungen
  die primaere Stelle im Projekt aktualisieren.
- **EmployeeManagement** benoetigt eine `.env`-Datei im Projekt-Root.
- `.tar` und `.rpm` Dateien sind per `.gitignore` vom Git-Push ausgeschlossen (>100 MB GitHub-Limit),
  existieren aber lokal unter `activiti-process/`.
