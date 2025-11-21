# 14_Buton_LED_Toggle (Latching Switch Logic)

Bu proje, **STM32F407-Discovery** kartı üzerinde bir butona basıldığında LED'in durumunu tersine çeviren (AÇIK ise KAPALI, KAPALI ise AÇIK yapan) bir uygulamadır.

Bu depo, `HAL_GPIO_TogglePin` fonksiyonunun kullanımını ve yaylı (momentary) bir butonu, yazılımsal olarak kalıcı bir anahtara (latching switch) dönüştürme mantığını gösterir.

---

### 🎯 Proje Senaryosu ve Mantığı

Bu uygulamanın temel amacı, LED'in mevcut durumunu hafızada tutmak ve her buton basımında bu durumu değiştirmektir.

1.  **Algılama:** Butona basıldığı (`RESET`) tespit edilir.
2.  **Toggle (Tersleme):** `HAL_GPIO_TogglePin` komutu ile pin `1` ise `0`, `0` ise `1` yapılır.
3.  **Bekleme (Rate Limiting):** İşlemden sonra `HAL_Delay(2000)` ile 2 saniye beklenir.
    * **Neden Bekliyoruz?** Mikrodenetleyici çok hızlıdır. Eğer bu bekleme olmazsa, siz butona "bir kere" bastığınızı sanırken, kod binlerce kez çalışıp LED'i gözle görülmeyecek hızda yakıp söndürebilir. Bu bekleme, her basışın tek bir işlem yapmasını garanti altına alır.

---

### ⚙️ Pull-Up Konfigürasyonu

Projenin düzgün çalışması için `.ioc` dosyasında buton pininin (`PA0`) **Pull-Up** olarak ayarlanması gereklidir (Önceki buton projeleriyle aynı).

* **Pin:** `PA0` -> `GPIO_Input`
* **Resistor:** `Pull-up`

<img width="843" height="644" alt="image" src="https://github.com/user-attachments/assets/a5bccc60-b813-4f18-9e9a-a4f0fd3519bf" />

---

### 🛠️ Gerekli Donanım

* **1x** STM32F407-Discovery Geliştirme Kartı
* **1x** LED
* **1x** 220 Ohm Direnç
* **1x** Push-Button
* **Breadboard ve Jumper Kablolar**

---

### 🔌 Devre Şeması

Buton bağlantısı **Pull-Up** mantığına göre (GND'ye) yapılmalıdır.

| Bileşen | STM32 Pini | Bağlantı Detayı |
| :--- | :--- | :--- |
| **Buton** | `PA0` | Bir bacak **PA0**, diğer bacak **GND** |
| **LED** | `PA1` | Anot -> **PA1**, Katot -> Direnç -> **GND** |


<img width="403" height="560" alt="image" src="https://github.com/user-attachments/assets/1f49cc65-e2c6-4f64-a080-7d3470171d79" />

---

### 💻 Kod Bloğu

<img width="991" height="243" alt="image" src="https://github.com/user-attachments/assets/f1ee9f6c-80fe-4b77-a98f-0e3e7bc16576" />

---

### 🚀 Nasıl Kullanılır?

1.  Bu depoyu klonlayın (`git clone ...`).
2.  STM32CubeIDE yazılımını açın.
3.  `File > Open Projects from File System...` seçeneği ile proje klasörünü seçin.
4.  Proje içindeki `.ioc` dosyasını açarak pin yapılandırmasını inceleyebilirsiniz.
5.  Derleyin (Build) ve ST-Link V2 üzerinden kartınıza yükleyin (Run).
