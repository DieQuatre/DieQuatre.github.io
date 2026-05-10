# ISUCPU Project 4 - Soru 1 Çözüm Rehberi

> Kaynak PDF (yalnızca referans):  
> https://drive.google.com/file/d/1sCpZNtWYRT1dWP9dStiuTP5VjQEnANdu/view?usp=sharing

## 1) Soru 1 senden tam olarak ne istiyor?

Bu soruda ISUCPU üzerinde bir kalp atışı izleme akışı kurman bekleniyor:

- Sensörden `Current_Time` oku.
- `Elapsed_Time = Current_Time - Last_Time` hesapla.
- Bu örnek çözümde sensör zamanının **saniye** cinsinden geldiği varsayımıyla `BPM = 60 / Elapsed_Time` hesapla. (Eğer giriş ms ise sabit 60 yerine 60000 kullanılmalıdır.)
- BPM değeri `< 40` ise “düşük nabız” sayacını artır.
- Bu durum **10 ölçüm üst üste** olursa buzzer çıkışını tetikle.
- Hesaplanan BPM değerlerini bellekte dairesel (circular) şekilde sakla.

Özetle: Hem doğru aritmetik, hem doğru eşik mantığı, hem de taşmadan çalışan bir circular buffer isteniyor.

## 2) Yaklaşım (adım adım)

1. `Last_Time` değerini RAM'de sabit bir adreste tut.
2. Her döngüde yeni zamanı oku, önceki zamanla farkı al.
3. Fark `0` ise bölme hatasını engellemek için BPM hesaplamasını atla.
4. Fark `0` değilse `60 / elapsed` ile BPM üret.
5. BPM'i buffer adresine yaz, pointer'ı artır.
6. Pointer üst sınıra gelince başlangıç adresine sar (wrap).
7. BPM `< 40` ise düşük-nabız sayacını artır, değilse sıfırla.
8. Sayaç `10` olunca buzzer üret ve sayacı yeniden sıfırla.

## 3) Sık yapılan hatalar ve düzeltmeler

### Hata A — DIV sırasında yanlış register kullanımı
Sık hata: `elapsed` bir register'da hesaplanıp sonra üstüne `60` yazılması; ardından bölmede eski/yanlış register'ın kullanılması.

**Düzeltme:**
- `elapsed` değerini ayrı register'da koru.
- `60` sabitini ayrı register'da üret.
- `DIV` komutunda gerçekten `60 / elapsed` yaptığından emin ol.

### Hata B — Bellek üst sınırının yanlış hesaplanması/kaydedilmesi
Sık hata: 8192 yerine ara değer (ör. 32) belleğe yazmak.

**Düzeltme:**
- `BUFFER_END = 8192` değerini gerçekten 8192 olacak şekilde hesapla ve kaydet.
- Pointer karşılaştırmasını bu gerçek üst sınıra göre yap.

### Hata C — Circular buffer wrap sınırında off-by-one
Sık hata: geçersiz adrese bir kez yazdıktan sonra wrap yapmak.

**Düzeltme:**
- Yazma sonrası pointer artırılıyorsa, `PTR == BUFFER_END` olduğunda hemen `BUFFER_START`'a dön.
- Buffer aralığını net düşün: ör. `[256, 8191]` yazılabilir; `8192` sınır/sonraki adres.

## 4) Düzeltilmiş örnek assembly (çalışan akış)

> Not: Aşağıdaki kod, verilen örnekteki komut tarzıyla uyumlu tutulmuştur (`ADD src,dst`, `STORE reg,[addrReg]`, vb.).
> Bu listede `DIV R0, R7` satırı BPM sonucunu `R7` register'ında üretmek üzere kullanılmıştır.
> Ayrıca örnek, zaman girişinin saniye cinsinden geldiği varsayımıyla hazırlanmıştır.

