Dream Project — API Spec (V1)
الإصدار: V1.0 (مبدئي)
التاريخ: 2026-02-08

ملاحظات عامة
- جميع المسارات تبدأ بـ /api/v1
- جميع الطلبات تتطلب Authorization باستثناء التسجيل والدخول والصفحات العامة
- الاستجابات بصيغة JSON

1) Auth & Users
POST /auth/register
- إنشاء حساب جديد (email, password, name)

POST /auth/login
- تسجيل الدخول

GET /users/me
- جلب بيانات المستخدم الحالي

PATCH /users/me
- تحديث الملف الشخصي (avatar_url, bio)

GET /users/me/skills
POST /users/me/skills
DELETE /users/me/skills/:skillId
- إدارة مهارات المطور

2) Projects
POST /projects
- إنشاء مشروع جديد

GET /projects
- قائمة المشاريع (فلاتر: status, section, team_size, latest, trending)

GET /projects/:projectId
- تفاصيل مشروع

PATCH /projects/:projectId
- تحديث بيانات المشروع (name, description, status)

POST /projects/:projectId/sections
- إنشاء الأقسام الافتراضية + الأقسام المخصصة

POST /projects/:projectId/sections/custom
- إضافة قسم مخصص (name)

3) Membership & Permissions
POST /projects/:projectId/join-requests
- إرسال طلب انضمام

GET /projects/:projectId/join-requests
- قائمة طلبات الانضمام (لصاحب المشروع)

POST /join-requests/:requestId/accept
- قبول طلب الانضمام

POST /join-requests/:requestId/reject
- رفض طلب الانضمام

POST /join-requests/:requestId/withdraw
- سحب الطلب من المطور

DELETE /projects/:projectId/members/:userId
- إخراج عضو (مع سبب)

GET /projects/:projectId/permissions
- جلب صلاحيات الأقسام لأعضاء المشروع

PATCH /projects/:projectId/permissions
- تعديل صلاحيات الأقسام لعضو

4) Tasks
POST /projects/:projectId/tasks
- إنشاء مهمة (section_id, title, description, due_date, assigned_user_id)

PATCH /tasks/:taskId
- تعديل مهمة (title, description, due_date, assigned_user_id)

POST /tasks/:taskId/start
- نقل الحالة إلى In Progress

POST /tasks/:taskId/review
- نقل الحالة إلى In Review (من المطور)

POST /tasks/:taskId/complete
- نقل الحالة إلى Completed (من صاحب المشروع)

POST /tasks/:taskId/extend
- تمديد وقت المهمة (من صاحب المشروع)

5) Project Lifecycle
POST /projects/:projectId/status/team-building
POST /projects/:projectId/status/in-progress
POST /projects/:projectId/status/ready-for-testing
POST /projects/:projectId/status/completed
- تغيير حالة المشروع حسب القواعد

6) Trust & Ratings
GET /users/:userId/trust
- عرض مستوى الثقة وسجل الأحداث

POST /trust/events
- إنشاء حدث ثقة (داخليًا فقط)

7) Notifications
GET /notifications
- قائمة الإشعارات

POST /notifications/:id/read
- تعليم إشعار كمقروء

8) Activity Feed
GET /projects/:projectId/activity
- سجل الأحداث داخل المشروع

GET /activity
- سجل أحداث المنصة (للصفحة العامة أو المدير)

9) Admin
GET /admin/violations
POST /admin/violations/:id/warn
POST /admin/violations/:id/resolve
POST /admin/projects/:projectId/remove
POST /admin/users/:userId/ban
- إدارة المخالفات والعقوبات

GET /admin/news
POST /admin/news
PATCH /admin/news/:id
- إدارة أخبار المنصة
