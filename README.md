

--------------------------------------
# A. Host (Hedef) Belirleme ve Keşif Parametreleri
________________________________________________________________________________________________________

### Parametre: `-iL <dosya adı>`

**Açıklama:** IP adreslerini veya host adlarını belirtilen bir dosyadan okur ve tarar. (Dosyada boş satır veya `#` ile başlayan yorum satırları atlanır.)

**Kullanım Amacı:** Büyük hedefleri veya spesifik listeleri toplu taramak için kullanılır.

**Örnek Komut:**

```bash
nmap -iL hedefler.txt
```

---

### Parametre: `-iR <host sayısı>`

**Açıklama:** Rastgele IP adresleri seçerek bu hedefleri tarar.

**Kullanım Amacı:** Genel internet keşfi veya rastgele hedeflerle testler için kullanılır.

**Örnek Komut:**

```bash
nmap -iR 50
```

---

### Parametre: `--exclude <host1>[,...]`

**Açıklama:** Tarama aralığından belirtilen hostları veya IP'leri hariç tutar.

**Kullanım Amacı:** Hassas veya bilinen sistemleri taramadan atlamak için kullanılır.

**Örnek Komut:**

```bash
nmap 192.168.1.0/24 --exclude 192.168.1.1,192.168.1.2
```

---

### Parametre: `--excludefile <dosya adı>`

**Açıklama:** Hariç tutulacak IP adreslerini bir dosyadan okur.

**Kullanım Amacı:** Çok sayıda hariç tutulacak IP adresi olduğunda kullanılır.

**Örnek Komut:**

```bash
nmap -iL liste.txt --excludefile atla.txt
```

---

### Parametre: `-sn`

**Açıklama (düzeltme):** Port taramasını atlar (no port scan) ve **sadece host keşfi** (host discovery) yapar. Bu, hiçbir port taraması yapılmayacağı, fakat ICMP/TCP/ARP gibi probe’ların gönderileceği anlamına gelir — "ping göndermemek" anlamına gelmez.

**Kullanım Amacı:** Canlı hostları hızlıca belirlemek için kullanılır.

**Örnek Komut:**

```bash
nmap -sn 192.168.1.0/24
```

---

### Parametre: `-Pn`

**Açıklama:** Host keşfini atlar; hedefin aktif olduğunu varsayar ve doğrudan port taramasına geçer.

**Kullanım Amacı:** ICMP/ping engellendiğinde veya hedefin kesinlikle up olduğu bilindiğinde taramaya devam etmek için kullanılır.

**Örnek Komut:**

```bash
nmap -Pn hedefsite.com
```

--------------------------------
# B. Host Keşif (Ping) Çeşitleri

> Not: Birçok keşif seçeneği **port numarası(lar)** ile kullanılır (örn. `-PS 80,443`). LAN içindeki en güvenilir keşif: **ARP ping (`-PR`)**.


-------------------------------------
### Parametre: `-PS` (TCP SYN Ping)

**Açıklama:** Hedefe TCP SYN paketi göndererek canlı host tespiti yapar.

**Kullanım Amacı:** ICMP engellendiğinde ya da belirli TCP portları üzerinden keşif için kullanılır.

**Örnek Komut:**

```bash
nmap -PS 443 hedef.com
```

---

### Parametre: `-PA` (TCP ACK Ping)

**Açıklama:** Hedefe TCP ACK paketi gönderir.

**Kullanım Amacı:** Güvenlik duvarı/filtre davranışını test etmek veya ACK yanıtı ile host tespiti yapmak için kullanılır.

**Örnek Komut:**

```bash
nmap -PA 80,443 hedef.com
```

---

### Parametre: `-PU` (UDP Ping)

**Açıklama:** Hedefe UDP paketi gönderir.

**Kullanım Amacı:** UDP üzerinden canlı host tespiti yapar (ör. DNS/53 gibi portlar). UDP paketleri genelde ICMP hatası dönebilir.

**Örnek Komut:**

```bash
nmap -PU 53 hedef.com
```

---

### Parametre: `-PE` (ICMP Echo)

**Açıklama:** ICMP Echo Request (standart ping) gönderir.

**Kullanım Amacı:** Klasik ping ile canlı host tespiti.

**Örnek Komut:**

```bash
nmap -PE hedef.com
```

---

### Parametre: `-PP` (ICMP Timestamp)

**Açıklama:** ICMP Timestamp isteği göndererek keşif yapar.

**Kullanım Amacı:** Bazı durumlarda farklı ICMP türleri daha iyi sonuç verir.

**Örnek Komut:**

```bash
nmap -PP hedef.com
```

---

### Parametre: `-PM` (ICMP Netmask)

**Açıklama:** ICMP Netmask isteği gönderir — bazı ağlarda faydalı olabilir.

**Örnek Komut:**

```bash
nmap -PM hedef.com
```

---

### Parametre: `-PR` (ARP Ping)

**Açıklama:** ARP istekleri gönderir; aynı yerel ağ (LAN) içinde en güvenilir host keşif yöntemidir.

**Kullanım Amacı:** LAN içindeki canlı host tespiti.

**Örnek Komut:**

