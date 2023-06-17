# PostgreSQL

Anahtar Kelimeler: PostgreSQL
Created time: June 15, 2023 1:37 PM
Etiket: Komutlar
ID: 5
Konu: Veri Tabanı
Status: In Progress
Status 2: Test

# Listeleme Komutları

SELECT customer_id, first_name, last_name FROM customer; | customer tablosunda ki id, first-name ve last-name bilgilerini getirir.

SELECT title, length / 60 FROM film | film tablosun da ki title sütunu listeler ve length sütunun da ki süreleri 60’a bölerek kaç saat olduklarını yazar.

### Tablo Birleştirme Komutu

SELECT first_name | | ' ' | | last_name | | ' - ' | | title  FROM actor, film | actor ve film tablolarından verileri first_name, last_name ve title sütunlarını alıp birleştirir. 

SELECT first_name | | ‘ ’ | | last_name, maas, maas* 1.35 FROM calisanlar | çalışanlar tablosunda ki insanların first_name ve last_name birleştirir. Maaşların normal hali ve %35 zam almış halini de gösterir.

### Alias Komutu

SELECT first_name | | ' ' | | last_name as Calisanlar, salary as Eskimaas, salary * 1.35 as Zamlımaas
FROM employees | Aliaslar ile getirdiğimiz sütunların isimlerini değiştirebiliriz. Böylece daha okunabilir.

### Distinct Komutu

SELECT DISTINCT rating, title FROM film |  Birden fazla sütun ile kullanılabiliriz aynı değerleri birden fazla göstermez.

SELECT DISTINCT ON (rating) rating, title FROM film | Bu komut ile rating sütununa göre her bir rating eş gelen ilk title değerini getir.

Açıklamak gerekirse; Film rating sütunu ve ftitle sütunu’nu DISTINCT komutu ile getirdiğimde tekrar eden her şey gelir. Fakat DISTINCT ON (rating) kullanırsam her rating değerinden bir film gelir. Bu genelde birden fazla sütun ile işiniz olduğunda işe yarayan bir şey. Sadece rating sütunu için olsaydı DISTINCT komutu tek başına yeterli olurdu.

![Untitled](PostgreSQL%208457806d366a4e529662521cd4dc4137/Untitled.png)

![Untitled](PostgreSQL%208457806d366a4e529662521cd4dc4137/Untitled%201.png)

## **ORDER BY Komutu**

SELECT title FROM film ORDER BY title DESC | Title tablosunda yer alan isimleri alfabetik bir şekilde sıralar. Desk ifadesi sayesinde bu sıralamayı tersten yapmanızı sağlar.

SELECT rating, length, title, rental_rate FROM film ORDER BY rating, length DESC; | Burada iki tane koşula göre sıralama yapıyoruz length tersine göre alırken, rating alfabetik bir şekilde sıralayacak. 

Ayrıca sütun isimleri yerine, sütun numaraları da kullanılabilirsin “ ORDER BY 1, 2 DESC “ şeklinde. Tabi bunu kendiniz saymanız gerekir SELET kısmında verdiğiniz sütunlar listesine göre yerleri değişir ve rakam olarak verdiğiniz ifadeyi değiştirmeniz gerekir.

### **Boş Sütunları Listeleme Komutu (NULLS)**

SELECT * FROM employees ORDER BY phone_number NULLS LAST | phone_number sütunun da ki  boş değerleri en altta gösterir.

SELECT * FROM employees ORDER BY phone_number NULLS FIRST | phone_number sütunun da ki  boş değerleri en üstte gösterir.

# Filtreleme Komutları

SELECT * FROM actor WHERE first_name = '**Tom**' | first_name Tom olan aktörleri listeler.

SELECT * FROM order WHERE type != 'online' | Ödeme yöntemi online dışında olanları listeler

SELECT * FROM payment WHERE price >= 50 | Fiyatı 50 ve üstü olanları listeler.

# Ekleme Komutları

-

# Silme Komutları

-

# Değiştirme Komutları

-

## Escape Karakter

SELECT E’I\’m a sql’; veya SELECT ‘I’’m a sql’;

SELECT $$’I’m a sql’$$;

## Mantıksal İfadeler ve Operatörler

| Operator | Açıklama |
| --- | --- |
| + | Toplama |
| – | Çıkarma |
| * | Çarpma |
| / | Bölme |
| % | Mod Alma |
| = | Eşittir |
| !=, <> | Eşit değildir. |
| < | Küçüktür |
| <= | Küçük veya eşittir |
| > | Büyüktür |
| >= | Büyüktür veya eşittir |

| Operatör | Açıklama |
| --- | --- |
| IN | Liste içerisinde ki değerlerden her hangi bir şartı karşılayanlar gelsin. |
| LIKE | Metin Karşılaştırma Operatörü |
| AND | Seçilecek satır için koşullardan en az biri doğru olmalıdır. |
| OR | Bir satırın seçilmesi için belirtilen tüm koşullar doğru olmalıdır. |
| NOT | Bir satırın seçilebilmesi için belirtilen koşulun yanlış olması gerekir. |
| BETWEEN | Belirli bir aralıktaki değerleri seçer. Değerler sayı, metin veya tarihler olabilir. |