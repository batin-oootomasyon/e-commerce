# 🛍️ WhatsApp AI E-Ticaret Asistanı (n8n Workflow)

Bu proje, **n8n**, **OpenAI** ve **Google Sheets** kullanarak oluşturulmuş akıllı bir WhatsApp satış ve destek asistanıdır.

Bu bot, müşterilerinizle doğal dilde sohbet eder, ürün stoklarını kontrol eder, sık sorulan soruları yanıtlar ve siparişleri otomatik olarak veritabanına (Google Sheets) kaydeder.


## 🚀 Özellikler

* **💬 Doğal Dil İşleme:** Müşterilerinizle robotik olmayan, akıcı bir dille konuşur.
* **📦 Stok Kontrolü:** "Elinizde kırmızı tişört var mı?" sorusuna anlık olarak stok listesinden bakarak yanıt verir.
* **🛒 Sipariş Yönetimi:** Müşteri satın almak istediğinde siparişi, tutarı ve müşteri bilgilerini kaydeder.
* **ℹ️ 7/24 Müşteri Desteği:** İade, kargo vb. konulardaki soruları SSS listesinden yanıtlar.
* **🧠 Hafıza:** Konuşma geçmişini hatırlar, böylece müşteri "Fiyatı ne kadar?" dediğinde hangi üründen bahsettiğini bilir.

---

## 🛠️ Gereksinimler

Bu projeyi çalıştırmak için aşağıdakilere ihtiyacınız vardır:

1.  **n8n:** Self-hosted veya Cloud versiyonu.
2.  **OpenAI API Key:** GPT-4o veya GPT-4-Turbo önerilir.
3.  **Meta for Developers Hesabı:** WhatsApp Business API kurulumu için.
4.  **Google Cloud Console:** Google Sheets API'sini etkinleştirmek ve Service Account oluşturmak için.

---

## 🗃️ Google Sheets Veritabanı Kurulumu (ÖNEMLİ)

Botun doğru çalışması için **tek bir Google E-Tablo** oluşturun ve içinde aşağıda belirtilen **3 ayrı sayfayı (sekme)** açın. Sütun isimlerinin birebir aynı olması önemlidir.

### 1. Sayfa Adı: `FAQ`
*Botun şirket politikaları (kargo, iade vb.) hakkında bilgi alacağı sayfa.*

| A (Soru) | B (Cevap) |
| :--- | :--- |
| Kargo kaç günde gelir? | Siparişleriniz 24 saat içinde kargoya verilir ve 3 gün içinde ulaşır. |
| İade süresi nedir? | Ürünü teslim aldıktan sonra 14 gün içinde iade edebilirsiniz. |

### 2. Sayfa Adı: `Inventory`
*Ürünlerin, fiyatların ve stok durumunun tutulduğu sayfa.*

| A (Urun_Adi) | B (Fiyat) | C (Stok_Adedi) | D (Aciklama) |
| :--- | :--- | :--- | :--- |
| Oversize T-Shirt | 350 TL | 50 | %100 Pamuk, Siyah renk |
| Kot Ceket | 1200 TL | 10 | Mavi, taşlanmış |

### 3. Sayfa Adı: `Orders`
*Botun aldığı siparişleri kaydedeceği sayfa. Başlangıçta boş bırakın.*

| A (Musteri_Telefon) | B (Siparis_Detayi) | C (Toplam_Tutar) | D (Tarih) |
| :--- | :--- | :--- | :--- |
| (Boş) | (Boş) | (Boş) | (Boş) |

---

## ⚙️ Kurulum Adımları

1.  **Workflow'u İçe Aktarın:** Bu repodaki `.json` dosyasını n8n'e import edin.
2.  **Credential Ayarları:**
    * `OpenAI Chat Model` node'una API anahtarınızı girin.
    * `WhatsApp Trigger` ve `Send Message` node'larına Meta API bilgilerinizi girin.
    * `Google Sheets` node'larına Google Service Account bilgilerinizi girin.
3.  **Sheet ID Tanımlama:**
    * Workflow içindeki 3 farklı Google Sheets node'una (FAQ, Inventory, Orders) gidip, oluşturduğunuz tablonun ID'sini (URL'deki uzun kod) yapıştırın.
4.  **System Prompt (Opsiyonel):**
    * AI Agent node'una tıklayın ve "System Message" kısmını işletmenize göre özelleştirin.
    * *Örnek:* "Sen X Butik'in yardımsever asistanısın. Asla stokta olmayan ürünü satma."
5.  **Aktifleştirin:** Workflow'u "Active" konumuna getirin.

---

## 🤖 Nasıl Çalışır? (Teknik Özet)

1.  **Trigger:** WhatsApp'tan mesaj gelir.
2.  **Router:** Mesajın bottan mı yoksa kullanıcıdan mı geldiği kontrol edilir.
3.  **AI Agent:** Gelen mesajı analiz eder.
    * Eğer bilgi sorusuysa -> **FAQ Tool**'unu kullanır.
    * Eğer ürün sorusuysa -> **Inventory Tool**'unu kullanır.
    * Eğer sipariş onayıysa -> **Orders Tool**'unu kullanarak satır ekler.
4.  **Response:** Oluşturulan nihai cevap WhatsApp üzerinden kullanıcıya gönderilir.

---

## 🤝 Katkıda Bulunma

Geliştirmeler ve PR'lar memnuniyetle kabul edilir. Herhangi bir hata bulursanız lütfen "Issues" kısmından bildirin.

---

**Lisans:** MIT
