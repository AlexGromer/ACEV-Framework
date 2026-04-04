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
---
id: <% tp.user.uuid() %>
Группа: <% tp.file.folder() %>
Порядковый номер в группе: <% nextNum %>
Дата создания: <% tp.date.now("YYYY-MM-DDTHH:mm") %>
Тема: 
Подтема: 
Источники: []

---
# <% tp.file.title %>
### Контекст 🧠
<% tp.file.cursor(1) %>

---
### Формат 
📝 Структурированная заметка с пояснениями

---
### Контент 

---
### Краткие выводы

--- 
### Связи 🔗
**Ручные связи**

**Связанные записи по теме:** 
```dataview 
TABLE WITHOUT ID
	"Порядковый номер в группе" AS "№",
	id AS "ID",
    Подтема AS "Подтема",
    file.link AS "Название",
    Группа AS "Группа",
    "Дата создания" AS "Дата создания"
FROM "Vaults" OR "Cards" OR "Archive"
WHERE Тема = this.Тема 
AND Тема != null
AND Подтема = this.Подтема
AND Подтема != null 
AND file.path != this.file.path 
```

---
### Теги
#Vaults/