```bash
nmap -PR 192.168.1.0/24
```


---
# C. Port Tarama Çeşitleri ve Kapsam

---

### Parametre: `-sS` (SYN Scan)

**Açıklama:** TCP SYN (yarı açık) taraması yapar. Stealth kabul edilir ve hızlıdır. **Root yetkisi** genelde gerekir (raw soket kullanımı).

**Kullanım Amacı:** Hızlı, yaygın ve genelde tespit edilme olasılığı daha düşük olan tarama.

**Örnek Komut:**

```bash
nmap -sS hedef.com
```

---

### Parametre: `-sT` (TCP Connect)

**Açıklama:** Tam TCP el sıkışması yapar (connect()). Root gerektirmez ama daha gürültülüdür.

**Kullanım Amacı:** Raw soket izni yoksa kullanılır.

**Örnek Komut:**

```bash
nmap -sT hedef.com
```

---

### Parametre: `-sA` (ACK Scan)

**Açıklama:** TCP ACK paketleri gönderir; portun filtrelenip filtrelenmediğini anlamaya yardımcı olur.

**Kullanım Amacı:** Güvenlik duvarı/filtre durumunu analiz etmek.

**Örnek Komut:**

```bash
nmap -sA hedef.com
```

---

### Parametre: `-sU` (UDP Scan)

**Açıklama:** UDP portlarını tarar. UDP scanning yavaş ve hata/yanıt bazlıdır.

**Kullanım Amacı:** DNS, SNMP, NTP gibi UDP servislerini keşfetmek için kullanılır.

**Örnek Komut:**

```bash
nmap -sU hedef.com
```

---

### Parametre: `-sN` (NULL), `-sF` (FIN), `-sX` (Xmas)

**Açıklama:** Bayraksız veya tek bayraklı stealth tarama türleri. Unix-benzeri TCP/IP implementasyonlarında farklı davranışlar gözlemlenebilir; **Windows** çoğunlukla RST döndüreceğinden bu yöntemler Windows üzerinde güvenilir değildir.

**Kullanım Amacı:** IDS/filtre tespiti veya belirli durumlarda gizli tarama.

**Örnek Komut:**

```bash
nmap -sN hedef.com
nmap -sF hedef.com
nmap -sX hedef.com
```

---

### Parametre: `-sO` (IP Protocol Scan)

**Açıklama:** Hedefin desteklediği IP protokollerini (ör. TCP, UDP, ICMP, GRE vb.) tespit eder.

**Kullanım Amacı:** IP katmanında hangi protokollerin açık olduğunu anlamak.

**Örnek Komut:**

```bash
nmap -sO hedef.com
```

---

### Parametre: `-p-` / `-p <portlar>`

**Açıklama:** `-p-` tüm portları (0-65535) tarar. `-p 80,443,1-1024` gibi belirli port aralıkları belirtilebilir.

**Kullanım Amacı:** Tüm portları taramak veya sadece spesifik portları hedeflemek.

**Örnek Komut:**

```bash
nmap -p- hedef.com
nmap -p 80,443,21 hedef.com
```

---

### Parametre: `-F` ve `--top-ports <sayı>`

**Açıklama:** `-F` hızlı tarama (varsayılan en popüler 100 port). `--top-ports N` ise en popüler N portu tarar.

**Kullanım Amacı:** Hız ve kapsam arasında denge kurmak.

**Örnek Komut:**

```bash
nmap -F hedef.com
nmap --top-ports 500 hedef.com
```

---
# D. Servis, Versiyon, OS, DNS ve Agresiflik

---
### Parametre: `-sV` (Version Detection)

**Açıklama:** Açık portlarda çalışan servisin versiyonunu tespit etmeye çalışır.

**Kullanım Amacı:** Hedef servislerin versiyon bilgisine göre zafiyet aramak için kullanılır.

**Örnek Komut:**

```bash
nmap -sV hedef.com
```

---

### Parametre: `-O` (OS Detection)

**Açıklama:** Paket/fingerprint yöntemleriyle hedefin işletim sistemini tahmin etmeye çalışır.

**Kullanım Amacı:** Hedef sistemin türünü (Windows, Linux, network appliance vb.) belirlemek.

**Örnek Komut:**

```bash
nmap -O hedef.com
```

---

### Parametre: `-sC` / `--script=default`

**Açıklama:** Nmap'in varsayılan scriptlerini çalıştırır. `-sC` genelde `--script=default` ile eşdeğerdir.

**Kullanım Amacı:** Açık portlarda ek bilgi toplamak ve temel kontroller yapmak.

**Örnek Komut:**

```bash
nmap -sV -sC hedef.com
```

---

### Parametre: `--script` ve `--script-args`

**Açıklama:** Nmap Scripting Engine (NSE) ile özel script/katagori çalıştırma. Örnek: `--script vuln`, `--script http-brute`.

**Kullanım Amacı:** Zafiyet taramaları, brute-force testleri, bilgi toplama scriptleri için.

**Örnek Komut:**

```bash
nmap --script vuln hedef.com
nmap --script http-brute --script-args userdb=users.txt,passdb=passes.txt hedef.com
```

