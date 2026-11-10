# המשך עבודה כשהמכסה של Claude נגמרת

המטרה: הפרויקט לא נעצר עד האיפוס השבועי. כל הכלים קוראים את אותו הקשר
(`AGENTS.md`) ואת אותו מצב עבודה (`TASKS.md`), כך שכל כלי ממשיך מאיפה שהקודם עצר.

> שמות מודלים, מכסות חינמיות ומחירים משתנים לעיתים קרובות — בדוק את המצב העדכני
> אצל כל ספק לפני שמסתמכים עליו.

## סדר עדיפויות (fallback ladder)
1. **Claude Code** — עבודה רגילה. כשהמכסה מתקרבת לסוף: בקש ממנו לעדכן `TASKS.md`.
2. **Claude Code + claude-code-router** — אותו כלי, מודל אחר (DeepSeek / Qwen / Gemini).
3. **Gemini CLI / Qwen Code** — סוכנים עם מכסה חינמית יומית.
4. **Ollama מקומי** — תמיד זמין, ללא מגבלה (דורש GPU סביר).
5. **בתשלום לפי שימוש** — Claude API / extra usage, רק כשיש deadline.

## לפני שהמכסה נגמרת — הפקודה לתת ל-Claude
```
עדכן את TASKS.md: מה עשית, מה חצי גמור, הצעד המדויק הבא. ואז commit.
```

## אפשרות 2: claude-code-router (מומלץ להתחלה)
מנתב את Claude Code לספקים אחרים — אותן פקודות, אותו ממשק.

התקנה (דורש Node.js):
```bash
npm install -g @anthropic-ai/claude-code @musistudio/claude-code-router
```

הגדרה: העתק את `claude-code-router.config.example.json` אל
`%USERPROFILE%\.claude-code-router\config.json` (ב-Windows) או
`~/.claude-code-router/config.json`, ומחק ספקים שאין לך להם מפתח.

מפתחות API — כמשתני סביבה, **אף פעם לא ב-repo** (PowerShell):
```powershell
setx DEEPSEEK_API_KEY "..."
setx OPENROUTER_API_KEY "..."
setx GEMINI_API_KEY "..."
```

הפעלה מתוך תיקיית הפרויקט:
```bash
ccr code
```
החלפת מודל תוך כדי עבודה: `/model deepseek,deepseek-reasoner`.

ניתוב בקובץ הדוגמה:
- `default` — DeepSeek (זול מאוד, טוב בקוד)
- `think` — DeepSeek Reasoner, למשימות שדורשות חשיבה
- `longContext` — Gemini Pro, כשההקשר גדול
- `background` — מודל מקומי ב-Ollama, למשימות רקע

## אפשרות 3: סוכנים חינמיים
**Gemini CLI** — מכסה חינמית עם חשבון Google:
```bash
npm install -g @google/gemini-cli
```
הוא קורא `GEMINI.md`; כדי שיקרא את `AGENTS.md`, צור בתיקיית הפרויקט
`.gemini/settings.json` עם `{ "contextFileName": "AGENTS.md" }`.

**Qwen Code** — מכסה חינמית עם חשבון Qwen:
```bash
npm install -g @qwen-code/qwen-code
```

**OpenCode / Aider / Cline (VS Code)** — עובדים עם כל מודל, כולל OpenRouter `:free` ו-Ollama.
OpenCode ו-Cline קוראים `AGENTS.md`; ב-Aider: `aider --read AGENTS.md --read TASKS.md`.

## אפשרות 4: מודל מקומי (Ollama)
```bash
ollama pull qwen2.5-coder:14b
```
- 8–12GB VRAM: מודל 7B. ‏16–24GB: ‏14B. יותר: מודלים גדולים יותר (למשל Devstral / Qwen3-Coder).
- מתחבר ל-claude-code-router (מופיע בקובץ הדוגמה), Aider, Cline ו-OpenCode.

## חלוקת עבודה
| משימה | מודל |
|---|---|
| ארכיטקטורה, באגים קשים ב-Revit API, לוגיקת RTL/קידוד | Claude |
| בדיקות, תיעוד, boilerplate, refactor פשוט, release notes | חינמי / זול |
| שינוי בלוגיקת הליבה שנעשה ע"י מודל חלש | לרשום ב-TASKS.md לבדיקה ע"י Claude |

## חיסכון במכסה של Claude
- `/model` — מודל קל יותר למשימות פשוטות.
- `/compact` כשהשיחה מתארכת; `/clear` בין משימות לא קשורות.
- סשנים קצרים וממוקדים, עם הפניה לקבצים ספציפיים במקום "תסתכל על כל הפרויקט".
