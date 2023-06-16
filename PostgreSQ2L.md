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

SELECT first_name || ' ' || last_name as Calisanlar, salary as Eskimaas, salary * 1.35 as Zamlımaas
FROM employees | Aliaslar ile getirdiğimiz sütunların isimlerini değiştirebiliriz. Böylece daha okunabilir.

### **ORDER BY Komutu**

SELECT title FROM film ORDER BY title DESC | Title tablosunda yer alan isimleri alfabetik bir şekilde sıralar. Desk ifadesi sayesinde bu sıralamayı tersten yapmanızı sağlar.

SELECT rating, length, title, rental_rate FROM film
ORDER BY rating, length DESC; | Burada iki tane koşula göre sıralama yapıyoruz length tersine göre alırken, rating alfabetik bir şekilde sıralayacak. 

Ayrıca sütun isimleri yerine, sütun numaraları da kullanılabilirsin “ ORDER BY 1, 2 DESC “ şeklinde. Tabi bunu kendiniz saymanız gerekir SELET kısmında verdiğiniz sütunlar listesine göre yerleri değişir ve rakam olarak verdiğiniz ifadeyi değiştirmeniz gerekir.

# Ekleme Komutları

-

# Silme Komutları

-

# Değiştirme Komutları

-

## Escape Karakter

SELECT E’I\’m a sql’;
SELECT ‘I’’m a sql’;


## Mantıksal İfadeler ve Operatörler

| Operator | Açıklama |
| --- | --- |
| + | Toplama |
| – | Çıkarma |
| * | Çarpma |
| / | Bölme |
| % | Mod Alma |
| ^ | Üssü ifade soldan sağa doğru |
| |/ | Karekök |
| ||/ | Küp kök |
| ! | Faktoriyel |
| @ | Mutlak değerini alma |
| = | Eşittir |
| != | Eşit değildir. |

| Operatör | Açıklama |
| --- | --- |
| < | Küçüktür |
| <= | Küçük veya eşittir |
| <> | Eşit değildir? |
| = | Eşittir |
| > | Büyüktür |
| > = | Büyüktür veya eşittir |
| || | birleştirme |
| !! = | Eşit değildir. |
| ~~ | LIKE |
| ! ~~ | NOT LIKE |
| ~ | Eşleştirme (regex), büyük / küçük harfe duyarlı |
| ~ * | Eşleşme (regex), büyük / küçük harfe duyarlı değil |
| ! ~ | Eşleşmez (regex), büyük / küçük harfe duyarlı |
| ! ~ * | (Regex) ile eşleşmez, büyük / küçük harfe duyarlı değildir |
