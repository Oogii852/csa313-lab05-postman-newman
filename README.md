
## 3. Newman ажиллуулалт

| Ажиллуулалт | Файл | assertions executed | assertions failed | exit code |
|---|---|---|---|---|
| PASS | results/newman-pass.txt | 48 | 0 | 0 |
| FAIL | results/newman-fail.txt | 48 | 1 | 1 |
| DOWN | results/newman-down.txt | 48 | 48 | 1 |

- PASS: 14 тест бүгд тэнцсэн тул exit code 0.
- FAIL: lab05-collection-fail.json-д T01-ийн oracle-ийг зориуд 201-ийн оронд 200 болгосон, тиймээс 1 assertion унаж exit code 1 (CI quality gate буруу бүтээлийг зогсооно).
- DOWN: сервер унтраасан үед ECONNREFUSED гарсан. Энэ бол интерфейсийн алдаа (холболт тогтсонгүй), oracle-ийн алдаа биш — хариу ирээгүй тул утгыг шалгах боломжгүй байсан. Exit code 1.

## 4. Дүгнэлт

(Энд 8-10 өгүүлбэрээр өөрөө бич.)
