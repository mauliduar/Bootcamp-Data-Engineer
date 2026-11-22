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

## SOAL NO. 3

### Query:

```sql
WITH nilai_tinggi AS(
    select d.difficulty_level, 
    d.score as max_score,
    c.challenge_id
    from difficulty d 
    LEFT JOIN challenges c 
    ON d.difficulty_level = c.difficulty_level
    
),
full_score AS(
    select 
    s.hacker_id,
    count(*) as count_max_score
    from submissions s
    INNER JOIN nilai_tinggi n
    ON n.challenge_id = s.challenge_id 
    WHERE s.score = n.max_score
    GROUP BY s.hacker_id
    HAVING count(*) > 1
    ),
nama AS(
    select h.name,
    f.hacker_id,
    f.count_max_score
    from hackers h 
    INNER JOIN full_score f
    ON h.hacker_id = f.hacker_id   
)
select hacker_id, name from nama
ORDER BY count_max_score DESC, hacker_id ASC;

```

![Hasil pengerjaan soal nomor 3](https://github.com/user-attachments/assets/d46fbedf-627a-4686-a040-881020d3c535)


Berikut merupakan nilai dari pengerjaan soal No 3.

Pada soal no 3, saya membuat 3 CTE

CTE yang pertama (nilai_tinggi) berfungsi untuk JOIN table difficulty dengan table challenges untuk mendapatkan nilai max score <br>
CTE yang kedua (full_score) berfungsi untuk membentuk logic hitung jumlah submissions, mengambil hacker_id yang memiliki score = max_score dan memiliki submisiion lebih dari 1 dan JOIN table submissions dengan table hasil CTE pertama (nilai_tinggi)<br>
CTE yang ketiga (nama) berfungsi untuk menampilkan nama hacker dan hacker id 

## SOAL NO. 4

### Query:

```sql
WITH gabung as (
    select
    w.id,
    wp.age,
    w.coins_needed,
    w.power,
    MIN(w.coins_needed) OVER (
        PARTITION BY age, power
    ) as coins_min
    from wands w 
    Inner Join wands_property wp 
    on w.code = wp.code 
    where is_evil = 0
)


select id,age,coins_needed,power
from gabung 
where coins_needed = coins_min
order by power desc, age desc;
```

![Hasil pengerjaan soal nomor 4](https://github.com/user-attachments/assets/62b1705f-4840-440c-bb6f-3d5ef37698bd)


Berikut merupakan nilai dari pengerjaan soal No 4.

Pada soal no 4
Saya membuat CTE yang berfungsi untuk menampilkan id, age, coins_needed, power, dan juga menghitung biaya minimum coin dengan filter umur serta power yang menggunakan window function, lalu tidak lupa untuk men JOIN kan kedua table yg ada yaitu table Wands dan Wands_Property.<br>
Pada akhir, akan melakukan pemanggilan mpada tabel hasil CTE(gabung) dengan menggunakan WHERE kondisi coins_needed = coins_min dan diurutkan bedasarkan power terbesar dan umur tertinggi.
