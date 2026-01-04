# 🕵️‍♂️ Crypto Alpha Scanner (On-Chain Intelligence)

> **"Piyasayı Takip Etmeyin, Piyasadan Önce Hareket Edin."**

![Banner](https://img.shields.io/badge/Status-Production%20Ready-success?style=for-the-badge) ![n8n](https://img.shields.io/badge/Built%20With-n8n-orange?style=for-the-badge) ![Chain](https://img.shields.io/badge/Network-EVM%20Compatible-blue?style=for-the-badge)

## 📖 Proje Özeti
**Crypto Alpha Scanner**, blokzincir üzerindeki (On-Chain) hareketleri **milisaniyeler içinde** analiz eden, henüz borsalara veya sosyal medyaya düşmemiş "Alpha" fırsatları yakalayan gelişmiş bir otomasyon sistemidir. Normal yatırımcılar CoinGecko'ya bakarken, bu bot doğrudan mempool ve blok verilerini izler.

### 🚀 Neden Bu Workflow? (Business Value)
*   **🐋 Balina Avcısı:** Büyük cüzdanların (Whales) yaptığı alım/satım işlemlerini anında tespit edin.
*   **⚡ Sniper Potansiyeli:** Yeni likidite havuzları (Liquidity Pools) eklendiği anda haberdar olun.
*   **🛡️ Erken Uyarı Sistemi:** Rug-pull veya ani satış baskılarını grafiklere yansımadan önce görün.
*   **🤖 Tam Otonom:** 7/24 çalışır, uyumaz, duygusal karar vermez.

---

## ⚙️ Teknik Mimari (Under the Hood)

Bu workflow, sıradan bir fiyat takipçisi değildir. **Web3 altyapısı** ile **AI analizini** birleştirir.

### Kullanılan Teknolojiler
*   **n8n (Workflow Engine):** Tüm mantıksal akışı yöneten beyin.
*   **Alchemy / Infura / QuickNode (RPC Provider):** Blokzincir verisine doğrudan erişim sağlayan websocket/HTTP düğümleri.
*   **Etherscan API:** Akıllı kontratları çözümlemek ve ABI (Application Binary Interface) verilerini okumak için.
*   **Telegram / Discord Webhooks:** Tespit edilen sinyalleri anlık olarak ekibe iletmek için.

### Çalışma Mantığı (Step-by-Step)
1.  **Block Listener (Cron/Webhook):** Her yeni blok üretiminde veya belirli eventlerde (örn: `PairCreated` Uniswap) tetiklenir.
2.  **Transaction Filtering:** Gelen binlerce işlem arasından sadece "Alpha" niteliği taşıyanları (Büyük hacim, Yeni Token, Özel Kontrat Etkileşimi) filtreler.
3.  **Data Ingestion & Enrichment:**
    *   Token adresini alır, CoinGecko/DexScreener API'larından metadata (isim, market cap) çeker.
    *   Honeypot kontrolü yapar (İsteğe bağlı GoPlus/TokenSniffer entegrasyonu).
4.  **AI Analysis (Opsiyonel):** İşlemin şüpheli mi yoksa fırsat mı olduğunu anlamak için LLM (GPT-4o) yorumu ekler.
5.  **Broadcast:** Hazırlanan zengin içerikli raporu Telegram kanalına "Al/Sat" butonu ile birlikte gönderir.

---

## 🛠️ Kurulum Rehberi (Deployment)

Bu sistemi kendi sunucunuzda ayağa kaldırmak için aşağıdaki adımları izleyin.

### Ön Gereksinimler
*   Self-hosted n8n veya n8n Cloud hesabı.
*   Etherscan API Key (Ücretsiz).
*   Alchemy veya Infura API Key (RPC bağlantısı için).
*   Telegram Bot Token (Bildirimler için).

### Adım 1: Workflow'u İçe Aktarın
1.  `Crypto Alpha Scanner` klasöründeki `workflow.json` dosyasını indirin.
2.  n8n panelinizde **"Import from File"** diyerek yükleyin.

### Adım 2: Credential Ayarları
Workflow içindeki şu düğümlere API anahtarlarınızı girin:
*   `Node: Etherscan Scan` -> Header Auth: `x-api-key: SENIN_API_KEYIN`
*   `Node: Telegram` -> Bot Token: `SENIN_TELEGRAM_BOT_TOKENIn`

### Adım 3: Filtreleri Özelleştirin
`Set Thresholds` düğümünü açarak kendi stratejinizi belirleyin:
```javascript
{
  "min_transfer_value_usd": 10000,  // Min 10k $'lık işlemleri bildir
  "watch_tokens": ["ETH", "SOL"],   // Sadece bu ağları izle
  "ignore_contracts": ["0x..."]     // Bilinen borsaları yoksay
}
```

---

## 🎯 Kullanım Senaryoları (Use Cases)

### Senaryo A: Yeni Gem Avcısı (Meme Token Hunter)
*   **Ayar:** Uniswap V2/V3 fabrikalarını izle. `PairCreated` eventini dinle.
*   **Sonuç:** Token daha DexScreener'a düşmeden kontrat adresini alırsın.

### Senaryo B: Balina Takipçisi (Whale Watcher)
*   **Ayar:** 100.000$ üzeri Stabil Coin (USDT/USDC) transferlerini izle.
*   **Analiz:** Eğer bir balina borsaya yüklü USDT gönderiyorsa, alım yapacaktır (Bullish). Borsadan soğuk cüzdana çekiyorsa, HODL yapacaktır (Bullish).

### Senaryo C: Smart Money Copy-Trade
*   **Ayar:** Başarılı olduğu bilinen cüzdan adreslerini izleme listesine al.
*   **Sonuç:** Onlar ne alırsa, bot anında sana bildirim atar.

---

## ⚠️ Yasal Uyarı
Bu yazılım bir yatırım tavsiyesi değildir. On-chain veriler manipüle edilebilir. Kendi araştırmanızı (DYOR) yapmadan işlem yapmayınız.

---
**Maintained by:** [xCodeWraith DEV.]
**License:** MIT