---

### Parametre: `-A` (Aggressive)

**Açıklama:** `-sV`, `-O`, `--script=default` ve `--traceroute` gibi bir dizi işlemi bir arada çalıştırır. Çok kapsamlı fakat tespit edilme riski yüksektir.

**Kullanım Amacı:** Kapsamlı bilgi toplama ve hızlı keşif (izinli ortamlarda).

**Örnek Komut:**

```bash
nmap -A hedef.com
```

---

### Parametre: `-T <0-5>` (Zamanlama şablonu)

**Açıklama:** Tarama hızını ve agresifliğini belirler. `T0` en yavaş (paranoid), `T5` en hızlı (insan dikkatini çeker). `T4` sık kullanılır ama IDS/IPS tetikleyebilir.

**Kullanım Amacı:** Hız ile tespit riski arasında denge kurmak.

**Örnek Komut:**

```bash
nmap -T4 hedef.com
```

---

### Parametre: `-v`, `-vv` (Verbosity)

**Açıklama:** Çıktı detayını artırır.

**Kullanım Amacı:** Tarama ilerlemesini daha ayrıntılı görmek için.

**Örnek Komut:**

```bash
nmap -vv hedef.com
```

---

### Parametre: `--reason`

**Açıklama:** Nmap'in bir port durumuna (open/closed/filtered) nasıl ulaştığını gösterir.

**Kullanım Amacı:** Sonuçların nedenlerini anlamak için.

**Örnek Komut:**

```bash
nmap -sS --reason hedef.com
```

---

### Parametre: `--open`

**Açıklama:** Çıktıda sadece açık portları gösterir (filtrelenmiş/kapalı olanları gizler).

**Kullanım Amacı:** Sadece işe yarayan (open) portlara odaklanmak için.

**Örnek Komut:**

```bash
nmap -sS --open hedef.com
```

---

### Parametre: `-n` ve `-R`

**Açıklama:** `-n` DNS çözümlemesini kapatır (sadece IP gösterir). `-R` ise her zaman DNS çözümlemesi yapar.

**Kullanım Amacı:** `-n` tarama süresini hızlandırır; `-R` isimleri görmek istediğinde kullanılır.

**Örnek Komut:**

```bash
nmap -n 192.168.1.1
nmap -R 192.168.1.100
```

---

### Parametre: `--traceroute`

**Açıklama:** Traceroute bilgisini ekler ve hedefe giden rotayı gösterir.

**Kullanım Amacı:** Hedefe giden ağ yolunu (hop sayısı) incelemek için.

**Örnek Komut:**

```bash
nmap --traceroute hedef.com
```

---
# E. Çıktı, Hız ve Diğer Faydalı Opsiyonlar (Ek)

---
- `-oN <dosya>` — Normal (okunabilir) çıktı kaydetme.
    
- `-oX <dosya>` — XML çıktı.
    
- `-oG <dosya>` — Grepable çıktı.
    
- `-oA <prefix>` — Hepsini birden (`-oN`, `-oX`, `-oG`) oluşturur.
    

**Örnek:** `nmap -sS -p 1-1000 -oA rapor hedef.com`

---

- Hız / throttle ayarları: `--min-rate <pps>`, `--max-rate <pps>`, `--scan-delay <ms>`, `--max-retries <sayı>`, `--host-timeout <ms>`.
    
- `--defeat-rst-ratelimit` — bazı RST rate-limit senaryolarında yardımcı olabilir.
    
- Kaynak kontrolü: `-g <port>` veya `--source-port <port>` — kaynak port belirleme.
    
- MAC spoof: `--spoof-mac <mac|vendor>` (LAN ortamında).
    
- IP spoofing: `-S <ip>` — çoğu durumda geri dönüş almaz; dikkatli kullan.
    
- IPv6 tarama: `-6`.
    
- `--packet-trace` — gönderilen/alınan paketleri gösterir (debug amaçlı).
    
- `--resume <file>` — durdurulmuş taramayı devam ettirme.
    

---
# F. Örnek Kombine Komutlar

```bash
# Hızlı LAN host keşfi
nmap -sn 192.168.1.0/24 -oN hosts.txt

# Tam SYN tarama, tüm portlar, versiyon tespiti, default scriptler, XML rapor
nmap -sS -p- -sV -sC -oX rapor.xml hedef.com

# Agresif keşif (izinli ortamda)
nmap -A -T4 hedef.com -oN agresif.txt

# Dosyadan hedef oku, belli IP'leri atla, sadece açık portları göster
nmap -iL hedefler.txt --excludefile atla.txt --open -oN sonuc.txt

# NSE örneği: zafiyet taraması
nmap --script vuln -sV -oN vuln.txt hedef.com
```

# G. Kısa Güvenlik ve Etik Uyarı

- **İzin olmadan tarama yapma.** İzin yoksa ağ sahiplerinin politikalarını ihlal edebilirsin ve yasal sonuç doğurabilir.
    
- `-A`, `-sS` gibi agresif taramalar ağ tarafından kolayca tespit edilir; üretim ağlarında dikkatli ol.
    

---

*Made by KejSrynT*