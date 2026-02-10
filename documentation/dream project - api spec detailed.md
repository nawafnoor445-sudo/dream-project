Dream Project — API Spec Detailed (V1)
الإصدار: V1.0 (مبدئي)
التاريخ: 2026-02-08

ملاحظات عامة
- جميع المسارات تبدأ بـ /api/v1
- الاستجابات بصيغة JSON
- المصادقة عبر Bearer Token في Authorization
- المعرّفات UUID

شكل الخطأ القياسي
{
  "error": {
    "code": "STRING",
    "message": "STRING",
    "details": {}
  }
}

1) Auth & Users

POST /auth/register
Request:
{
  "name": "STRING",
  "email": "STRING",
  "password": "STRING"
}
Response 201:
{
  "user": {
    "id": "UUID",
    "name": "STRING",
    "email": "STRING",
    "role": "developer",
    "trust_level": "bronze",
    "created_at": "ISO"
  },
  "token": "JWT"
}
Errors: email_taken, weak_password

POST /auth/login
Request:
{
  "email": "STRING",
  "password": "STRING"
}
Response 200:
{
  "token": "JWT"
}
Errors: invalid_credentials

GET /users/me
Response 200:
{
  "user": {
    "id": "UUID",
    "name": "STRING",
    "email": "STRING",
    "avatar_url": "STRING?",
    "bio": "STRING?",
    "role": "developer",
    "trust_level": "bronze",
    "trust_score": 0,
    "projects_count": 0
  }
}

PATCH /users/me
Request:
{
  "avatar_url": "STRING?",
  "bio": "STRING?"
}
Response 200: updated user

GET /users/me/skills
Response 200:
{
  "skills": [
    { "id": "UUID", "name": "STRING", "rating": 0 }
  ]
}

POST /users/me/skills
Request:
{ "skill_id": "UUID" }
Response 201: created
Errors: skill_exists

DELETE /users/me/skills/:skillId
Response 204

2) Projects

POST /projects
Request:
{
  "name": "STRING",
  "description": "STRING",
  "required_members": 10,
  "icon_url": "STRING?"
}
Response 201:
{
  "project": {
    "id": "UUID",
    "name": "STRING",
    "status": "planned",
    "current_members": 0,
    "required_members": 10,
    "owner_id": "UUID"
  }
}

GET /projects
Query:
- status
- section (section name)
- team_size (min/max)
- latest (true/false)
- trending (true/false)
Response 200:
{
  "projects": [
    { "id": "UUID", "name": "STRING", "status": "team_building" }
  ]
}

GET /projects/:projectId
Response 200:
{
  "project": {
    "id": "UUID",
    "name": "STRING",
    "description": "STRING",
    "status": "in_progress",
    "current_members": 5,
    "required_members": 10,
    "owner_id": "UUID"
  }
}

PATCH /projects/:projectId
Request:
{
  "name": "STRING?",
  "description": "STRING?",
  "status": "STRING?"
}
Response 200: updated project
Errors: invalid_status_transition

POST /projects/:projectId/sections
Response 201:
{
  "sections": [
    { "id": "UUID", "name": "Backend", "permission_default": "view_and_work" }
  ]
}

POST /projects/:projectId/sections/custom
Request:
{ "name": "STRING" }
Response 201:
{ "section": { "id": "UUID", "name": "STRING", "is_custom": true } }

3) Membership & Permissions

POST /projects/:projectId/join-requests
Response 201: created
Errors: team_full, request_limit, already_requested

GET /projects/:projectId/join-requests
Response 200:
{ "requests": [ { "id": "UUID", "user_id": "UUID", "status": "pending" } ] }

POST /join-requests/:requestId/accept
Response 200:
{ "member": { "user_id": "UUID", "project_id": "UUID" } }

POST /join-requests/:requestId/reject
Response 200

POST /join-requests/:requestId/withdraw
Response 200

DELETE /projects/:projectId/members/:userId
Request:
{ "reason": "STRING" }
Response 200
Errors: reason_required

GET /projects/:projectId/permissions
Response 200:
{
  "permissions": [
    { "user_id": "UUID", "section_id": "UUID", "permission": "view_only" }
  ]
}

PATCH /projects/:projectId/permissions
Request:
{
  "user_id": "UUID",
  "section_id": "UUID",
  "permission": "view_only"
}
Response 200

4) Tasks

POST /projects/:projectId/tasks
Request:
{
  "section_id": "UUID",
  "title": "STRING",
  "description": "STRING?",
  "due_date": "ISO?",
  "assigned_user_id": "UUID?"
}
Response 201:
{ "task": { "id": "UUID", "status": "not_started" } }

PATCH /tasks/:taskId
Request:
{ "title": "STRING?", "description": "STRING?", "due_date": "ISO?" }
Response 200

POST /tasks/:taskId/start
Response 200

POST /tasks/:taskId/review
Response 200

POST /tasks/:taskId/complete
Response 200

POST /tasks/:taskId/extend
Request:
{ "due_date": "ISO" }
Response 200

5) Project Lifecycle

POST /projects/:projectId/status/team-building
POST /projects/:projectId/status/in-progress
POST /projects/:projectId/status/ready-for-testing
POST /projects/:projectId/status/completed
Response 200
Errors: invalid_status_transition

6) Trust & Ratings

GET /users/:userId/trust
Response 200:
{
  "trust_level": "bronze",
  "trust_score": 0,
  "events": [
    { "type": "task_completed", "delta": 5, "created_at": "ISO" }
  ]
}

POST /trust/events
Request:
{
  "user_id": "UUID",
  "project_id": "UUID?",
  "event_type": "STRING",
  "delta": 5,
  "reason": "STRING?"
}
Response 201

7) Notifications

GET /notifications
Response 200:
{
  "notifications": [
    { "id": "UUID", "type": "join_request", "is_read": false }
  ]
}

POST /notifications/:id/read
Response 200

8) Activity Feed

GET /projects/:projectId/activity
Response 200:
{ "events": [ { "summary": "STRING", "created_at": "ISO" } ] }

GET /activity
Response 200:
{ "events": [ { "summary": "STRING", "created_at": "ISO" } ] }

9) Admin

GET /admin/violations
Response 200

POST /admin/violations/:id/warn
Response 200

POST /admin/violations/:id/resolve
Response 200

POST /admin/projects/:projectId/remove
Response 200

POST /admin/users/:userId/ban
Response 200

GET /admin/news
Response 200

POST /admin/news
Request:
{ "title": "STRING", "body": "STRING" }
Response 201

PATCH /admin/news/:id
Request:
{ "title": "STRING?", "body": "STRING?" }
Response 200
