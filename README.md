# Бие даалт 14 — API Testing: Postman + Newman

**F.CSM311 | DummyJSON API**

---

## Хурдан эхлэх

### 1. Newman суулгах
```bash
npm install -g newman newman-reporter-htmlextra
```

### 2. Collection ажиллуулах
```bash
newman run postman/collection.json \
  -e postman/env.dev.json \
  --reporters cli,htmlextra \
  --reporter-htmlextra-export reports/api.html
```

### 3. Report харах
```bash
open reports/api.html   # macOS
xdg-open reports/api.html  # Linux
```

---

## Token тохиргоо

`postman/env.dev.json`-д token автоматаар Login request-аас chain хийгдэнэ:
1. "Login — Token авах" request ажиллана
2. Token environment-д хадгалагдана
3. Дараагийн authenticated request-үүд автоматаар ашиглана

**Secret хадгалах:** Бодит token-г хэзээ ч commit хийхгүй. `REPLACE_WITH_REAL_TOKEN` placeholder орхих.

---

## Бүтэц

```
bie-daalt-14/
├── README.md
├── REFLECTION.md
├── partA/
│   ├── SETUP.md
│   └── screenshot.png        ← Postman-аас screenshot хий
├── postman/
│   ├── collection.json       ← 8+ request, 3 folder, 20+ тест
│   ├── env.dev.json          ← dev (placeholder)
│   └── env.ci.json           ← CI (placeholder)
├── .github/workflows/
│   └── api-tests.yml
└── reports/
    └── api.html              ← Newman run-ы report
```

---

## Tests хураангуй

| Folder | Requests | Assertions |
|--------|----------|------------|
| Auth | 2 | 9 |
| Products — Happy Path | 5 | 18 |
| Error Cases | 3 | 7 |
| **Нийт** | **10** | **34** |

---

*Багш: Агвааны ОТГОНБАЯР | ШУТИС МХТ Сургууль*