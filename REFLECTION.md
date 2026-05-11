# REFLECTION.md — Бие даалт 14

**F.CSM311 | API Testing — Postman + Newman**

---

## 1. Аль assertion хамгийн их үнэ цэнэтэй санагдсан вэ? Яагаад?

**Schema + property шалгалт** хамгийн их үнэ цэнэтэй байлаа.

`pm.expect(d).to.have.property('id')` гэх мэт шалгалтууд нь API-ийн "contract"-г тодорхойлдог. Status 200 ирсэн ч response body-д шаардлагатай талбар байхгүй байж болно — жишээлбэл, `price` талбар алга болсон бол frontend тооцоо хийж чадахгүй болно. Энэ нь API хувилбар өөрчлөгдөхөд breaking change-ийг тэр даруй илрүүлдэг.

Ялангуяа `pm.expect(d.email).to.match(/@/)` гэх мэт бизнес дүрмийн шалгалт — зөвхөн талбар байгааг биш, утга нь утга учиртай эсэхийг шалгадаг учир илүү хүчтэй.

---

## 2. Negative test жишээгээ дэлгэрэнгүй тайлбарла

**"POST Login — Буруу нууц үг"** тест:

`username: "wronguser"`, `password: "wrongpassword"` явуулахад:
- **Шалгадаг зүйл:** Server 400/401 буцаадаг уу, `accessToken` буцаагаагүй уу
- **Илрүүлдэг алдаа:** Хэрэв server алдаатай нэвтрэх мэдээлэлд 200 + token буцаавал энэ нь аюулгүй байдлын ноцтой алдаа. Эсвэл error response-д `message` байхгүй бол client-д ямар алдаа гарснаа мэдэхгүй болно.

`pm.expect(d).to.not.have.property('accessToken')` — энэ assertion нь "алдааны үед token буцаагаагүй" гэдгийг баталгаажуулдаг. "Хэвийн ажиллах"-ыг шалгахаас ч чухал — алдааны зам ч бас гэрээ (contract).

---

## 3. Postman-д амжилттай ажиллаж байсан тест Newman-д fail болсон уу? Яагаад?

Тийм. `GET Auth/Me — Token байхгүй` тест Postman-д pass болсон боловч Newman-д анх fail болсон.

**Шалтгаан:** Postman-д collection-level auth `{{token}}` environment variable автоматаар ашигладаг байсан ч "No Auth" тохируулсан request-д ч бас хуучин environment-ийн token-г `Authorization` header-д автоматаар нэмж байсан. Newman-д environment-ийн token хоосон (placeholder) байсан тул header огт ирсэнгүй — тэгэхэд 401 зөв ирсэн. Постман локал орчинд хуучин token-тэй тул 200 ирж байсан.

**Шийдвэр:** `auth: { type: "noauth" }` collection level override зөв хийгдсэн эсэхийг шалгасан.

---

## 4. Token болон secret-ыг хэрхэн зохицуулсан вэ?

- `env.dev.json`-д `"value": "REPLACE_WITH_REAL_TOKEN"` placeholder ашигласан — бодит token хэзээ ч commit хийгдэхгүй
- `.gitignore`-д `postman/env.dev.local.json` нэмсэн — локал бодит token энд л хадгалагдана
- `env.ci.json`-д `"REPLACE_THIS_IN_CI"` placeholder — DummyJSON-д CI тест хийхэд login chain-аар шинэ token авдаг тул static token хэрэггүй
- **Дүрэм:** Token-г README-д `REPLACE_THIS` гэж тайлбарласан, bagшд ч мэдэгдэлгүйгээр шалгаж болно

---

## 5. API өөрчлөгдвөл collection-ийн аль хэсэг хамгийн их эвдрэх вэ?

**Schema assertion-ууд** хамгийн эмзэг — `products` response-д `price` → `cost` болбол 10+ тест нэг дор fail болно.

**Login chain** хоёр дахь эмзэг газар — `accessToken` → `token` болвол бүх authenticated request fail болно, учир нь `pm.environment.set('token', json.accessToken)` `undefined` хадгалах болно.

**Бууруулах арга:**
- Schema-г нэг `pm.test('Schema', ...)` блокт цуглуулж, `required fields` const болгон тодорхойлох
- Login chain-ийн token field нэрийг `AUTH_TOKEN_FIELD = 'accessToken'` гэж нэг газарт хадгалах
- Collection-д `contract test` folder тусад нь болгон, "breaking change detector" болгон ашиглах
