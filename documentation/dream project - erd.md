Dream Project — ERD (V1) نصي مختصر
الإصدار: V1.0 (مبدئي)
التاريخ: 2026-02-08

1) Users
Users (id) 1 ────< Projects (owner_id)
Users (id) 1 ────< MembershipRequests (user_id)
Users (id) 1 ────< ProjectMembers (user_id)
Users (id) 1 ────< SectionPermissions (user_id)
Users (id) 1 ────< Tasks (assigned_user_id)
Users (id) 1 ────< TrustEvents (user_id)
Users (id) 1 ────< Notifications (user_id)
Users (id) 1 ────< DisputeLog (user_id)
Users (id) 1 ────< ActivityFeed (actor_user_id)

Users (id) >────< Skills (id) عبر UserSkills

2) Projects
Projects (id) 1 ────< Sections (project_id)
Projects (id) 1 ────< Tasks (project_id)
Projects (id) 1 ────< MembershipRequests (project_id)
Projects (id) 1 ────< ProjectMembers (project_id)
Projects (id) 1 ────< SectionPermissions (project_id)
Projects (id) 1 ────< TrustEvents (project_id)
Projects (id) 1 ────< DisputeLog (project_id)
Projects (id) 1 ────< ActivityFeed (project_id)

3) Sections
Sections (id) 1 ────< SectionLinks (section_id)
Sections (id) 1 ────< Tasks (section_id)
Sections (id) 1 ────< SectionPermissions (section_id)

4) Skills
Skills (id) 1 ────< UserSkills (skill_id)

5) Memberships
MembershipRequests (id) [user_id + project_id + status]
ProjectMembers (id) [user_id + project_id]

6) Tasks
Tasks (id) [project_id + section_id + assigned_user_id]

7) Activity & Audit
TrustEvents (id) [user_id + project_id]
Notifications (id) [user_id]
DisputeLog (id) [user_id + project_id]
ActivityFeed (id) [actor_user_id + project_id?]

ملاحظات
- UserSkills هو جدول ربط (Many-to-Many) بين Users و Skills.
- ProjectMembers يمثل العضوية المقبولة.
- SectionPermissions يربط مطورًا بقسم داخل مشروع بصلاحية محددة.
