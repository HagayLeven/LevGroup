# 📋 מסמך העברה - פרויקט LEV GROUP

> **הוראות לקלוד קוד:** קרא את המסמך הזה כדי להבין את הפרויקט לפני שאתה מתחיל לעבוד עליו. הכל מסוכם כאן - מהעסק, דרך הלוגיקה, ועד הקבצים שכבר נבנו.

---

## 👤 מי המשתמש

**חגי לונשטיין** (Hagay Levenshtein) - מפתח פרילנסר ישראלי.
תחת המותג **Levenshtein Studio**, מתמחה ב-React, Firebase, ו-RTL interfaces.

**העסק שהוא בונה עבורו את המערכת:**
- **שם:** LEV GROUP (קבוצת לב)
- **סלוגן:** "השקט שלך, ההצלחה שלנו"
- **התמחות:** ניהול ואחזקת בנייני מגורים באזור אשדוד
- **טלפון:** 052-5339443 / 054-5339443
- **מייל:** levgroup4u@gmail.com
- **כתובת:** שדרות ירושלים 18, אשדוד
- **ע.מ:** 208839340

---

## 🎯 מה המערכת עושה

מערכת **הצעות מחיר דיגיטליות** לשירותי ניהול בנייני מגורים, שמחליפה את התהליך הידני של אקסל + PDF + חתימה פיזית.

