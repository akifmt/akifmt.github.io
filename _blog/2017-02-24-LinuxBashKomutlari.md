---
layout: single-tr
title:  "Linux Bash Komutları"
excerpt: "Linux Bash Komutları"
header:
  teaser: "blogimages/blog10_bash.png"
categories: 
  - blog-tr
tags:
  - bash
---

![bash](/images/blogimages/blog10_bash.png "bash")<br>


- **Aktif klasör:**
```
pwd
```

- **Görünüm:**

```bash
ls
'Detaylı Görünüm Parametreleri:
-l *detaylı*
-lrt *detaylı*
-d *klasör*
-r *read*
-w *write*
-x *execute*'
```

- **Aktif Klasör Değiştirme:**
```
cd <klasör adı> 
```

- **Dosya Olusturma:**
```
touch <dosya adı> 
```

- **Ekrana Çıktı:**

```bash
ctrl+k	'sağa doğru siler'

echo "hello world"
echo "hello coders" > dosyaadi		'Ciktiyi dosyaya gonder'
echo $?			'Son işlemin sonucunu verir (0 başarılı)'
cat dosyaadi		'dosya içeriğini ekrana basar'
more dosyaadi		'dosya içeriğini ekrana basar'
less dosyaadi		'dosya içeriğini ekrana basar'
nano dosyaadi		'nano text editor ile açar'
vim dosyaadi		'vim text editor ile açar'
head -n 5 output-1.txt		'ilk 5 satırı göster'
tail -n 5 output-1.txt		'son 5 satırı göster'
tail -f log-file.txt		'son satırları yazdırır ve yeni gelen satırlatı takip eder'
```

- **Klasör Oluşturma:**
```
mkdir klasoradi		'Klasör oluşturur'
```

- **İçerik Göndererek veya Alarak Çalıştırma:**

```bash
./prog < input-1.txt		'input-1 içeriğini prog a göndererek çalıştırır.'
./my-prog < input-1.txt > output-1.txt		'çıktıları output a atar'
./my-prog 12 2> output-1.txt		'stderr den gelenleri output a yönlendirir'
./my-prog < input-1.txt > all-output.txt 2>&1		'stderr, stdoutput a yönlendir'

echo "asdasd" >> log-file.txt  'dosya sonuna ekler'
```

- **Derleme:**
```
gcc -o my-prog my-prog.c			'gcc ile derler.'
```

- **Process'de Arama:**
```
ps aux | grep my-prog		'çalışan process ler arasında arama yapma'
```

- **Process İşlemleri:**

```bash
'Tüm prosesler bunlarla başlar: 0:stdinput 1:stdoutput 2:stderror'
'Prosesslerin tutulduğu sistem proc klasörüdür.'
'
ctrl+c : interrupt (sigint)
		   (sigterm)
		   (sighup) -> konfigurasyonu update eder
		   (sigkill) -> sorgusuz öldürür
ctrl+l : clear terminal işlemi yapar
ctrl+d : (end of stream) çalışan aktif process i öldürür
'
kill -l			'kill için sinyaller listesi gösterir'
kill -INT 12334		'sigint sinyali gönderir'
kill -TERM 12334	'sigterm sinyali ile öldürür'

cat cmdline		'caliştığı konum'
cat environ		'process in enviroment veriable ları'
cat limits		'process limitleri'

top		'anlık olarak işlemleri gösterir'
htop		'güncelleyerek gösterir'
```

- **Path İşlemleri:**

```bash
echo $PATH		'path içeriği'
export PATH=$PATH:/home/vm/deneme		'path in sonuna yeni dizin ekleme'
```

- **Device İşlemleri:**

```bash
ls /dev		'tüm device ları gösterir'
ls -lrt /dev/null	'çıktıları null a gönderir'
```

- **Veri Transferi:**

```bash
curl http://akifmt.github.io  	'indirir'
curl http://akifmt.github.io > /dev/null  	'indirir sonuçları null e gönderir'
curl -s http://akifmt.github.io > /dev/null	'silence modda indirir sonuçları null e gönderir'
curl http://akifmt.github.io > /dev/null 2>&1	'stderr den gelenleri de yönlendir'

curl -s https://demo.consul.io/v1/catalog/services	'silence modda indirir'
curl -s https://demo.consul.io/v1/catalog/service/web?pretty	'silence modda indirir'
curl -s https://demo.consul.io/v1/catalog/service/web?pretty | grep Address	'silence modda indirir Address satırlarını bulur'
curl -s https://demo.consul.io/v1/catalog/service/web?pretty | jq '.[0].Address'	'Address kısmının ilkini alır'
curl -s https://demo.consul.io/v1/catalog/service/web?pretty | jq -r '.[].Address'	'Tüm Address kısımlarının tümünü alır'
curl -s https://demo.consul.io/v1/catalog/service/web?pretty | jq -r '.[].Address' | while read serverAddr; do curl -s $serverAddr > $serverAddr.txt; done
'Address satırlarının tümünü bulur sağa gönderir, Address adlarına göre dosya oluşturur'
```

- **Dosya İşlemleri:**

```bash
echo "asd,qwe,qwwww" | cut -d, -f2		', den sonra 2. kelimeyi alır'
echo "asd;qwe;qwwww" | cut -d\; -f2		'; den sonra 2. kelimeyi alır'
echo "asd,qwe,qwwww" | awk -F, '{print $2}'		', den sonra 2. kelimeyi alır'
ls -lrt | awk '{print $9}'		'9. sütunları yazdır'
ls -lrt | cat		'pipe çıktıyı sağdakine verir'
wc -l		'line sayar'
ls -lrt | wc -l	'ls den gelen sonucları sağa gönderir satır sayar'
find . -name my-prog	'ismi my-prog ile başlayanları bulur'
find . -type f	'dosyaları bulur'
find . -name "*.txt"	'uzantısı txt olanı bulur'
find -name "*.txt" | while read filename; do echo $filename; done	'txt bulur saüa gönderir filename olarak yazdırır'
find -name "*.txt" | while read filename; do rm $filename; done	'bulduğu dosyaları siler'
dd if=/dev/zero of=zero-file.txt bs=512 count=2	'2x512KB boş dosya oluşturur. '
hexdump zero-file.txt 'hex olarak ve tekrar eden kısımları kısaltarak gösterir.'
hexdump -v zero-file.txt	'hepsini gösterir kısaltmasız olarak gösterir'
dd if=/dev/urandom of=random-file.txt bs=512 count=4	'rastgele değerli 4x512KB dosya oluşturur'
```

- **İşlemci Üzerinde İşlemler:**

```bash
dd if=/dev/zero of=null 'sıfır üreterek null aygıtına gönderir (kontrolsüz işlemci yüklemesi)'
stress-ng -c 1 -l 40	'1 CPU üzerinde %40 yükleme yapar'
stress-ng -c 0 -l 40	'tüm CPUlarde %40 yükleme yapar'
```

- **Ağ Üzerinde İşlemler:**

```bash
ifconfig	'ağ cihazlarını ve bağlantıları gösterir'
route -n	'tüm erişimleri gösterir'
sudo wondershaper ens33 512 512 '512Kpbs indirme ve gönderme sınırlaması yapar'
sudo wondershaper ens33 clear	'limitleri kaldırır'
```

- **Satır Sonları (lineending):**

| Linux  | LF (line feed)   |  (\n) |
| Unix  |  CR (carriage return)| (\r)   |
| Windows  | CRLF  (carriage return line feed) |   |
