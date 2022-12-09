<p align = "center">МИНИСТЕРСТВО НАУКИ И ВЫСШЕГО ОБРАЗОВАНИЯ<br>
РОССИЙСКОЙ ФЕДЕРАЦИИ<br>
ФЕДЕРАЛЬНОЕ ГОСУДАРСТВЕННОЕ БЮДЖЕТНОЕ<br>
ОБРАЗОВАТЕЛЬНОЕ УЧРЕЖДЕНИЕ ВЫСШЕГО ОБРАЗОВАНИЯ<br>
«САХАЛИНСКИЙ ГОСУДАРСТВЕННЫЙ УНИВЕРСИТЕТ»</p>
<br><br><br><br><br><br>
<p align = "center">Институт естественных наук и техносферной безопасности<br>Кафедра информатики<br>Коньков Никита Алексеевич</p>
<br><br><br>
<p align = "center">Лабораторная работа №9<br><b>«Разработка серверных скриптов»</b><br>01.03.02 Прикладная математика и информатика</p>
<br><br><br><br><br><br><br><br><br><br><br><br>
<p align = "right">Научный руководитель<br>
Соболев Евгений Игоревич</p>
<br><br><br>
<p align = "center">г. Южно-Сахалинск<br>2022 г.</p>
<br><br><br><br><br><br><br><br><br><br><br><br>

<h1 align = "center">Введение</h1>

<p> <b>HTML</b> (или же HyperText Markup Language) - стандартизированный язык разметки документов для просмотра веб-страниц в браузере. Веб-браузеры получают HTML документ от сервера по протоколам HTTP/HTTPS или открывают с локального диска, далее интерпретируют код в интерфейс, который будет отображаться на экране монитора. </p>

<p><b>PHP</b> (произносится пи-эйч-пи́) — скриптовый язык программирования, созданный для генерации HTML-страниц на веб-сервере и работы с базами данных. На сегодняшний момент поддерживается подавляющим большинством представителей хостингов. Входит в «LAMP» — «стандартный» набор для создания веб-сайтов.</p>

<h1 align = "center">Цели и задачи</h1>

<p>1. Создать базу данных «university» в программе-дизайнере MySQL Workbench.</p>

<p>2.В базе данных «university» создать таблицу «students» с полями:id тип int – ключ (PK), счетчик (AI);name тип varchar, ненулевое (NN);d_id тип int </p>

<p>3. Заполнить таблицу «students» произвольными записями (вкладка Inserts) - 5 строк (поле id следует заполнять нулями).</p>

<p>4. Сохранить созданную в программе-дизайнере схему базы данных на локальный компьютер.</p>

<p>5. Запустить генерацию базы данных на сервере MySQL (Пункт меню: Database->Forward Engineer. В опциях необходимо поставить галки против пунктов: DROP Objects Before Each CREATE Object и Generate INSERT Statements for Tables).</p>

<p>6. Подключиться к базе данных MySQL (команда mysql –u root –p).</p>

<p>7. Активизировать базу данных «university» (команда use).</p>

<p>8. Выполнить SQL команду: SELECT * FROM students; результаты записать в отчет.</p>

<p>9. Выполнить SQL команды:</p>
<p>UPDATE students SET name = ‘Ivan’ WHERE id = 2;</p>
<p>SELECT * FROM students WHERE id = 2; результаты записать в отчет.</p>

<p>10. Выполнить SQL команды:</p>
<p>DELETE FROM students WHERE id = 2;</p>
<p>SELECT * FROM students; результаты записать в отчет.</p>

<p>11. Проанализировать полученные результаты.</p>

