---
id: <% tp.user.uuid() %>
Группа: Atlas
Дата создания: <% tp.date.now("YYYY-MM-DDTHH:mm") %>

---
# <% tp.file.title %>

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