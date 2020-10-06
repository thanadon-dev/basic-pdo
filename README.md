FB:Thanadon Kongkanun [![Click](https://www.img.in.th/images/209839257f2c5439dbdad8509960979d.png)](https://www.facebook.com/markker.mtd/)
===================
นี่เป็นคำสั่งการใช้งาน PHP PDO เบื้องต้น ถ้าเป็นประโยชน์ให้กับใครก็สามารถที่จะแชร์ต่อกันได้นะครับผม

* [การเชื่อมต่อฐานข้อมูล](#ConnectDatabase)

* [แสดงผลข้อมูล](#SelectData)

* [เพิ่มข้อมูล มาเร็วๆนี้](#AddData)

* [ลบข้อมูล มาเร็วๆนี้](#DeleteData)

ConnectDatabase 
------------
#### (การเชื่อมต่อฐานข้อมูล)
```php 
<?php
  // การเชื่อมต่อฐานข้อมูล
 define('DB_SERVER','localhost');
 define('DB_USER','root');
 define('DB_PASS','');
 define('DB_NAME','Thanadon');

try {
  $pdo = new PDO("mysql:host=".DB_SERVER.";dbname=".DB_NAME, DB_USER, DB_PASS);
  // เช็คการเชื่อมต่อฐานข้อมูล
  $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
  echo "การเชื่อมต่อฐานข้อมูลสำเร็จ" ;
} catch(PDOException $e) {
  echo "การเชื่อมต่อฐานข้อมูลผิดพลาด กรุณาลองใหม่อีกครั้ง: " . $e->getMessage();
}

?>
```

SelectData 
------------
#### (การแสดงผลข้อมูล)
```php 
<?php
    $sql = $pdo->prepare("SELECT * FROM news");
    $sql->execute();
    $result = $sql->fetchAll();
    foreach ($result as $row) {
    ?>

        <div class="col-md-4 p-3">
            <div class="card">
                <img src="<?php echo $row['images']; ?>" height="250" class="card-img-top" alt="">
                <div class="card-body">
                    <h2><?php echo $row['title']; ?></h2>
                    <p class="card-text"><?php echo $row['detail']; ?></p>
                </div>
            </div>
        </div>

    <?php
    }
    ?>
```
