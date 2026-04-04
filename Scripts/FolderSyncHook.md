<%*
if (!window.folderSyncHookRegistered) {
    app.vault.on('rename', async (file, oldPath) => {
        // 1. Базовые проверки: только md-файлы
        if (file.extension !== 'md' || !file.parent) return;

        // 2. Вычисляем старую и новую директории
        const newFolderPath = file.parent.path;
        const oldFolderPath = oldPath.includes('/') ? oldPath.substring(0, oldPath.lastIndexOf('/')) : '/';

        // 3. ЗАЩИТА: Если папка не изменилась (это просто переименование файла),
        // мы останавливаем скрипт, чтобы не сбить текущий порядковый номер!
        if (newFolderPath === oldFolderPath) return;

        // --- БЛОК ИНДЕКСАЦИИ ---
        let maxNumber = 0;
        
        // Получаем все md-файлы в новой папке (исключая сам наш файл)
        const siblings = file.parent.children.filter(f => f.extension === 'md' && f.path !== file.path);

        // Ищем самый большой "Порядковый номер" среди соседей
        for (const sibling of siblings) {
            // Обращаемся к кэшу Обсидиана, чтобы не открывать файлы физически (работает мгновенно)
            const cache = app.metadataCache.getFileCache(sibling);
            const num = cache?.frontmatter?.['Порядковый номер'];
            
            if (typeof num === 'number' && num > maxNumber) {
                maxNumber = num;
            }
        }
        
        const nextNumber = maxNumber + 1; // Наш новый номер

        // --- БЛОК ЗАПИСИ ---
        await app.fileManager.processFrontMatter(file, (fm) => {
            fm['Группа'] = newFolderPath;
            fm['Порядковый номер'] = nextNumber;
        });
    });
    
    window.folderSyncHookRegistered = true;
    console.log("System: FolderSyncHook v3 (Group + Sequence indexing) registered.");
}
%>