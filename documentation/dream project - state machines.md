Dream Project — State Machines (V1)
الإصدار: V1.0 (مبدئي)
التاريخ: 2026-02-08

1) Project State Machine

الحالات:
- Planned
- Team Building
- In Progress
- Ready for Testing
- Completed

الانتقالات المسموحة (من → إلى) + الشروط:

Planned → Team Building
- المسموح: Project Owner فقط
- الشروط: لا يوجد

Team Building → In Progress
- المسموح: Project Owner فقط
- الشروط: اكتمال العدد المطلوب أو قبول عدد كاف من المطورين

In Progress → Ready for Testing
- المسموح: Project Owner فقط
- الشروط: اكتمال جميع المهام الأساسية

Ready for Testing → Completed
- المسموح: Project Owner فقط
- الشروط: انتهاء مرحلة الاختبار

ملاحظات:
- أي محاولة انتقال خارج القائمة تعتبر Invalid Transition.
- تغيير الحالة يسجل في Activity Feed ويطلق إشعاراً للأعضاء.

2) Task State Machine

الحالات:
- Not Started
- In Progress
- In Review
- Completed

الانتقالات المسموحة (من → إلى) + الشروط:

Not Started → In Progress
- المسموح: المطور المعين أو Project Owner
- الشروط: لا يوجد

In Progress → In Review
- المسموح: المطور المعين فقط
- الشروط: انتهى من العمل

In Review → Completed
- المسموح: Project Owner فقط
- الشروط: قبول المراجعة

ملاحظات:
- الانتقال إلى Completed هو الحدث الذي يُسجّل للتقييم.
- أي محاولة انتقال خارج القائمة تعتبر Invalid Transition.
- يمكن لصاحب المشروع تمديد Due Date في أي حالة قبل Completed.
