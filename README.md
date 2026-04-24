# LEV GROUP · מערכת הצעות מחיר

> השקט שלך, ההצלחה שלנו

מערכת דיגיטלית ליצירה, שליחה וחתימה על הצעות מחיר לשירותי ניהול ואחזקת בניינים.

## 🌐 הקישור החי

**https://hagayleven.github.io/LevGroup/**

## ✨ פיצ'רים

- 📝 **יצירת הצעות מחיר** - טופס חכם עם מחשבון אוטומטי
- 🏢 **חישוב דינמי** - מבוסס מיקומים, שעות עבודה, חברת מעליות, גינון ומערכות מורכבות
- 🔗 **שליחת קישור ייחודי** - לקוח מקבל קישור ווטסאפ ופותח מהנייד
- ✍️ **חתימה דיגיטלית** - חתימה באצבע ישירות מהנייד
- 📄 **הורדת PDF** - להעברה לדיירים
- 📁 **ארכיון** - כל ההצעות נשמרות ב-Firebase
- ⚙️ **מרכז בקרה** - עריכת תעריפים בלייב
- 🔔 **התראות** - ntfy + Email ברגע שלקוח חותם

## 🛠️ טכנולוגיות

- HTML / CSS / JavaScript (ללא מסגרת)
- Firebase Firestore (שמירת הצעות)
- SignaturePad (חתימה דיגיטלית)
- jsPDF + html2canvas (יצירת PDF)
- EmailJS + ntfy.sh (התראות)

## 🚀 התקנה (למפתחים)

המערכת היא קובץ HTML יחיד - אין צורך ב-build process.

### הגדרות נדרשות

ערוך את `index.html` בשורות הבאות:

```javascript
// Firebase config
const firebaseConfig = {
  apiKey: "...",
  authDomain: "...",
  projectId: "...",
  // ...
};

// EmailJS (אופציונלי)
emailjs: {
  serviceId: "...",
  templateId: "...",
  publicKey: "..."
}

// ntfy topic
ntfyTopic: "lev-group-signed"
```

## 📞 יצירת קשר

**LEV GROUP - קבוצת לב**
- 📞 052-5339443
- ✉️ levgroup4u@gmail.com
- 📍 שדרות ירושלים 18, אשדוד

---

Built with ❤️ by [Hagay Levenshtein](https://github.com/HagayLeven)
