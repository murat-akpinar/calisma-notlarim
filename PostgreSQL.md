# PostgreSQL

Anahtar Kelimeler: PostgreSQL
Created time: June 15, 2023 1:37 PM
Etiket: Komutlar
ID: 5
Konu: Veri Tabanı
Status: In Progress
Status 2: Test

## Listeleme Komutları

```sql
SELECT customer_id, first_name, last_name FROM customer; 
--customer tablosunda ki id, first-name ve last-name bilgilerini getirir.
```

```sql
SELECT title, length / 60 FROM film
--film tablosun da ki title sütunu listeler ve length sütunun da ki süreleri 60’a bölerek kaç saat olduklarını yazar.
```

## Tablo Birleştirme Komutu

```sql
SELECT first_name || ' ' || last_name || ' - ' || title  FROM actor, film
--actor ve film tablolarından verileri first_name, last_name ve title sütunlarını alıp birleştirir. 
```

```sql
SELECT first_name || ' ' || last_name, maas, maas* 1.35 FROM calisanlar
--çalışanlar tablosunda ki insanların first_name ve last_name birleştirir. Maaşların normal hali ve %35 zam almış halini de gösterir.
```

# Filtreleme Komutları

```sql
SELECT * FROM actor WHERE first_name = '**Tom**'
--first_name Tom olan aktörleri listeler.
```

```sql
SELECT * FROM order WHERE type != 'online'
--Ödeme yöntemi "online" olmayanları listeler
```

```sql
SELECT * FROM payment WHERE price >= 50
--Fiyatı 50 ve üstü olanları listeler.
```

### Alias Komutu

```sql
SELECT first_name || ' ' || last_name as Calisanlar, salary as Eskimaas, salary * 1.35 as Zamlımaas
FROM employees
--Aliaslar ile getirdiğimiz sütunların isimlerini değiştirebiliriz. Böylece daha okunabilir.
```

### DISTINCT Komutu

```sql
SELECT DISTINCT rating, title FROM film
--Birden fazla sütun ile kullanılabiliriz aynı değerleri birden fazla göstermez.
```

```sql
SELECT DISTINCT ON (rating) rating, title FROM film
--Bu komut ile rating sütununa göre her bir rating eş gelen ilk title değerini getir.
```

Açıklamak gerekirse; Film rating sütunu ve ftitle sütunu’nu **DISTINCT** komutu ile getirdiğimde tekrar eden her şey gelir. Fakat **DISTINCT ON** (rating) kullanırsam her rating değerinden bir film gelir. Bu genelde birden fazla sütun ile işiniz olduğunda işe yarayan bir şey. Sadece rating sütunu için olsaydı **DISTINCT** komutu tek başına yeterli olurdu.

![PostgreSQL](https://github.com/magwyen/calisma-notlarim/blob/main/img/Untitled.png) ![](https://github.com/magwyen/calisma-notlarim/blob/main/img/Untitled%201.png)

## **ORDER BY Komutu**

```sql
SELECT title FROM film ORDER BY title DESC
--Title tablosunda yer alan isimleri alfabetik bir şekilde sıralar. Desk ifadesi sayesinde bu sıralamayı tersten yapmanızı sağlar.
```

```sql
SELECT rating, length, title, rental_rate FROM film ORDER BY rating, length DESC;
--Burada iki tane koşula göre sıralama yapıyoruz length tersine göre alırken, rating alfabetik bir şekilde sıralayacak. 
```

Ayrıca sütun isimleri yerine, sütun numaraları da kullanılabilirsin “ **ORDER BY** 1, 2 DESC “ şeklinde. Tabi bunu kendiniz saymanız gerekir SELET kısmında verdiğiniz sütunlar listesine göre yerleri değişir ve rakam olarak verdiğiniz ifadeyi değiştirmeniz gerekir.

## **Boş Sütunları Listeleme Komutu (NULLS)**

```sql
SELECT * FROM employees ORDER BY phone_number NULLS LAST
--phone_number sütunun da ki  boş değerleri en altta gösterir.
```

```sql
SELECT * FROM employees ORDER BY phone_number NULLS FIRST
--phone_number sütunun da ki  boş değerleri en üstte gösterir.
```

# Mantıksal Operatörler

## AND ve OR Komutları

```sql
SELECT * FROM address WHERE district = 'Adana' AND city_id = 5;
--AND operatörün de her iki koşul da karşılanmalıdır.
```

```sql
SELECT * FROM payment WHERE staff_id = 2 OR amount > 5;
--OR operatörün de bir koşul yerine gelmesi yeterlidir. staff_id 1 olsa bile amount değeri 5’den büyükse o satırları getirecektir.
```

## NOT Komutu

```sql
SELECT * FROM calisanlar WHERE city_name NOT IN ('Ankara')
--Ankara dışında olan çalışanları getirir.
--NOT komutu değilini, olmayanları alır.
```

## BETWEEN Komutu

```sql
SELECT * FROM film WHERE time BETWEEN 100 AND 120
--Süresi 100 ve 120 arasında olan filmleri listeler. Eğer time başına NOT operatörünü eklersen bunların dışında kalanları getirir.
```

```sql
SELECT * FROM employees WHERE hire_date BETWEEN '2017-01-01' AND '2017-12-31'
--İki tarih arasında işe başlayan çalışanları getirir.
```

```sql
SELECT * FROM staff WHERE '2022-05-01' BETWEEN hire_date AND departure_date
--2022-05-01 tarihinde çalışanları listeler. 
--Where ile bu tarihi belirtip hire_date ve departure_date sütunlarını bu tarihe uyan değerleri getirir.
```

# Arama Komutları

## IN Komutu

```sql
SELECT * FROM customer WHERE first_name IN ('Leslie', 'Kelly', 'Tracy')
--first_name sütununda ifade edilen isimleri getirir.
```

```sql
SELECT * FROM customer WHERE city_id IN (6, 78, 1) 
--city_id sütununda ifade edilen şehirleri getirir.
```

## LIKE Komutu

**%** işareti ile ifadenin başına veya sonunda ekleyerek 0 veya daha fazla karakter sayısını ifade eder.

**_** işareti ile ifadenin başına veya sonuna ekleyerek istediğimiz karakter sayısı kadar olması lazım. Örnek olarak “For_” bu ifadede en fazla 4 karakterlik olan her şeyi getirir. Bu ifadeyi aramak istediğimiz kelime uzunluğuna göre artırabiliyoruz.

Küçük, büyük harfe duyarlıdır. PostgreSQL özel “ILIKE” komutu vardır. Bu konutla arama yaparsanız küçük, büyük harf ayırt etmez

```sql
SELECT * FROM customer WHERE first_name LIKE '%n’
--first_name kısmın da "n" ile biten isimleri getirir.
```

```sql
SELECT * FROM customer WHERE first_name LIKE '%u_'
--first_name kısmında sondan bir önce ki harfi "u" olanları getirir.
```

```sql
SELECT * FROM customer WHERE first_name LIKE '_her%'
--first_name kısmında ilk 3 karakter kısmı ayrılmış. 
--ilk harf her şey olabilir fakat ona 1 karakterlik yer ayrılmış.
--Ondan sonra gelen 3 harf "her" olacak
--Ondan sonra ki kısım 0 veya daha fazla karaktere sahip olabilir.
```

# Ekleme Komutları

-

# Silme Komutları

-

# Değiştirme Komutları

-

## Escape Karakter

SELECT E’I\’m a sql’; veya 

SELECT ‘I’’m a sql’;

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
