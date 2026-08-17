# Volley & Chill – Event Feedback Formu

10. etkinlikten sonra oluşturulan feedback formu. WhatsApp grubunda paylaşılıp gelen yanıtlar otomatik olarak Google Sheets'e kaydediliyor.

## Nasıl Çalışıyor

```
Kullanıcı (WhatsApp linki) 
  → index.html (GitHub Pages)
  → fetch POST → n8n Webhook
  → n8n: Google Sheets (append)
  → n8n: Respond to Webhook
```

## Bileşenler

| Parça | Konum |
|---|---|
| Form (statik site) | `index.html` → [https://grknc.github.io/volley-feedback/](https://grknc.github.io/volley-feedback/) |
| Webhook (production) | `https://n8n.canakci.xyz/webhook/volley-chill-feedback` |
| Kayıt tablosu | Google Sheets – `volley-feedback` dosyası, `Feedback` sekmesi |

## Kurulum (özet)

1. `index.html` bu repoda root'ta duruyor, GitHub Pages Settings → Pages → Deploy from branch (main / root) ile yayınlanıyor.
2. `index.html` içindeki `WEBHOOK_URL` sabiti production webhook'a işaret ediyor (test URL değil).
3. n8n'de workflow: **Webhook → Google Sheets → Respond to Webhook**, workflow **Active** olmalı.
4. Google Sheets node'unda mapping mode **"Map Each Column Manually"**, her alan `{{ $json.body.<field> }}` ile eşleniyor.

## Değişiklik Yaparken

- Forma yeni bir alan eklersen: hem `index.html`'deki `payload` objesine, hem n8n'deki Sheets node mapping'ine, hem de Sheets'teki başlık satırına eklemen gerekir.
- n8n'de node ayarı değiştirdikten sonra **mutlaka Save** — kaydetmezsen production webhook eski ayarla çalışmaya devam eder.
- Test için Postman kullanılabilir ama Production URL (`webhook/...`) ile — Test URL (`webhook-test/...`) sadece n8n editöründe "Listen for test event" açıkken çalışır.

## Kayıtlı Alanlar

`event_number`, `overall_rating`, `organization_satisfaction`, `team_balance`, `players_per_court`, `event_duration`, `atmosphere_rating`, `join_again`, `future_event_preferences`, `improvement_feedback`, `best_part`, `name`, `source`, `submitted_at`
