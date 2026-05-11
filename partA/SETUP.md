# Part A — Setup

## Сонгосон API: DummyJSON

**URL:** https://dummyjson.com  
**Base URL (dev):** `https://dummyjson.com`

## Brief

DummyJSON нь frontend болон API testing-д зориулсан үнэгүй, нээлттэй REST API. Бодит
CRUD үйлдэл (create, update, delete), JWT authentication, pagination, search зэрэг
боломжуудыг дэмждэг. Response нь бодит апп-д хэрэглэдэгтэй ижил формат.

## Endpoints (ашигласан)

| Method | URL | Тайлбар |
|--------|-----|---------|
| POST | `/auth/login` | JWT token авах |
| GET | `/auth/me` | Token-оор өөрийн профайл |
| GET | `/products` | Бүтээгдэхүүн жагсаалт (pagination) |
| GET | `/products/:id` | Нэг бүтээгдэхүүн |
| POST | `/products/add` | Шинэ бүтээгдэхүүн үүсгэх |
| PUT | `/products/:id` | Бүтээгдэхүүн шинэчлэх |
| DELETE | `/products/:id` | Бүтээгдэхүүн устгах |
| GET | `/products/999999` | 404 error кейс |

## Auth

- **Арга:** JWT Bearer token
- **Login:** `POST /auth/login` → `{ username, password }` → `{ token, ... }`
- **Test хэрэглэгч:** `username: emilys`, `password: emilyspass`
- **Header:** `Authorization: Bearer {{token}}`
- **Token хугацаа:** 60 минут (expiresInMins: 60)

## Rate limit

- Тодорхой rate limit алба ёсоор зарлаагүй — тест хийхэд хэтрэхгүй
- Хуурамч CRUD: `POST/PUT/DELETE` нь DB-д бодитоор хадгалдаггүй — response зөв форматаар ирнэ

## Environment хувьсагчид

| Хувьсагч | dev утга | staging | prod |
|---------|----------|---------|------|
| `baseUrl` | `https://dummyjson.com` | `https://dummyjson.com` | `https://dummyjson.com` |
| `token` | *(login-аас авна)* | — | — |
| `userId` | *(login-аас авна)* | — | — |
| `productId` | `1` | `1` | `1` |
