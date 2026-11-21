## SOAL NO. 1

### Query:

```sql
WITH coba AS (
    SELECT
        s.Name,
        s.Marks,
        g.Grade
    FROM Students s
    LEFT JOIN Grades g
        ON s.Marks BETWEEN g.Min_Mark AND g.Max_Mark
)

SELECT
    CASE
        WHEN Grade < 8 THEN NULL
        ELSE Name
    END AS Name,
    Grade,
    Marks
FROM coba
ORDER BY Grade DESC, Name ASC, Marks ASC;
```

![Hasil pengerjaan soal nomor 1](https://github.com/user-attachments/assets/3f6d6a9a-2262-492b-8f89-2efaf47fe4fe)

Berikut merupakan nilai dari pengerjaan soal No 1.

Pada soal no 1, saya menggunakan CTE terlebih dahulu untuk melakukan JOIN antar tabel Students dan table Grades.

Lalu di luar CTE baru saya select untuk menampilkan kolom apa saja ya dibutuhkan, dan untuk menampilkan kolom 'Nama' Saya menggunakan konsep Case When untuk memnuhi syarat Jika Grade < 8 Maka Nama yang ditampilkan adalah Null.

---

## SOAL NO. 2

### Query:

```sql
WITH coba AS(
    SELECT MIN(LAT_N) AS a,
    MIN(LONG_W) AS b,
    MAX(LAT_N) AS c,
    MAX(LONG_W) AS d
    FROM STATION
)
select
    ROUND(ABS(c-a) + ABS(d-b),4)
    FROM coba;
```

![Hasil pengerjaan soal nomor 2](https://github.com/user-attachments/assets/edbdf0aa-33cc-45a9-ae0b-8f5ca7abadcc)

Berikut merupakan nilai dari pengerjaan soal No 2.

Pada soal no 2, saya menggunakan CTE terlebih dahulu untuk melakukan agregasi menggunakan MIN dan MAX

Lalu di luar CTE baru saya melakukan perhitungan jarak dengan menggunakan fungsi agregasi Absolute berdasarkan nilai yang sudah diambil dari CTE