```asm
; ISUCPU Project 4 - Q1 (Heartbeat + Circular Buffer)
; Varsayım: 8k x 16 bellek, buffer başlangıcı 256, sınır 8192 (exclusive)
; Port 1: zaman girişi, Port 2: buzzer çıkışı

; --- Sabit/Adres düzeni (RAM) ---
; [0] = Last_Time
; [1] = Buffer_End   (8192)
; [2] = Buffer_Start (256)

MOV R4, 1          ; sabit 1 (artırma)

; Last_Time = 0
MOV R0, 0
STORE R0, [R0]

; Buffer_Start = 256
MOV R6, 16
MOV R7, 16
MUL R6, R7         ; R7 = 256
MOV R6, 2
STORE R7, [R6]     ; [2] = 256
LOAD R3, [R6]      ; R3 = write pointer = 256

; Buffer_End = 8192 (= 256 * 32)
MOV R6, 31
ADD R4, R6         ; R6 = 32
MUL R6, R7         ; R7 = 256 * 32 = 8192
MOV R0, 1
STORE R7, [R0]     ; R0 bir üst satırda 1 yapıldığı için yazma adresi 1 olur

; threshold = 40
MOV R1, 20
MOV R0, 20
ADD R0, R1         ; R1 = 40

; low_count_limit = 10
MOV R5, 10
MOV R2, 0          ; R2 = consecutive low counter

MAIN:
IN R0, 1           ; R0 = Current_Time

; elapsed hesapla
MOV R6, 0
LOAD R7, [R6]      ; R7 = Last_Time
STORE R0, [R6]     ; Last_Time = Current_Time
SUB R7, R0         ; R0 = Current_Time - Last_Time (elapsed)

; elapsed == 0 ise bölme yapma
MOV R6, 0
CMP R0, R6
BEQ MAIN

; BPM = 60 / elapsed
MOV R7, 30
ADD R7, R7         ; R7 = 60
DIV R0, R7         ; ISUCPU ders notundaki kullanım varsayımıyla BPM sonucu R7'de tutulur

; BPM'i circular buffer'a yaz
STORE R7, [R3]
ADD R4, R3         ; R3 = R3 + 1

; wrap kontrolü: pointer == 8192 ise 256'ya dön
MOV R6, 1
LOAD R0, [R6]      ; R0 = 8192
CMP R3, R0
BNE NO_WRAP
MOV R6, 2
LOAD R3, [R6]      ; R3 = 256

NO_WRAP:
; BPM < 40 ?
CMP R7, R1
BLT LOW_HR

; düşük değilse sayaç sıfırlanır
MOV R2, 0
JMP MAIN

LOW_HR:
ADD R4, R2         ; low_count++
CMP R2, R5
BLT MAIN

; 10 ardışık düşük ölçüm -> buzzer
OUT R7, 2
MOV R2, 0
JMP MAIN
```

## 5) Gereksinim -> Kod eşlemesi

| Gereksinim | Kod parçası |
|---|---|
| Sensörden zaman oku | `IN R0, 1` |
| `Elapsed_Time = Current - Last` | `LOAD R7,[0]` + `STORE R0,[0]` + `SUB R7, R0` |
| `BPM = 60 / Elapsed` | `MOV/ADD` ile R7'ye 60 yüklenir, `DIV R0, R7` sonrası R7 BPM değerini taşır |
| BPM'leri belleğe yaz | `STORE R7, [R3]` |
| Circular buffer wrap | `ADD R4,R3` + `CMP R3,Buffer_End` + `LOAD R3,Buffer_Start` |
| Eşik kontrolü (`< 40`) | `CMP R7, R1` + `BLT LOW_HR` |
| 10 ardışık düşük ölçüm | `ADD R4,R2` + `CMP R2,R5` |
| Buzzer tetikleme | `OUT R7, 2` |

## 6) Kısa kontrol listesi (teslimden önce)

- `DIV` satırında pay/payda register'ları doğru mu?
- 8192 değeri gerçekten `Buffer_End` olarak belleğe yazılıyor mu?
- Wrap koşulu, geçersiz adrese yazmadan çalışıyor mu?
- BPM `< 40` değilse sayaç sıfırlanıyor mu?
- `Elapsed_Time == 0` durumunda bölme korunuyor mu?
