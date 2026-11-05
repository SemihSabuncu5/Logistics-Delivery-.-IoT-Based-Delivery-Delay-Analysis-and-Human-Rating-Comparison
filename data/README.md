# Veri Kartı – Zomato teslimat veri seti

## Kaynak
- Dosya: `Zomato Dataset.csv`
- Dosyanın temin edilme biçimi: Öğrencinin sağladığı `archive(1).zip` arşivi.
- Özgün yayıncı / URL: Arşiv içinde belirtilmiyor; doğrulanamadı.
- Lisans: Arşiv içinde lisans metni veya lisans URL'si bulunmuyor; kullanım izni teyit edilmeli.
- İndirme tarihi: Öğrencinin beyanına göre ayrıca kaydedilmelidir; sistem tarihi olarak 05.11.2026 dosya inceleme tarihi yazılmıştır, özgün indirme tarihi olduğu iddia edilmez.
- Ham dosya: `data/raw/Zomato Dataset.csv` (elle değiştirilmemiştir).
- Temiz dosya: `data/processed/deliveries_cleaned.csv`.

## Boyut ve kapsam
- Ham satır: 45,584
- Ham sütun: 20
- Veri dönemi: 2022-02-11 – 2022-04-06
- Teslimat kimliği: 45,584 benzersiz
- Kurye kimliği: 1,320 benzersiz

## Temizleme / dönüştürme
1. Sütun adları SQL kullanımına uygun `snake_case` biçimine dönüştürüldü.
2. Metin alanlarının baş/son boşlukları temizlendi; boş metinler NULL olarak ele alındı.
3. `Order_Date`, gün-ay-yıl formatında tarihe dönüştürüldü.
4. `Time_Orderd` ve `Time_Order_picked` alanlarındaki `HH:MM` değerleri parse edildi. `0.833333333` gibi 0–1 arası gün kesri değerleri saate dönüştürüldü. Parse edilemeyen değerler NULL bırakıldı.
5. Sipariş zamanı ile paket alma zamanı birleştirilerek `order_datetime` ve `pickup_datetime` oluşturuldu. Paket alma saati sipariş saatinden küçükse ertesi gün varsayımı uygulandı.
6. Kurye puanında 1–5 aralığı dışındaki değerler NULL yapıldı. Ham dosyada bu dönüşüm yapılmadı.
7. Restoran ve teslimat koordinatlarından Haversine formülüyle `straight_line_distance_km` hesaplandı. 0 km veya 100 km üzeri değerler şüpheli kabul edilerek NULL bırakıldı.
8. Gerçek teslimat süresi (`time_taken_min`) değiştirilmedi.
9. Eksik değerler ortalama/medyan ile doldurulmadı.

## Veri kalitesi notları
- Kurye yaşı, puanı, sipariş saati, hava, trafik, çoklu teslimat ve şehir alanlarında eksikler vardır.
- Puan alanında 1–5 aralığı dışındaki kayıtlar bulunmuştur; temizlenmiş kopyada NULL yapılmıştır.
- Saat alanlarında sayısal gün kesri formatı ve eksik/çözümlenemeyen değerler vardır.
- `time_taken_min` mevcut veri setinde gerçekleşen teslimat süresidir; planlanan süre değildir.
- `Delivery_person_Ratings` gecikme kararı için ayrı insan doğrulaması değildir.

## Üretilen veri
Sentetik teslimat, sensör, ivmeölçer veya insan puanlama satırı üretilmemiştir. Türetilen tek ölçümler tarih-saat alanları ve koordinatlardan hesaplanan kuş uçuşu mesafedir; bunlar veri kartında açıkça belirtilmiştir.
