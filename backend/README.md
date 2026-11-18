# Sprint 8 Backend — MongoDB ve Mongoose

Öğrenci No: **2416501039**

Sprint 8'de Sprint 7 REST adresleri korunmuş, etkinlik verileri RAM dizisi yerine MongoDB'de kalıcı hale getirilmiştir.

## Kurulum

```bash
cd backend
npm install
```

MongoDB Atlas bağlantı adresini yalnızca kendi `backend/.env` dosyana yaz:

```env
PORT=3000
MONGODB_URI=mongodb+srv://KULLANICI:SIFRE@CLUSTER_ADRESI/kampus
```

> Gerçek Atlas adresi, kullanıcı adı veya şifre GitHub'a, README'ye, `.env.example`a, Postman collection'a veya ekran görüntüsüne yazılmaz.

## Seed

Veritabanını 6 örnek etkinlikle doldur:

```bash
npm run seed
```

Seed her çalıştığında önce `events` koleksiyonunu temizler, sonra aynı 6 kaydı ekler. Bu nedenle iki kez çalıştırınca da kayıt sayısı 6 kalır.

## Çalıştırma

```bash
npm run dev
```

Sunucu:

```text
http://localhost:3000
```

## Endpointler

| Method | Endpoint | Başarılı | Hata |
|---|---|---:|---:|
| GET | `/health` | 200 | — |
| GET | `/events` | 200 | — |
| GET | `/events/:id` | 200 | 404 |
| POST | `/events` | 201 | 400 |

`GET /events?category=Atölye` ile kategori filtresi kullanılabilir.

## MongoDB / Mongoose

- Bağlantı: `src/config/database.js`
- Model: `src/models/event-model.js`
- Seed verisi: `src/data/seed-events.js`
- Seed scripti: `src/seed.js`
- Controller: `src/controllers/event-controller.js`

Modelde `required`, `trim`, `minlength`, `maxlength`, `enum`, `min`, `default` ve `timestamps` kuralları kullanılmaktadır.

## Postman

6 istek:

```text
requests/kampus-api.json
```

Sprint 8'de `_id` MongoDB tarafından üretildiği için tek etkinlik isteği `event-2` kullanmaz. Collection, `GET /events` yanıtındaki ilk gerçek `_id` değerini otomatik olarak `eventId` değişkenine kaydeder.

## Kalıcılık testi

1. `npm run seed`
2. `npm run dev`
3. `POST /events` ile yeni kayıt ekle → 201
4. `GET /events` → 7 kayıt
5. Sunucuyu `Ctrl+C` ile kapat
6. Tekrar `npm run dev`
7. `GET /events` → eklenen kayıt hâlâ vardır

Bu davranış Sprint 8'in kalıcılık şartını doğrular.
