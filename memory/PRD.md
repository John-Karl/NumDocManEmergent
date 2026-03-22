# NumDocMan - Product Requirements Document

## Project Overview
**NumDocMan** - Application web de gestion de la documentation numérique professionnelle, inspirée d'OpenDocMan, développée pour RealCMB Group.

**Last Updated**: 2025-02
**Version**: 1.0.0 MVP

---

## Architecture

### Tech Stack
- **Frontend**: React 18, Tailwind CSS, react-i18next, Recharts, Lucide React, Sonner
- **Backend**: FastAPI (Python), SQLAlchemy async, asyncpg
- **Database**: PostgreSQL 15 (`postgresql://numdocman:numdocman@localhost:5432/numdocman_localdev`)
- **Auth**: JWT (email/password) + Google OAuth (Emergent)
- **Storage**: Local filesystem (`/app/backend/uploads/`)
- **i18n**: react-i18next (FR + EN)

### Design
- **Primary Color**: `#2E60CC` (RealCMB blue)
- **Accent Color**: `#E50000` (RealCMB red)
- **Fonts**: Chivo (headings), IBM Plex Sans (body), IBM Plex Mono (code/IDs)
- **Logo**: RealCMB Group gear logo

---

## User Personas
1. **Super Administrateur**: Vue globale toutes organisations, gestion utilisateurs/rôles
2. **Administrateur Organisation**: Gestion des membres, rôles, configuration
3. **Chef de Projet**: Création de projets, configuration workflows, types documents
4. **Contributeur**: Upload documents, transitions workflow, signatures
5. **Observateur**: Lecture seule des documents

---

## Core Requirements (Static)
1. ✅ Multi-base de données SQL (PostgreSQL) - architecture extensible NoSQL
2. ✅ Structure documentaire par projet avec règles d'identification personnalisables
3. ✅ KPIs administrateur cross-projets et cross-organisations
4. ✅ Personnalisation des familles/types de documents et règles ID (format: OrgID_ProjID_Phase_DocType_00001)
5. ✅ KPIs personnalisables (tables kpi_configs)
6. ✅ Workflows personnalisés (états, transitions, rôles requis, signature/commentaire obligatoire)
7. ✅ Types de documents personnalisables (code, couleur, icône)
8. ✅ Rôles et droits d'accès (superadmin, admin org, membres projet)
9. ✅ Design moderne et épuré
10. ✅ Responsive design complet
11. ✅ Signature électronique (canvas dessiné + texte avec champs: Titre, Nom, Entreprise, Entité, Email, Date/Heure/Fuseau)
12. ✅ Stockage configurable (local par défaut, distant URL)
13. ✅ Interface bilingue FR/EN

---

## What's Been Implemented (2025-02)

### Backend Routes
- `POST/GET /api/auth/*` - Auth JWT + Google OAuth
- `GET/POST /api/organizations` - CRUD organisations
- `GET/POST/PUT/DELETE /api/projects/*` - CRUD projets + membres
- `GET/POST/PUT/DELETE /api/projects/{id}/document-types` - Types de documents
- `GET/POST/PUT/DELETE /api/projects/{id}/workflow-states` - États workflow
- `GET/POST/PUT/DELETE /api/projects/{id}/workflow-transitions` - Transitions
- `GET/POST /api/projects/{id}/id-rule` - Règles d'identification
- `GET/POST/PUT/DELETE /api/documents` - CRUD documents avec **pagination** (page + per_page)
- `POST /api/documents/{id}/upload` - Upload fichier
- `GET /api/documents/{id}/download` - Téléchargement
- `POST /api/documents/{id}/transition` - Transition workflow
- `GET /api/documents/{id}/history` - Historique
- `POST/GET /api/documents/{id}/signatures` - Signatures électroniques
- `GET/POST/PUT/DELETE /api/admin/users` - Gestion utilisateurs
- `GET/POST/PUT/DELETE /api/organizations/{id}/roles` - Gestion rôles
- `GET /api/kpi/overview` - KPIs globaux
- `GET /api/kpi/by-project` - KPIs par projet
- `GET/POST/PUT/DELETE /api/storage` - Config stockage

### Frontend Pages
- **LoginPage**: Email/password + Google OAuth, switch FR/EN
- **DashboardPage**: KPI cards + BarChart + PieChart + recent activity
- **ProjectsPage**: Liste projets par org, création/édition
- **ProjectDetailPage**: Onglets (Types docs, Workflow, Règle ID, Membres)
- **DocumentsPage**: Liste avec filtres, création, upload, transitions, historique
- **AdminPage**: Gestion utilisateurs et rôles (superadmin uniquement)
- **SettingsPage**: Config stockage local/distant, switch langue

### Database Models
- users, user_sessions, organizations, org_members, roles
- projects, project_members, document_types, doc_id_rules
- workflow_states, workflow_transitions
- documents, document_history, signatures
- storage_configs, kpi_configs

---

## Test Credentials
- **Admin**: admin@numdocman.com / Admin123! (superadmin)

---

## Prioritized Backlog

### P0 - Critical Next Steps
- [ ] PostgreSQL keepalive: configurer un service systemd pour garder PostgreSQL actif au redémarrage du container
- [ ] Tests d'intégration complets pour les transitions de workflow

### P1 - High Priority Features
- [x] **Visionneuse PDF intégrée** (react-pdf v10) - toolbar zoom/navigation, sidebar métadonnées+signatures
- [x] **Pagination des documents** - 20/page, navigation pages, compteur "X-Y sur Z résultats"
- [ ] Invitations utilisateurs par email (SendGrid)
- [ ] Export PDF des documents avec signatures
- [ ] Pagination des listes de documents
- [ ] Recherche full-text dans les documents
- [ ] Filtres avancés sur les documents

### P2 - Enhancement Features
- [ ] Commentaires sur les documents
- [ ] Notifications en temps réel (WebSocket)
- [ ] Dashboard KPIs personnalisables (drag-and-drop)
- [ ] Import/export CSV des documents
- [ ] Audit trail complet avec export
- [ ] Intégration stockage cloud (AWS S3, Azure Blob)
- [ ] Support NoSQL (MongoDB) pour certains types de données
- [ ] API REST publique avec documentation Swagger complète
- [ ] Mode sombre
- [ ] Application mobile (React Native)

---

## Next Action Items
1. Tester les fonctionnalités avec des données réelles
2. Configurer PostgreSQL pour la persistance (systemd service)
3. Ajouter la pagination sur les listes de documents
4. Implémenter la visionneuse PDF intégrée
5. Configurer le stockage cloud (S3)