### התרחיש המרכזי:
1. **אורן (נציג שטח)** יושב מול ועד בית של בניין
2. במחשב/טאבלט/נייד, אורן פותח את המערכת וממלא פרטים (פרטי בניין, מיקומי ניקיון, מעליות, גינון, הטבות וכו')
3. המחשבון מחשב **בלייב** את המחיר החודשי הכולל ומחלק במספר הדירות → **₪ לדירה לחודש**
4. אורן לוחץ "יצירת הצעה" → נוצר **קישור ייחודי** (שמור ב-Firebase)
5. אורן שולח את הקישור לועד הבית בווטסאפ
6. ועד הבית פותח את הקישור מהנייד שלו → רואה הצעת מחיר מעוצבת
7. **חותם באצבע** (SignaturePad)
8. אורן מקבל **התראת push** שההצעה נחתמה (ntfy + אימייל)
9. ועד הבית לוחץ "הורד PDF" ומעביר לדיירים

**חשוב:** המערכת צריכה לעבוד גם במקומות עם **קליטה גרועה** - לכן היא קובץ HTML יחיד עם Firebase בלבד (ללא backend server).

---

## 💰 מודל התמחור

הלקוח (ועד הבית) רואה **מחיר סופי לדירה** (למשל ₪300/דירה/חודש). המחיר הזה מורכב מחישוב פנימי של:

### רכיבי המחיר:
1. **ניקיון** - חישוב: `(שעות שבועיות × 70₪ × 4.33 שבועות) × (1 + 35% צו הרחבה) × (1 + 10% רווח)`
   - כל מיקום מוגדר כ: שם + דקות + פעמים בשבוע
2. **מעליות** - `מחיר_לתחנה × מספר_תחנות` (מחיר משתנה לפי חברה)
   - חברות: שינדלר, אוטיס, תיסנקרופ, קון (עריכה במרכז בקרה)
3. **גינון** - `מחיר_לפעם (250₪) × פעמים בחודש`
4. **מערכות מורכבות** - תוספת קבועה ₪500/חודש (כיבוי אש, מאגרי מים, משאבות)
5. **רווח חברת הניהול** - קבוע ₪100/חודש
6. **תוספות מותאמות** - שורות חופשיות שאורן מוסיף
7. **הנחה** - סכום ₪ שמחוסר מהסה"כ
8. **מע"מ 18%** - מתווסף בסוף

**חישוב סופי:** `סה"כ חודשי / מספר דירות = ₪ לדירה`

---

## 🧱 מבנה המערכת

**קובץ יחיד:** `index.html` (כ-2540 שורות) - מכיל HTML + CSS + JS בקובץ אחד.

המערכת היא **SPA** (Single Page Application) עם 4 מצבים לפי URL parameters:

| URL | מסך | למי |
|-----|-----|-----|
| `/` או `?` | **יצירת הצעה חדשה** (Admin) | אורן / חגי |
| `?view=archive` | **ארכיון** כל ההצעות | אורן / חגי |
| `?view=settings` | **מרכז בקרה** - עריכת תעריפים | אורן / חגי |
| `?id=xxx` | **תצוגת הצעה + חתימה** | ועד הבית (לקוח) |

### הטכנולוגיות:
- **Vanilla JavaScript** (ללא framework)
- **Firebase Firestore** - שמירת הצעות ותעריפים
- **SignaturePad** (v4.1.7) - חתימה דיגיטלית באצבע
- **jsPDF + html2canvas** - יצירת PDF בצד-לקוח
- **EmailJS** - התראות מייל
- **ntfy.sh** - push notifications לטלפון
- **Google Fonts:** Heebo (עברית) + Instrument Serif (אנגלית דקורטיבית)

### Design System:
- **צבעים:** שחור (#1A1A1A) + זהב (#C5A572) + קרם (#FAF7F2) - מתאים לברנד LEV GROUP
- **RTL** (עברית) עם fallback לעיצוב LTR במקומות הנכונים
- **Responsive** מלא - עובד על נייד, טאבלט ודסקטופ

---

## 🗄️ מבנה Firestore

### Collection: `quotes`
כל מסמך מייצג הצעת מחיר אחת:

```javascript
{
  // Client info
  client: {
    name: "נציגות שדרות רוטשילד 30",
    representative: "דוד כהן",
    phone: "050-1234567",
    email: "..."
  },

  // Building info
  building: {
    address: "שדרות רוטשילד 30, אשדוד",
    apartments: 26,
    floors: 7,
    parkingType: "לא מקורה"  // לא מקורה | מקורה | אין | תת-קרקעי
  },

  // Cleaning calculator
  cleaningLocations: [
    { name: "לובי כניסה", minutes: 30, timesPerWeek: 5 },
    { name: "קומות", minutes: 60, timesPerWeek: 2 },
    { name: "חדר מדרגות", minutes: 60, timesPerWeek: 1 }
  ],

  // Elevators
  elevators: [
    { name: "מעלית 1", company: "שינדלר", stations: 7, _price: 315 }
  ],

  // Other services
  gardeningVisitsPerMonth: 2,
  hasComplexSystems: false,

  // Custom extras
  customExtras: [
    { name: "תיאור", amount: 100 }
  ],

  // Financial
  discount: 0,
  vatPct: 18,

  // Text content (editable per quote)
  servicesText: "...",  // טקסט חופשי של שירותים כלולים
  benefitsText: "...",  // טקסט חופשי של הטבות
  notesText: "...",     // הערות ותוקף

  // Calculated snapshot
  calc: {
    cleaningTotal: 2624,
    elevatorsTotal: 315,
    gardeningTotal: 500,
    complexSystemsTotal: 0,
    managementProfit: 100,
    customExtrasTotal: 0,
    subtotalBeforeDiscount: 3539,
    discountAmount: 0,
    subtotalAfterDiscount: 3539,
    vatAmount: 637,
    totalMonthly: 4176,
    perApartment: 161,
    apartments: 26
  },

  // Metadata
  quoteNumber: "LG-0001",
  shareId: "abc123xyz456",
  createdAt: Timestamp,

  // Signature (filled when signed)
  signature: null | {
    name: "דוד כהן",
    idOrRole: "יו\"ר ועד",
    image: "data:image/png;base64,..."
  },
  signedAt: null | Timestamp,
  status: "pending" | "signed",

  // Tariffs snapshot (for historical accuracy)
  tariffsSnapshot: { ... }  // העתק של התעריפים בזמן יצירת ההצעה
}
```

### Collection: `settings` → Document: `app_settings`
תעריפים עולמיים שמשפיעים על חישוב הצעות חדשות:

```javascript
{
  cleaning: {
    hourlyRate: 70,
    expansionPct: 35,
    profitPct: 10,
    weeksPerMonth: 4.33
  },
  management: { baseProfit: 100 },
  elevators: [
    { company: "שינדלר", pricePerStation: 45 },
    { company: "אוטיס", pricePerStation: 42 },
    // ...
  ],
  gardening: { pricePerVisit: 250 },
  complexSystems: { price: 500 },
  vatPct: 18
}
```

---

## ⚠️ מה **עדיין לא** הוגדר - חובה לסיים!

הקובץ `index.html` מכיל placeholders במקומות האלה (חיפוש: `YOUR_`):

### 1. Firebase Config (קריטי!)
```javascript
const firebaseConfig = {
  apiKey: "YOUR_API_KEY_HERE",
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT.appspot.com",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID"
};
```
**חגי - יש לך פרויקט Firebase קיים מהפרויקט של החולצות?** אפשר להשתמש בו שוב, או ליצור חדש ב-[console.firebase.google.com](https://console.firebase.google.com).

### 2. EmailJS (אופציונלי - להתראות מייל)
```javascript
emailjs: {
  serviceId: "YOUR_SERVICE_ID",
  templateId: "YOUR_TEMPLATE_ID",
  publicKey: "YOUR_PUBLIC_KEY"
}
```
**חגי - יש לך כבר חשבון EmailJS מהפרויקט של החולצות!** (מוזכר בזיכרון שלי). אפשר להשתמש באותו.

### 3. ntfy Topic
```javascript
ntfyTopic: "lev-group-signed"
```
שם ייחודי לטופיק. צריך להתקין את אפליקציית ntfy בטלפון של אורן ולהרשם לטופיק הזה.

### 4. Firestore Security Rules
בקונסול של Firebase → Firestore Database → Rules, נדרש להעתיק:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /quotes/{quoteId} {
      // כל אחד יכול לקרוא וליצור (הצעות חדשות)
      allow read, create: if true;
      // עדכון - רק להוספת חתימה (לא מחיקה או שינוי שדות אחרים)
      allow update: if true;  // לפרודקשן - להחמיר
      allow delete: if false;
    }
    match /settings/{docId} {
      allow read: if true;
      allow write: if true;  // לפרודקשן - להחמיר או להוסיף Auth
    }
  }
}
```

---

## 📁 קבצים שכבר נבנו

### בתיקייה `LevGroup/` שלי:
1. **`index.html`** - הקובץ הראשי (2540 שורות)
2. **`README.md`** - תיעוד הפרויקט
3. **`.gitignore`** - קבצים שלא יעלו לגיט
4. **`404.html`** - דף שגיאה מעוצב

### GitHub Repo:
- **URL:** [github.com/HagayLeven/LevGroup](https://github.com/HagayLeven/LevGroup)
- **סטטוס:** ריק כרגע (הקבצים עדיין לא הועלו)

---

## 🎯 משימות עכשיו - לפי סדר עדיפות:

### A. העלאה ל-GitHub + GitHub Pages (דחוף)
1. `git init` בתיקייה
2. `git add .` כל הקבצים
3. `git commit -m "Initial upload"`
4. `git remote add origin https://github.com/HagayLeven/LevGroup.git`
5. `git push -u origin main`
6. לפתוח GitHub Settings → Pages → Branch: main → Save
7. לוודא שהקישור `https://hagayleven.github.io/LevGroup/` חי

### B. הגדרת Firebase (דחוף)
1. להשתמש בפרויקט Firebase הקיים (מהחולצות) או ליצור חדש
2. להפעיל Firestore Database
3. להעתיק את הקונפיג לתוך `index.html`
4. להדביק את ה-Security Rules
5. לבדוק שיצירת הצעה והחתימה עובדות

### C. הגדרת EmailJS + ntfy (פחות דחוף)
1. לחבר את פרטי EmailJS הקיימים
2. לבחור ntfy topic (הצעה: `lev-group-signed` או משהו ייחודי)
3. להתקין ntfy באנדרואיד/iOS ולהרשם לטופיק

### D. בדיקות (חשוב!)
1. לפתוח `/` → למלא טופס → ליצור הצעה → לראות שהקישור נוצר
2. לפתוח את הקישור בדפדפן אחר (או אינקוגניטו) → לחתום
3. לוודא שההתראה מגיעה ל-ntfy
4. לבדוק שה-PDF מורד נכון

### E. שיפורים אפשריים בעתיד
- **Firebase Auth** - להגן על מסך האדמין מפני כניסה לא מורשית
- **Edit Quote** - אפשרות לערוך הצעה קיימת לפני חתימה
- **Duplicate Quote** - לשכפל הצעה קיימת כבסיס לחדשה
- **Email the PDF** - שליחת ה-PDF ישירות ללקוח במייל
- **Multiple signatories** - אם ועד הבית הוא מספר אנשים
- **Price history graph** - גרף של הצעות שנחתמו לאורך זמן
- **Custom themes per client** - אם LEV GROUP ירצה לפתח עוד מותגים

---

## 🔗 קישורים שימושיים

- **GitHub Repo:** https://github.com/HagayLeven/LevGroup
- **הקישור החי (אחרי העלאה):** https://hagayleven.github.io/LevGroup/
- **Firebase Console:** https://console.firebase.google.com
- **EmailJS:** https://www.emailjs.com/
- **ntfy:** https://ntfy.sh/
- **Hagai's GitHub:** https://github.com/HagayLeven

---

## 💡 הערות לקלוד קוד

1. **שפה:** חגי מעדיף עברית בשיחה. הקוד יכול להיות באנגלית.
2. **סגנון:** חגי אוהב **פרקטי > תיאורטי**. "Ship it fast" - לא בונים עקומות למידה.
3. **עיצוב:** חגי שם חזק על UX/UI. הבר גבוה. פרי-דטייל מטריאל.
4. **RTL:** זה פרויקט עברית - לוודא שכל שינוי לא שובר RTL.
5. **קובץ יחיד:** **לא לפצל** את `index.html` לקבצי JS/CSS נפרדים. זו החלטה מכוונת.
6. **מובייל:** אורן ישתמש מהנייד ברוב הזמן. **mobile-first** הוא לא סלוגן - זה קריטי.
7. **Git:** חגי לא בהכרח מנוסה עם git מעבר לבסיסי. להסביר כל פקודה.

---

**בהצלחה! 🚀 חגי סומך עליך.**
