# PRD — CursiFy Mobile

## 1) Problem Statement
Criar o app mobile educacional **CursiFy** com autenticação, catálogo de cursos, inscrição e experiência por perfil (Aluno, Professor e Admin), mantendo consistência funcional com a proposta do sistema web e UX mobile moderna.

## 2) Arquitetura Definida
- **Frontend:** Expo + React Native + TypeScript (mobile-first)
- **Backend:** FastAPI + JWT + WebSocket base + MongoDB
- **Banco:** MongoDB (coleções: `users`, `courses`, `enrollments`)
- **Integração:** Frontend consome `${EXPO_PUBLIC_BACKEND_URL}/api`

> Nota de contexto: o pedido original cita Spring Boot, porém neste ambiente a stack operacional é FastAPI/MongoDB.

## 3) Personas
- **Aluno:** busca cursos, vê detalhes, faz inscrição e acompanha seus cursos.
- **Professor:** publica e gerencia cursos.
- **Admin:** monitora métricas gerais de usuários/cursos/inscrições.

## 4) Core Requirements (estáticos)
1. Cadastro/login com JWT.
2. Controle por perfis (student/teacher/admin).
3. Catálogo público de cursos.
4. Tela de detalhes de curso.
5. Inscrição em curso (student/admin).
6. Área professor para criação de cursos.
7. Área admin com overview.
8. Base inicial de WebSocket para chat em tempo real.

## 5) Implementado (com data)

### 2026-03-18
#### Backend
- Estrutura completa de autenticação JWT:
  - `POST /api/auth/register`
  - `POST /api/auth/login`
  - `GET /api/auth/me`
- Endpoints de cursos e inscrições:
  - `GET /api/courses`
  - `GET /api/courses/{course_id}`
  - `POST /api/courses` (teacher/admin)
  - `POST /api/courses/{course_id}/enroll` (student/admin)
  - `GET /api/enrollments/me`
- Endpoint administrativo:
  - `GET /api/admin/overview` (admin)
- Endpoint professor:
  - `GET /api/professor/courses`
- Base WebSocket criada:
  - `ws /api/ws/chat/{room_id}`
- Correção crítica de serialização MongoDB (`_id`) em respostas.
- Índices de banco inicializados no startup.

#### Frontend
- Novo app mobile em tema claro/minimalista com componentes reutilizáveis.
- Fluxo de autenticação completo:
  - alternância login/cadastro
  - escolha de perfil no cadastro
- Navegação por abas por perfil:
  - Catálogo, Meus Cursos, Professor, Admin, Perfil (dinâmico por role)
- Catálogo de cursos + card + detalhes.
- Inscrição em curso pelo frontend.
- Área professor para criação de curso.
- Área admin com métricas resumidas.
- Perfil com logout.
- Imagens de curso/perfil em base64 (compatível com preview).

#### Qualidade e Testes
- Testes automáticos do agente de QA: **100% backend / 100% frontend**.
- Relatório: `/app/test_reports/iteration_1.json`.
- Suite de regressão adicionada em `/app/backend/tests/test_cursify_api.py`.

## 6) Backlog Priorizado

### P0 (próximo ciclo)
1. Persistência de sessão no app (AsyncStorage + refresh de estado).
2. Mensageria em tempo real real (WebSocket com histórico por sala).
3. Ajustes de segurança JWT (secret obrigatório via env, sem fallback inseguro).

### P1
1. Upload real de mídia (vídeo/áudio) com armazenamento.
2. Download offline de aulas.
3. Notificações push em eventos de curso/chat.
4. Avaliação de cursos e ranking inicial.

### P2
1. Gamificação completa (desafios diários, leaderboard avançado).
2. Canal de suporte e denúncias com moderação detalhada.
3. Acessibilidade avançada (leitor, alto contraste, ajustes de fonte).

## 7) Next Tasks List
1. Implementar persistência de login no frontend.
2. Entregar chat real-time (texto) ponta a ponta com histórico.
3. Adicionar upload de áudio para professor/aluno no chat.
4. Evoluir módulo admin para moderação (denúncias/ações).