<p>12. К базовой функциональности, следует добавить следующие возможности:</p>
<li>
<lo>К сообщению пользователь может добавить картинку или текстовый файл</lo>
<lo>Изображение должно быть не более 320х240 пикселей, при попытке залить изображение большего размера, картинка должна быть пропорционально уменьшена до заданных размеров, допустимые форматы файлов: JPG, GIF, PNG</lo>
<lo>Текстовый файл не должен быть больше чем 100кб, формат TXT</lo>
<lo>Просмотр файлов должен сопровождаться визуальными эффектами (для примера можно посмотреть http://www.huddletogether.com/projects/lightbox/ )</lo>
</li>

<h1 align = "center">Решение</h1>  

```php
<?php
	$servername = "localhost";
	$username = "root";
	$password = "";
	$dbname = "university";

	$conn = new mysqli($servername, $username, $password, $dbname);
	if ($conn->connect_error) {
		die("Ошибка в подключении: " . $conn->connect_error);
	  }	 
	
	function showtab($a,$b,$c,$d)
	{
		$connect = new mysqli($a,$b,$c,$d);
		$sql = "SELECT * FROM students";
		if($result = $connect->query($sql))
		{
		echo "<table border='1' style='text-align: center; font-size: 25px; width: 500px;'><tr><th>Id</th><th>Name</th><th>D_id</th></tr>";
		foreach($result as $row){
			echo "<tr>";
				echo "<td>" . $row["id"] . "</td>";
				echo "<td>" . $row["name"] . "</td>";
				echo "<td>" . $row["d_id"] . "</td>";
			echo "</tr>";
		}
		echo "</table>";
		}
	}

	showtab($servername, $username, $password, $dbname);
  
	echo "<br><br>";

	$sql = "SELECT * FROM students";
	$result = $conn->query($sql);
	if ($result->num_rows > 0) {
		while($row = $result->fetch_assoc()) {
		  echo "id: " . $row["id"]. ", name: " . $row["name"]. " d_id: " . $row["d_id"] . "<br>";
		}
	} else {
		echo "Никого не найдено";
	}

	echo "<br><br>";

	$sql = "UPDATE students SET name = 'Ivan' WHERE id=2";
	$conn->query($sql);
	if($conn->affected_rows == 0)
	{
		echo "Не найдено подходящих элементов";
	}
	else
	{
		showtab($servername, $username, $password, $dbname);
	}

	echo "<br><br>";

	$sql = "DELETE FROM students WHERE id=2";
	$conn->query($sql);
	if($conn->affected_rows == 0)
	{
		echo "Не найдено подходящих элементов";
	}
	else
	{
		showtab($servername, $username, $password, $dbname);
	}

	echo "<br><br>";

	$sql = "SELECT * FROM students";
	$result = $conn->query($sql);
	if ($result->num_rows > 0) {
		while($row = $result->fetch_assoc()) {
		  echo "id: " . $row["id"]. ", name: " . $row["name"]. " d_id: " . $row["d_id"] . "<br>";
		}
	} else {
		echo "Никого не найдено";
	}
	$conn->close();
?>
```

<h1 align = "center">Телефонная книга</h1>

<h2 align = "center">index.html</h2>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        form {
            text-align:center;
            margin:auto;
        }
        input {
            height:2vw;
        }
        input {
            font-size:1vw;
            width: 15vw;
            border-radius:5px;
        }
    </style>
</head>
<body>
<form action="AddToTab.php" method="post">
    <input type="submit" value="Добавить">
</form>
<form action="ShowTab.php" method="post">
    <input type="submit" value="Посмотреть">
</form>
</body>
</html>
```

<h2 align = "center">get.php</h2>

```php
<script src='https://www.google.com/recaptcha/api.js'></script>

<?php


if (isset($_POST['g-recaptcha-response']) && $_POST['g-recaptcha-response']) {
	$secret = '6LcO8UwjAAAAAHaDkjmyyWfsjuHifM51cCARAC4B';
	$ip = $_SERVER['REMOTE_ADDR'];
	$response = $_POST['g-recaptcha-response'];
	$rsp = file_get_contents("https://www.google.com/recaptcha/api/siteverify?secret=$secret&response=$response&remoteip=$ip");
	$arr = json_decode($rsp, TRUE);
	if ($arr['success']) {
        $db = "lab91011"; 
        $host = "localhost"; 
        $user = "root"; 
        $pass = ""; 
        $conn = new mysqli($host, $user, $pass, $db);
        if ($conn->connect_error) {
	        die("Ошибка в подключении: " . $conn->connect_error);
        }

        $userFile = null;

        function getExtension($x) {
            $res = explode(".", $x);
            return $res[count($res) - 1];
        }


        $extArray = array("txt", "png", "jpeg", "gif");

        $extension = getExtension(basename($_FILES['fl']['name']));
        

        if(!in_array($extension, $extArray)){ //Нужно доделать

        } else {
            if ($extension == "txt") {
                if (filesize($_FILES['fl']['tmp_name']) <= 102400) {//нужно доделать
                    $index = rand(1000, 9999);
                    $uploaddir = 'files/';
                    $fileName = basename($_FILES['fl']['name']);
                    
                    if($fileName != ""){
                        $userFile = $uploaddir . $index . "-" . basename($_FILES['fl']['name']);
                
                        if(!move_uploaded_file($_FILES['fl']['tmp_name'], $userFile)){
                            
                        }
                    }
                }
            } else {
                $index = rand(1000, 9999);
                $uploaddir = 'files/';
                $fileName = basename($_FILES['fl']['name']);
                
                if($fileName != ""){
                    $userFile = $uploaddir . $index . "-" . basename($_FILES['fl']['name']);
            
                    if(!move_uploaded_file($_FILES['fl']['tmp_name'], $userFile)){
                        
                    }
                }
            }
        }

        $userName = $_POST['name'];
        $userURL = $_POST['homepage'];
        $userText = $_POST['text'];
        $userEmail = $_POST['email'];
        $userBrowser = $_SERVER['HTTP_USER_AGENT'];
        $userIp = $_SERVER['REMOTE_ADDR'];

        if(isset($userName) && isset($userText) && isset($userEmail) && !empty($userName) && !empty($userText) && !empty($userEmail)){
            $userName = htmlspecialchars(trim($userName));
            $userEmail = htmlspecialchars(trim($userEmail));
            $userText = htmlspecialchars(trim($userText));
            $userURL = htmlspecialchars(trim($userURL));

            $res = mysqli_query($conn, "INSERT INTO guests (name, email, homepage, text, browser, ip, fl) VALUES 
            ('$userName', '$userEmail', '$userURL', '$userText', '$userBrowser', '$userIp', '$userFile')");
            
            if($res == true) { 
                echo "<meta http-equiv='Refresh' content='0; URL=index.php'>"; 
            } else { 
                echo "Ошибка";
            }
        } else {
            echo "<meta http-equiv='Refresh' content='0; URL=index.php'>";
        }

        $conn->close();
    } else {
        echo "<meta http-equiv='Refresh' content='0; URL=index.php'>";
        }
} else {
    echo "<meta http-equiv='Refresh' content='0; URL=index.php'>";
    }
?>
```

<h2 align = "center">ShowTab.php</h2>

```php
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        form {
            display: flex;
            align-items: center;
            justify-content: center;
        }
        .inp {
            font-size:2vw;
            width: 8vw;
        }
        .yes {
            font-size:2vw;
            max-width: 15px;
        }
		.a {
			width: 20px;
		}
    </style>
    <link rel="stylesheet" href="css/lightbox.css" type="text/css" media="screen" />
    <script type="text/javascript" src="js/prototype.js"></script>
    <script type="text/javascript" src="js/scriptaculous.js?load=effects,builder"></script>
    <script type="text/javascript" src="js/lightbox.js"></script>
</head>
<body>
<?php
try {
    $conn = new PDO("mysql:host=localhost;dbname=lab91011", "root", "");
    $conn->setAttribute(PDO::ATTR_EMULATE_PREPARES,false);

    $page = 1;

    if (isset($_GET['page'])) {
        $page = filter_var($_GET['page'], FILTER_SANITIZE_NUMBER_INT);
    }

    $personCount = 25;

    $sqlcount = "select count(*) as tR from guests";
    $res = $conn->prepare($sqlcount);
    $res->execute();
    $row = $res->fetch();
    $tR = $row['tR'];
    $pages = ceil($tR / $personCount);

    $offset = ($page-1) * $personCount;
  
    $sql = "select * from guests limit :offset, :personCount";
  
    $res = $conn->prepare($sql);
  
    $res -> execute(['offset'=>$offset, 'personCount'=>$personCount]);
  
    echo "<table border='1' style='margin: auto; text-align: center; font-size: 1vw; width: 100vw;'><tr><th style='width: 3vw;'>Id</th>
    <th style='width: 10vw;'>Name</th><th style='width: 17vw;'>Email</th><th style='width: 15vw;'>Homepage</th><th style='width: 20vw;'>Text</th>
    <th style='width: 10vw;'>Ip</th><th style='width: 15vw;'>Browser</th><th style='width: 10vw;'>FileImage</th></tr>";
  
    while (($row = $res -> fetch(PDO::FETCH_ASSOC)) !== false) {
        echo "<tr>";
        echo "<td style='max-width: 3vw;'>" . $row["id"] . "</td>";
        echo "<td style='max-width: 10vw; word-wrap: break-word;'>" . $row["name"] . "</td>";
        echo "<td style='max-width: 17vw; word-wrap: break-word;'>" . $row["email"] . "</td>";
        echo "<td style='max-width: 15vw; word-wrap: break-word;'>" . $row["homepage"] . "</td>";
        echo "<td style='max-width: 20vw; word-wrap: break-word;'>" . $row["text"] . "</td>";
        echo "<td style='max-width: 10vw; word-wrap: break-word;'>" . $row["ip"] . "</td>";
        echo "<td style='max-width: 15vw; word-wrap: break-word;'>" . $row["browser"] . "</td>";
        $path = $row['fl'];
        $arr = explode(".", $path);

        if(count($arr) > 1){
            $ext = $arr[count($arr) - 1];

            if($ext == "txt"){
                echo "<td style='max-width: 77vw; word-wrap: break-word;'><a href=\"" . $path . "\" target=\"_blank\" class=\"btn btn-primary btn-lg active\" role=\"button\">TXT</td>";
            } else {
                echo "<td style='max-width: 77vw; word-wrap: break-word;'><a href=\"" . $path . "\" rel=\"lightbox\"><img src=\"" . $path . "\" width=\"160\" height=\"120\"></a></td>";
            }

        } else {
            echo "<td style='max-width: 77vw; word-wrap: break-word;'>Отсутствует</td>";
        }
        echo "</tr>";
    }
  
    echo "</table>";

    echo "<br><table border='1' align='center' style='width: 30vw;text-align: center;'>";

    echo "<tr>";

    if ($page - 1 >= 1) {
        echo "<td class='yes'><a href=".$_SERVER['PHP_SELF']."?page=".($page - 1).">Предыдущая</a></td>";
    }

    if ($page + 1 <= $pages) {
        echo "<td class='yes'><a href=".$_SERVER['PHP_SELF']."?page=".($page + 1).">Следующая</a></td>";
    }

    echo "</tr>";

    echo "</table>";

} catch(PDOException $e) {
    echo $e->getMessage();
}
?>
<br>
<form action="index.php" method="get" >
<input class="inp" type="submit" name="back" value="Назад">
</form>
</body>
</html>
```

<h2 align = "center">AddToTab.php</h2>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <script src='https://www.google.com/recaptcha/api.js'></script>
    <style>
        form {
            align-items:center;
        }
        input {
            height:2vw;
        }
        input, textarea {
            font-size:1vw;
            width: 15vw;
            border-radius:5px;
        }
        .parent {
            width: 100%;
            height: 100%;
            position: absolute;
            top: 0;
            left: 0;
            overflow: auto;
        }
        .child {
            position: absolute;
            top: 50%;
            left: 50%;
            margin: -125px 0 0 -125px;
            text-align:center;
        }
        
    </style>
<?php
$db = "lab91011"; 
$host = "localhost"; 
$user = "root"; 
$pass = ""; 
$conn = new mysqli($host, $user, $pass); 
if ($conn->connect_error) {
	die("Ошибка в подключении: " . $conn->connect_error);
}

?>
<form enctype="multipart/form-data" action="get.php" method="post">
<div class="parent">
    <div class="child">
        <label>Имя</label><br>
        <input type="text" name="name" size="45"><br>
        <label>Почта</label><br>
        <input type="email" name="email" size="45"><br>
        <label>Сайт(Необязательно)</label><br>
        <input type="url" name="homepage" size="45"><br>
        <label>Текст</label><br>
        <textarea name="text"></textarea><br>
        <input type="file" name="fl" accept=".jpeg, .png, .gif, .txt"><br>
        <div style="width: 15vw; text-align: center;"> 
        <label>Допустимые форматы</label><br>
        <label>.jpeg, .png, .gif, .txt(Не более 100Кб)</label><br>
        </div>
        <div class="g-recaptcha" data-sitekey="6LcO8UwjAAAAAGRaOkB5t2VulDxiFokCabyqF8YI"></div>
        <div class="text-danger" id="recaptchaError"></div>
        <input type="submit">
    </div>
    
</div>
</form>
</body>
</html>
```

<h1 align = "center">Вывод</h1>
<p>Опираясь на материал с сайта w3school, помощь различных руководств, Я выполнил лабораторную работу и научился некоторым функциям в mySQL (на php).</p>