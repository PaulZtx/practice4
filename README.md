# practice4 Гарбузов Павел БСБО-01-20
---
# 1
---
# 2 задание 
1. **Использование устаревшего API MySQL:**
   ```php
   $result = mysqli_query($GLOBALS["___mysqli_ston"], $getid );
   ```
   Использование устаревшего API MySQL без обработки возможных ошибок делает код уязвимым для SQL-инъекций. 
   Рекомендуется использовать подготовленные запросы или хотя бы проверять и очищать входные данные.

2. **Небезопасное использование входных данных:**
   ```php
   $id = $_GET[ 'id' ];
   ```
   Входные данные не проверяются на безопасность перед использованием в запросе SQL. Это может привести к SQL-инъекциям.
   Рекомендуется использовать подготовленные запросы или хотя бы функции, такие как `mysqli_real_escape_string`, для обработки входных данных.

3. **Отсутствие обработки ошибок:**
   ```php
   $result = mysqli_query($GLOBALS["___mysqli_ston"], $getid );
   ```
   Отсутствие проверки результата запроса и обработки ошибок делает код менее устойчивым.
   Рекомендуется добавить проверку на успешное выполнение запроса и обработку ошибок.

4. **Отсутствие использования подготовленных выражений:**
   ```php
   $getid  = "SELECT first_name, last_name FROM users WHERE user_id = '$id';";
   ```
   Использование строковых конкатенаций для создания SQL-запроса может сделать код уязвимым к инъекциям.
   Рекомендуется использовать подготовленные выражения для обеспечения безопасности.

5. **Использование суперглобального массива без проверки:**
   ```php
   $id = $_GET[ 'id' ];
   ```
   Отсутствие проверки наличия параметра 'id' в массиве `$_GET` может привести к ошибкам, если параметр не передан.

6. **Использование устаревшего MySQL-закрытия соединения:**
   ```php
   ((is_null($___mysqli_res = mysqli_close($GLOBALS["___mysqli_ston"]))) ? false : $___mysqli_res);
   ```
   Этот код использует устаревший способ закрытия соединения с MySQL. Вместо этого рекомендуется использовать `mysqli_close($GLOBALS["___mysqli_ston"]);`.

7. **Отсутствие безопасности при выводе:**
   ```php
   $html .= '<pre>User ID exists in the database.</pre>';
   ```
   При выводе сообщений на страницу рекомендуется использовать функции безопасности, такие как `htmlspecialchars`, чтобы избежать возможных атак XSS.
---
# 3 задание 
Исправленый участок кода 
```
<?php
if (isset($_GET['Submit'])) {
    $id = $_GET['id'];

    $conn = new mysqli("localhost", "username", "password", "databaseName");
    // Проверка соединения
    if ($conn->connect_error) {
        die("Connection failed: " . $conn->connect_error);
    }
    // Использование подготовленного запроса для предотвращения SQL-инъекций
    $stmt = $conn->prepare("SELECT first_name, last_name FROM users WHERE user_id = ?");
    $stmt->bind_param("s", $id);
    $stmt->execute();
    $result = $stmt->get_result();

    // Получение результатов
    $num = $result->num_rows;
    if ($num > 0) {
       
        $html .= '<pre>Пользовательский ID существует в БД.</pre>';
    } else {

        // Пользователь не найден
        header($_SERVER['SERVER_PROTOCOL'] . ' 404 Not Found');

        $html .= '<pre>Пользовательский ID не существует в БД.</pre>';
    }

    $stmt->close();
    $conn->close();
}

?>
```
---
# 4 задание 
![image2](https://github.com/PaulZtx/practice4/assets/36164890/91094e58-0894-46f4-b74f-6d69cc9de4a8) 
