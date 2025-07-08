---
Harika, mentorumun isteği üzerine, komponent listesi ve IC odaklı ilerleme planımı destekleyecek bir **kontrol listesi** oluşturuyorum. Bu liste, projenin bu aşamasında hiçbir detayı atlamamak adına bana rehberlik edecek.

---

### **Gigabit Ethernet Switch Donanım Tasarımı: Komponent Seçimi ve IC Odaklı Kontrol Listesi**

Bu kontrol listesi, ana IC'ler (Switching ASIC, Yönetim CPU'su, Ethernet PHY'ler, PoE Kontrolcü) ve bunların çevresel bileşenlerinin belirlenmesi sürecinde bana yol gösterecek.

---

### **Genel Yaklaşım:**

* **Önce Ana IC'ler:** İlk öncelik olarak ana IC'leri (Switching ASIC, Yönetim CPU'su) ve aralarındaki kritik bağlantıları belirleyeceğim.
* **Veri Sayfası Odaklılık:** Seçilen ana IC'lerin veri sayfalarını adım adım inceleyerek, her bir pinin ve fonksiyonun gerektirdiği çevresel komponentleri (kapasitörler, dirençler, osilatörler vb.) tespit edeceğim.
* **Referans Tasarımlar:** Üreticilerin referans tasarımlarını ve Altium projesini, tipik uygulama şemalarını ve kullanılan komponent değerlerini anlamak için kullanacağım.
* **Tedarik ve Maliyet:** Seçilen komponentlerin piyasada bulunabilirliğini ve maliyetini göz önünde bulunduracağım.

---

### **Kontrol Listesi:**

#### **1. Anahtarlama ASIC'i (Switching ASIC) ve Çevresel Komponentler:**

* **ASIC Model Seçimi:** (Örn: Broadcom BCM53xx, Marvell 88E6xxx, Realtek RTL83xx)
    * **Güç Beslemesi (VCC):**
        * decoupling kapasitörler (Örn: 0.1uF, 1uF)
        * Bulk kapasitörler (Örn: 10uF, 47uF)
        * Ferrit boncuklar (Gürültü filtreleme için)
        * Voltaj regülatörleri (ASIC'in gerektirdiği farklı voltaj rayları için, Örn: 3.3V, 1.8V, 1.0V)
    * **Saat Kaynağı (Clock Source):**
        * Kristal osilatör (Örn: 25MHz) veya
        * Yüksek frekanslı osilatör (Örn: 125MHz)
        * İlgili kapasitörler ve dirençler
    * **Reset Devresi:**
        * Reset entegresi veya
        * RC devresi (Direnç, Kapasitör)
    * **CPU Arayüzü Bağlantıları (SPI, I2C, MDIO/MDC):**
        * Çekme/çekme-aşağı dirençler (Pull-up/Pull-down resistors)
        * Seri dirençler (Sinyal bütünlüğü için)
    * **Seri Veri Arayüzleri (SGMII, QSGMII, RGMII):**
        * AC kuplaj kapasitörleri (Örn: 10nF, 100nF)
        * Terminasyon dirençleri (Sinyal yansımalarını önlemek için)
    * **GPIO'lar (General Purpose Input/Output):**
        * Varsayılan durum için çekme dirençleri
        * LED sürücü dirençleri

#### **2. Yönetim CPU'su (MCU/CPU) ve Çevresel Komponentler:**

* **CPU Model Seçimi:** (Örn: NXP i.MX, STM32MP1, Microchip SAMxxxx)
    * **Güç Beslemesi (VCC):**
        * Decoupling kapasitörler (Örn: 0.1uF, 1uF)
        * Bulk kapasitörler (Örn: 10uF, 47uF)
        * Voltaj regülatörleri (Örn: 3.3V, 1.2V)
    * **Saat Kaynağı:**
        * Kristal osilatör (Örn: 24MHz, 40MHz)
        * İlgili kapasitörler ve dirençler
    * **Reset Devresi:**
        * Reset entegresi veya
        * RC devresi (Direnç, Kapasitör)
    * **Bellek Arayüzü (RAM/Flash):**
        * RAM (Örn: DDR3/DDR4 SDRAM, 128MB)
        * Flash Bellek (Örn: NAND Flash, NOR Flash, eMMC, 32MB - 128MB)
        * İlgili terminasyon dirençleri ve decoupling kapasitörler
    * **CPU-ASIC Arayüz Bağlantıları:**
        * Çekme dirençleri
        * Seri dirençler
    * **Ethernet MAC/PHY Entegrasyonu:**
        * Eğer CPU'da entegre değilse, harici Ethernet PHY (Örn: RTL8201F, KSZ8081)
        * Magnetik transformatörler (RJ-45 için)
        * İlgili pasifler (dirençler, kapasitörler)

#### **3. Ethernet PHY Çipleri (Her Port Grubu İçin Tekrarlanacak):**

* **PHY Model Seçimi:** (Örn: Marvell 88E151x, Microchip KSZ9031, Realtek RTL8211F)
    * **Güç Beslemesi (VCC):**
        * Decoupling kapasitörler (Örn: 0.1uF, 1uF)
        * Bulk kapasitörler (Örn: 10uF)
    * **Saat Kaynağı:**
        * Kristal osilatör (Örn: 25MHz) veya
        * ASIC'ten gelen referans saat hattı
    * **Seri Arayüz Bağlantıları (SGMII, RGMII):**
        * AC kuplaj kapasitörleri (Örn: 10nF, 100nF)
        * Terminasyon dirençleri
    * **RJ-45 Arayüzü:**
        * Magnetik transformatörler (Entegre veya harici)
        * Ortak mod bobinleri (Common mode chokes)
        * ESD koruma diyotları (TVS diyotları)
    * **LED Sürücüler:**
        * Akım sınırlama dirençleri

#### **4. PoE Güç Alt Sistemi (8 Port İçin):**

* **PoE Yöneticisi/PSE Kontrolcü IC Seçimi:** (Örn: Microchip PD69210, TI TPS23881)
    * **Güç Beslemesi (VCC):**
        * Decoupling kapasitörler
        * Bulk kapasitörler
    * **Kontrol Arayüzü (I2C/SPI):**
        * Çekme dirençleri
    * **Port Başına Güç Devreleri (8 Adet):**
        * Güç MOSFET'leri (PD692xx ile harici, TPS2388x ile entegre olabilir)
        * Akım algılama dirençleri (Sense resistors)
        * Koruma diyotları (Free-wheeling diodes, TVS diyotları)
        * Aşırı akım ve kısa devre koruma bileşenleri

#### **5. Genel Sistem Bileşenleri:**

* **Güç Kaynağı Konnektörü:** (DC jak, Vidalı terminal)
* **Harici Güç Adaptörü:** (Örn: 12V / 15A AC-DC adaptör, min 180W)
* **Dahili DC-DC Dönüştürücüler:** (Farklı voltaj rayları için, Örn: 12V'tan 5V, 3.3V, 1.8V, 1.0V'a)
    * İndüktörler (Bobinler)
    * Çıkış/Giriş kapasitörleri (Örn: Elektrolitik, Seramik)
    * MOSFET'ler (Eğer hariciyse)
* **Sistem LED'leri:**
    * Güç gösterge LED'i
    * PoE durum LED'i
    * Sistem durum/hata LED'i
    * Akım sınırlama dirençleri
* **Reset Butonu:** (Küçük taktik anahtar)
* **Seri Konsol Portu (Debug/Yönetim için):**
    * UART-USB dönüştürücü IC (Örn: FT232R) veya
    * Sadece UART pin çıkışları
    * İlgili kapasitörler/dirençler

---

Bu kontrol listesi, bir sonraki aşamada veri sayfalarını incelemeye başladığımda her bir bileşenin gerektirdiği alt komponentleri daha sistemli bir şekilde belirlememe yardımcı olacak. Her bir ana IC'nin veri sayfasında bu listeyi referans alarak ilerleyeceğim.
