# Web SQL

Аналог IndexedDB, асинхронное API которое позволяет хранить данные на стороне клиента в виде таблиц и строк. Необходимо отметить, с 18 ноября 2010 года W3C официально прекратил поддержку этой технологии. Размер базы данных ограничен 5МВ.

Основные этапы работы с документами:

1\) Открытие базы данных в ".js"-файле. Данный код создаёт объект, представляющий БД, а если базы данных с таким именем не существует, то создаётся и она. При этом в аргументах указывается имя базы данных, версия, отображаемое имя и приблизительный размер. Кроме того важно отметить, что приблизительный размер не является ограничением. Реальный размер базы данных может изменяться.:

```text
	db = openDatabase("ToDo", "0.1", "A list of to do items.", 200000);
	db = openDatabase("Название базы данных", "Версия", "отображаемое имя", размер в байтах);
```

также возможно два исхода открытия БД:

```text
db.onError();
db.onSuccess();
```

Успешность подключения к БД можно оценить, проверив объект db на null:

```text
if(!db){alert("Failed to connect to database.");
```

2\) Выполнение запросов к данным: Для выполнения запросов к БД предварительно надо создать транзакцию, вызвав функцию **database.transaction\(\)**. У неё один аргумент, а именно **другая JavaScript функция**, принимающая объект транзакции и предпринимающая запросы к базе данных. Существует два типа transaction - чтение + письмо \(**transaction\(\)**\) и только чтение **readtransaction\(\)**. Собственно сам SQL запрос можно выполнить, вызвав функцию _**executeSql**_ объекта транзакции. Она принимает 4 аргумента:

* строка SQL запроса
* массив параметров запроса \(параметры подставляются на место вопросительных знаков в SQL запросе\)
* функция, вызываемая при успешном выполнении запроса
* функция, вызываемая в случае возникновения ошибки выполнения запроса

```text
db.transaction(function(tx) {
	tx.executeSql("SELECT COUNT(*) FROM (имя базы), [], function(result){}, function(tx, error){});
})
```

3\) Вставка данных:

```text
db.transaction(function(tx) {
    tx.executeSql("INSERT INTO ToDo (label, timestamp) values(?, ?)", ["Купить iPad или HP Slate", new Date().getTime()], null, null);
});
```

4\) Работа с результатами запросов. Результат выполнения запроса на выборку данных содержит набор строк, а каждая строка содержит значения столбцов таблицы для данной строки. Вы можете получить доступ к какой-либо строке результата вызвав функцию result.rows.item\(i\), где i – индекс строки. Далее, для получения требуемого значения, нужно обратиться к конкретному столбцу по имени – result.rows.item\(i\)\[ «label»\].

```text
db.transaction(function(tx) {
		tx.executeSql("SELECT * FROM ToDo", [], function(tx, result) {
			for(var i = 0; i < result.rows.length; i++) {
		document.write('<b>' + result.rows.item(i)['label'] + '</b><br />');
	}}, null)});
```



```text
db.deleteTodo = function(id) {
  		var db = html5rocks.webdb.db;
  		db.transaction(function(tx){
    	    tx.executeSql("DELETE FROM todo WHERE ID=?", [id],
            html5rocks.webdb.onSuccess,
          html5rocks.webdb.onError);
    });
}
```


