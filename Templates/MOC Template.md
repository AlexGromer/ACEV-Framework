<%* // 1. Сначала выполняем все вычисления (этот блок скроется после создания заметки) 
const currentFolder = tp.file.folder(true);
const files = app.vault.getMarkdownFiles();
let count = 0; 
for (let file of files) { 
if (file.parent && file.parent.path === currentFolder && file.path !== tp.file.path(true)) {
count++; 
} 
} 
let nextNum = count + 1; 
_%>
<%*
// Вычисляем алиас, удаляя префикс "MOC " из названия файла
let cleanTitle = tp.file.title.replace("MOC ", "");
_%>
---
id: <% tp.user.uuid() %>
Группа: Atlas
Порядковый номер в группе: <% nextNum %>
Тема: 
Дата создания: <% tp.date.now("YYYY-MM-DDTHH:mm") %>

---
# <% tp.file.title %>
## Карта темы: <% cleanTitle %>

--- 
### Сводная таблица материалов
```dataview 
TABLE WITHOUT ID
	"Порядковый номер в группе" AS "№",
	id AS "ID",
    Подтема AS "Подтема",
    file.link AS "Название",
    Группа AS "Группа",
    "Дата создания" AS "Дата создания"
WHERE Тема = [[<% cleanTitle %>]] 
AND file.path != this.file.path
SORT "Порядковый номер в группе" ASC 
```

---
### Теги
#MOC