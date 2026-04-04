---
Группа: Atlas
id: 8860fbc8-47f8-413f-bb19-ac4b55d1e944
Дата создания: 2026-04-02T00:41
---
# MOC Index

--- 
### Сводный индекс MOC
```dataview 
TABLE WITHOUT ID
	"Порядковый номер в группе" AS "№",
	id AS "ID",
    file.link AS "Название",
    "Дата создания" AS "Дата создания"
WHERE file.folder = this.file.folder
AND file.path != this.file.path
SORT "Порядковый номер в группе" ASC 
```

---
### Теги 
#MOC
