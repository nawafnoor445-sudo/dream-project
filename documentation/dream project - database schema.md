Dream Project — قاعدة البيانات (V1)
الإصدار: V1.0 (مبدئي)
التاريخ: 2026-02-08

ملاحظات عامة
- جميع المفاتيح الأساسية UUID.
- التواريخ بصيغة UTC (created_at, updated_at).
- الحقول النصية القصيرة: varchar(255)، والوصف: text.

1) Enums (قيم ثابتة)
- user_role: guest, developer, project_owner, admin
- project_status: planned, team_building, in_progress, ready_for_testing, completed
- task_status: not_started, in_progress, in_review, completed
- membership_status: pending, accepted, rejected, withdrawn
- notification_type: join_request, join_accepted, join_rejected, member_withdrew,
  project_status_changed, task_completed, violation_warning
- trust_level: bronze, silver, gold
- dispute_status: open, closed
- section_permission: view_only, view_and_work

2) Users
- id (PK)
- name
- email (unique)
- password_hash
- avatar_url (nullable)
- bio (nullable)
- role (user_role)
- trust_level (trust_level) default bronze
- trust_score (int) default 0
- projects_count (int) default 0
- created_at
- updated_at

3) Skills (اختياري كقاموس مهارات)
- id (PK)
- name (unique)

4) UserSkills
- id (PK)
- user_id (FK → Users.id)
- skill_id (FK → Skills.id)
- rating (int) default 0
- created_at

5) Projects
- id (PK)
- owner_id (FK → Users.id)
- name
- description
- icon_url (nullable)
- current_members (int) default 0
- required_members (int)
- status (project_status) default planned
- idea_version (varchar(50)) nullable
- created_at
- updated_at

6) Sections
- id (PK)
- project_id (FK → Projects.id)
- name (varchar(100))
- permission_default (section_permission) default view_and_work
- is_custom (bool) default false
- created_at

7) SectionLinks
- id (PK)
- section_id (FK → Sections.id)
- title (varchar(255))
- url (varchar(500))
- link_type (varchar(50)) nullable (GitHub, Figma, Discord...)
- created_at

8) MembershipRequests
- id (PK)
- project_id (FK → Projects.id)
- user_id (FK → Users.id)
- status (membership_status) default pending
- created_at
- updated_at

قيود مقترحة:
- يمنع وجود طلبين نشطين لنفس المستخدم في نفس المشروع.
- الحد الأقصى 10 طلبات نشطة لكل مطور (منطق تطبيق).

9) ProjectMembers
- id (PK)
- project_id (FK → Projects.id)
- user_id (FK → Users.id)
- joined_at

10) SectionPermissions
- id (PK)
- project_id (FK → Projects.id)
- user_id (FK → Users.id)
- section_id (FK → Sections.id)
- permission (section_permission) default view_and_work
- created_at

11) Tasks
- id (PK)
- project_id (FK → Projects.id)
- section_id (FK → Sections.id)
- title
- description (nullable)
- status (task_status) default not_started
- assigned_user_id (FK → Users.id) nullable
- due_date (nullable)
- created_by (FK → Users.id)  -- صاحب المشروع
- created_at
- updated_at

12) TrustEvents
- id (PK)
- user_id (FK → Users.id)
- project_id (FK → Projects.id) nullable
- event_type (varchar(50))  -- task_completed, task_late, withdrawal, removed_member
- delta (int)
- reason (text) nullable
- created_at

13) Notifications
- id (PK)
- user_id (FK → Users.id)
- type (notification_type)
- message (text)
- is_read (bool) default false
- created_at

14) DisputeLog
- id (PK)
- project_id (FK → Projects.id)
- user_id (FK → Users.id)
- violation_type (varchar(50))
- description (text)
- status (dispute_status) default open
- created_at

15) ActivityFeed
- id (PK)
- scope (varchar(20))  -- project أو platform
- project_id (FK → Projects.id) nullable
- actor_user_id (FK → Users.id) nullable
- event_type (varchar(50))
- summary (text)
- created_at

العلاقات الأساسية
- Users 1 → Many Projects (owner_id)
- Projects 1 → Many Sections
- Projects 1 → Many Tasks
- Projects ↔ Developers عبر MembershipRequests + ProjectMembers
- Sections ↔ Developers عبر SectionPermissions
- Users ↔ Skills عبر UserSkills
- Projects/Users → TrustEvents, Notifications, DisputeLog, ActivityFeed

فهرسة مقترحة
- Users: email (unique)
- Projects: owner_id, status, created_at
- MembershipRequests: project_id, user_id, status
- ProjectMembers: project_id, user_id
- Tasks: project_id, section_id, status, assigned_user_id
- ActivityFeed: project_id, created_at
