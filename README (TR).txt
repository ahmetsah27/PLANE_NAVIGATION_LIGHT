# RC Uçak Kanat ve Gövde Işıkları

Kağıt/RC uçak maketi için Arduino ile kontrol edilen gerçekçi navigasyon ve anti-çakışma (strobe) ışık sistemi. Sol kanatta kırmızı, sağ kanatta yeşil sabit pozisyon ışıkları; kanat ve gövdede senkronize çift-flaşlı çakar (strobe) ışıkları bulunur.

## Kullanılan Malzemeler

- Arduino Uno
- 3x Kırmızı LED (pozisyon ışığı)
- 1x Yeşil LED (pozisyon ışığı)
- 1x Sarı LED (kanat çakarı)
- 1x Sarı LED (gövde çakarı)
- Direnç (~220-330 ohm, her LED için)
- Breadboard ve jumper kablolar

## Pin Bağlantıları

| Pin | Görev | Davranış |
|---|---|---|
| 8 | Gövde çakarı (bodyYellow) | Çift flaş (strobe) |
| 9 | Kanat çakarı (wingYellow) | Çift flaş (strobe) |
| 10, 11, 12 | Kırmızı pozisyon ışıkları (redPins) | Sabit yanar |
| 13 | Yeşil pozisyon ışığı (greenPin) | Sabit yanar |

Devre şeması `ARDUINO_CIRCUIT_DIAGRAM.png` dosyasında mevcuttur.

## Çalışma Mantığı

Kod `loop()` içinde sürekli şu sırayı tekrar eder:

1. **Kanat çakarı** (pin 9) çift flaş yapar: 40ms açık → 100ms kapalı → 40ms açık → 500ms kapalı
2. **Gövde çakarı** (pin 8) çift flaş yapar: aynı zamanlama (40-100-40-500 ms)
3. Baştan tekrar başlar

Kırmızı ve yeşil pozisyon ışıkları (`redPins`, `greenPin`) `setup()` içinde bir kere `HIGH` yapılıp sabit bırakılır, `loop()` içinde hiç değiştirilmez — yani sürekli yanık kalırlar.

## Zamanlamayı Değiştirmek

Flaş sürelerini veya bekleme sürelerini değiştirmek için `loop()` içindeki `delay()` değerlerini düzenlemeniz yeterlidir:

```cpp
digitalWrite(wingYellow, HIGH);
delay(40);   // flaş süresi (ms)
digitalWrite(wingYellow, LOW);
delay(100);  // iki flaş arası bekleme (ms)
digitalWrite(wingYellow, HIGH);
delay(40);   // flaş süresi (ms)
digitalWrite(wingYellow, LOW);
delay(500);  // sonraki döngüye kadar bekleme (ms)
```

Kanat ve gövde çakarları için aynı yapı ayrı ayrı `wingYellow` ve `bodyYellow` bloklarında düzenlenebilir.

## Pin Değiştirmek

Farklı bir pine bağlamak isterseniz dosyanın en üstündeki değişkenleri güncellemeniz yeterlidir:

```cpp
int wingYellow  = 9;
int bodyYellow  = 8;
int redPins[]   = {10, 11, 12};
int greenPin    = 13;
```

## Notlar

- Her LED'e mutlaka uygun değerde direnç (~220-330 ohm) bağlanmalıdır.
- `redPins` dizisindeki 3 pin aynı anda, birlikte sabit yanacak şekilde ayarlanmıştır.
- Proje `delay()` tabanlı yazılmıştır; kod basit ve okunması kolay olacak şekilde tasarlanmıştır.