# Лаб 05 — API систем тест (Postman + Newman)

- Нэр: Б. Отгонсүх
- Код: B242270021

    $ node -v
    v24.21.0
    $ newman -v
    6.2.3

## 1. Сонголт ба төлөөлөх утгууд

| Сонголт | Эквивалент анги | Төлөөлөх утга |
|---|---|---|
| studentID | идэвхтэй | status = "active" |
| studentID | идэвхгүй | status = "inactive" |
| studentID | байхгүй | хэзээ ч үүсгээгүй ID |
| courseID | байгаа | PUT /courses/:id-аар үүсгэсэн |
| courseID | байхгүй | хэзээ ч үүсгээгүй ID |
| prerequisites | бүгд үзсэн | ["CS201","CS202"], оюутан хоёуланг нь үзсэн |
| prerequisites | зарим нь | ["CS201","CS202"], оюутан зөвхөн CS201 үзсэн |
| prerequisites | огт үзээгүй | ["CS201"], coursesTaken = [] |
| prerequisites | урьдачгүй (хязгаар) | [], coursesTaken = [] |
| Хүсэлтийн формат | талбар дутуу / буруу JSON | studentID эсвэл courseID орхисон; хаагдаагүй JSON |

Боломжгүй хослол: байхгүй оюутанд coursesTaken гэж байхгүй, байхгүй хичээлд prerequisites гэж байхгүй тул эдгээр сонголтын хослол утгагүй.

## 2. Спецификаци (14 тест)

| ID | Нөхцөл | Хүлээгдэх статус | result |
|---|---|---|---|
| T01 | Happy path: идэвхтэй оюутан, урьдач хангасан | 201 | OK |
| T02 | Олон урьдач бүгд хангагдсан | 201 | OK |
| T03 | Идэвхгүй оюутан | 200 | ERROR_INACTIVE_STUDENT |
| T04 | Байхгүй оюутан | 200 | ERROR_NO_STUDENT |
| T05 | Байхгүй хичээл | 200 | ERROR_NO_COURSE |
| T06 | Урьдач огт үзээгүй | 200 | ERROR_PREREQUISITES (missing=[CS201]) |
| T07 | Урьдач зарим нь дутуу | 200 | ERROR_PREREQUISITES (missing=[CS202]) |
| T08 | Давхар алдаа: идэвхгүй оюутан + байхгүй хичээл | 200 | ERROR_INACTIVE_STUDENT |
| T09 | Давхар алдаа: байхгүй оюутан + байхгүй хичээл | 200 | ERROR_NO_STUDENT |
| T10 | Давхар алдаа: идэвхгүй оюутан + урьдач дутуу | 200 | ERROR_INACTIVE_STUDENT |
| T11 | Хязгаар: урьдачгүй хичээл, хоосон coursesTaken | 201 | OK |
| T12 | Хязгаар: courseID дутуу | 400 | ERROR_BAD_REQUEST |
| T13 | Хязгаар: studentID дутуу | 400 | ERROR_BAD_REQUEST |
| T14 | Хязгаар: буруу JSON | 400 | ERROR_BAD_JSON |

T08-T10-аар тогтоосон дараалал: оюутны шалгалт (байхгүй/идэвхгүй) нь хичээл болон урьдачийн шалгалтаас өмнө хийгддэг.

## 3. Newman ажиллуулалт

| Ажиллуулалт | Файл | assertions executed | assertions failed | exit code |
|---|---|---|---|---|
| PASS | results/newman-pass.txt | 48 | 0 | 0 |
| FAIL | results/newman-fail.txt | 48 | 1 | 1 |
| DOWN | results/newman-down.txt | 48 | 48 | 1 |

- PASS: 14 тест бүгд тэнцсэн тул exit code 0.
- FAIL: lab05-collection-fail.json-д T01-ийн oracle-ийг зориуд 201-ийн оронд 200 болгосон, тиймээс 1 assertion унаж exit code 1 (CI quality gate буруу бүтээлийг зогсооно).
- DOWN: сервер унтраасан үед ECONNREFUSED гарсан. Энэ бол интерфейсийн алдаа (холболт тогтсонгүй), oracle-ийн алдаа биш — хариу ирээгүй тул утгыг шалгах боломжгүй байсан. Exit code 1.
