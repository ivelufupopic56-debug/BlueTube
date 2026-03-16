# 📱 BlueTube – ניגון יוטיוב ברקע

אפליקציית אנדרואיד אישית שמדמה יוטיוב פרימיום:
- ✅ ניגון ברקע (המסך כבוי / מעבר אפליקציה)
- ✅ מצב תמונה-בתמונה (PiP)
- ✅ כפתור ניווט, רענון, בית
- ✅ ממשק כחול חם / dark mode
- ✅ אין פרסומות דרך AdBlock מובנה ב-WebView

---

## 🛠️ בנייה עם Android Studio (קל ביותר)

### דרישות
- [Android Studio](https://developer.android.com/studio) (גרסת Hedgehog 2023.1 ומעלה)
- JDK 17 (מגיע עם Studio)

### שלבים
1. פתח Android Studio → **Open** → בחר את תיקיית `BlueTube`
2. המתן ל-Gradle Sync לסיום (דקה-שתיים)
3. **Build → Build Bundle(s) / APK(s) → Build APK(s)**
4. הAPK יופיע ב:
   ```
   app/build/outputs/apk/debug/app-debug.apk
   ```
5. העבר לטלפון וסמן **"מקורות לא ידועים"** בהגדרות

---

## 🖥️ בנייה מהמסוף (ללא Android Studio)

```bash
# ודא ש-ANDROID_HOME מוגדר (לדוגמה: ~/Android/Sdk)
export ANDROID_HOME=~/Android/Sdk
export PATH=$PATH:$ANDROID_HOME/platform-tools

cd BlueTube

# הרשאות לגרידל
chmod +x gradlew

# בנייה
./gradlew assembleDebug

# ה-APK:
# app/build/outputs/apk/debug/app-debug.apk

# התקנה ישירה לטלפון מחובר (ADB)
adb install -r app/build/outputs/apk/debug/app-debug.apk
```

---

## 📲 התקנה ידנית

1. העבר `app-debug.apk` לטלפון (WhatsApp / כבל / Drive)
2. פתח את הקובץ בטלפון
3. אם יופיע חסימה → **הגדרות → אפליקציות → BlueTube → "אפשר התקנה"**
4. התקן ✅

---

## ⚙️ איך עובד ניגון ברקע

| מנגנון | תיאור |
|--------|--------|
| `BackgroundPlayService` | שירות Foreground שמונע מהמערכת לסגור את התהליך |
| JS Patch | מבטל את `visibilitychange` / `blur` שיוטיוב משתמש בהם לעצירת ניגון |
| `onPause()` לא קורא ל-`webView.onPause()` | מונע עצירת הצלילים בזמן מעבר אפליקציה |
| `START_STICKY` | גורם ל-Android להפעיל מחדש את השירות אם נסגר |

---

## 🔔 הרשאות שהאפליקציה מבקשת

| הרשאה | סיבה |
|-------|------|
| `INTERNET` | גלישה ביוטיוב |
| `FOREGROUND_SERVICE` | ניגון ברקע |
| `WAKE_LOCK` | מונע כיבוי CPU בזמן שמיעה |
| `POST_NOTIFICATIONS` | הודעת "מנגן ברקע" בסרגל |

---

## 🐛ידוע / מגבלות

- **לוגין גוגל**: עלול להיתקל בבעיית WebView login. אם יש בעיה, נסה לנקות cookies ולהיכנס שוב.
- **PiP**: דורש Android 8.0+
- **פרסומות**: חלק מהפרסומות עלולות עדיין להופיע (WebView ללא uBlock)
- **רזולוציה**: יוטיוב Mobile UI (לא Desktop)

---

_BlueTube – לשימוש אישי בלבד_
