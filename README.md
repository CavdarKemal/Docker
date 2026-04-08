# Docker — Zentrale Container-Konfigurationen

Alle Docker-Compose-Dateien fuer die Projekte unter `E:\Projekte\ClaudeCode\` sind hier
zentral abgelegt, getrennt nach Projekt.

## Uebersicht

| Projekt | Verzeichnis | Container | Ports | Beschreibung |
|---------|-------------|-----------|-------|--------------|
| **activiti-process** | `activiti-process/` | TestSupportClient-activiti6, -activiti5 + je DB | 9090, 9091, 2525, 2526 | Activiti 5 + 6 BPMN-Engines fuer testsupport_client-Tests |
| **EmployeeManagement** | `EmployeeManagement/` | postgres, backend, frontend | 5432, 8082, 3000 | Spring Boot + React App (LIVE: em.cavdar.de) |
| **TemplateGUI** | `TemplateGUI/` | template-postgres | 5433 | PostgreSQL fuer TemplateGUI-Entwicklung |
| **ITSQ-Explorer** | `ITSQ-Explorer/` | template-postgres | 5432 | PostgreSQL fuer ITSQ-Explorer |
| **CTE-Dashboard-Spring** | `CTE-Dashboard-Spring/` | cte-dashboard-db, cte-dashboard-app | 5432, 8081 | Spring Boot Batch-App + PostgreSQL |
| **StandardMDIGUI** | `StandardMDIGUI/` | standardmdi-db, standardmdi-app | 5432 | Java Swing MDI + PostgreSQL |
| **CTE-Dashboard** | `CTE-Dashboard/` | cte-dashboard-db, backend, frontend | 5432, 4000, 3000 | Node.js Backend + Next.js Frontend |

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

```bash
# Activiti 5 + 6 (fuer testsupport_client-Tests)
cd docker/activiti-process
docker compose up -d
docker compose -f docker-compose-a5.yml up -d

# EmployeeManagement (Entwicklung)
cd docker/EmployeeManagement
docker compose up -d

# EmployeeManagement (Produktion)
cd docker/EmployeeManagement
docker compose -f docker-compose.yml -f docker-compose.prod.yml up -d

# TemplateGUI (nur Postgres)
cd docker/TemplateGUI
docker compose up -d

# ITSQ-Explorer (nur Postgres)
cd docker/ITSQ-Explorer
docker compose up -d

# CTE-Dashboard-Spring
cd docker/CTE-Dashboard-Spring
docker compose up -d

# StandardMDIGUI
cd docker/StandardMDIGUI
docker compose up -d

# CTE-Dashboard (Node.js)
cd docker/CTE-Dashboard
docker compose up -d
```

## Hinweise

- **Dockerfiles** bleiben in den jeweiligen Projekt-Repos (brauchen den Build-Kontext).
  Ausnahme: activiti-process — dort ist alles selbststaendig in diesem Verzeichnis.
- **Build-Pfade** in den Compose-Dateien zeigen via `../../<Projekt>/` zurueck auf die Quellverzeichnisse.
- **EmployeeManagement** benoetigt eine `.env`-Datei (liegt in `EmployeeManagement/`).
