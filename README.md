FB:Thanadon Kongkanun [![Click](https://www.img.in.th/images/209839257f2c5439dbdad8509960979d.png)](https://www.facebook.com/markker.mtd/)
===================
นี่เป็นคำสั่งการใช้งาน PHP PDO เบื้องต้น ถ้าเป็นประโยชน์ให้กับใครก็สามารถที่จะแชร์ต่อกันได้นะครับผม

* [การเชื่อมต่อฐานข้อมูล](#ConnectDatabase)

* [แสดงผลข้อมูล](#SelectData)

* [เพิ่มข้อมูลลงฐานข้อมูล](#Insertdata)

* [เรื่องวัน/เวลา](#Date)

* [สุ่มตัวอักษร](#RandomString)

* [คอนจ๊อบ(ตั้งเวลาให้ PHP ทำงานซ้าๆ)](#CronJob)

* [ดึงข้อมูล JsonApi มาแสดงผล)](#SelectJsonApi)

* [การแรมดอมแบบตั้ง %)](#Randompercent)


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
    
//2

    <?php
            foreach ($data as $row) {
            ?>

                <tr>
                    <td><?php echo $row['id']; ?></td>
                    <td><?php echo $row['name']; ?></td>
                    <td><?php echo $row['username']; ?></td>
                    <td>@<?php echo $row['email']; ?></td>
                </tr>

            <?php
            }
            ?>
```

Insertdata
------------
#### เพิ่มข้อมูลลงฐานข้อมูล
```php
$insert = $pdo->prepare('INSERT INTO news (username, password)
            VALUES (:username, :password)');

            $insert->execute([
                'username' => array,
                'password' => array,
            ]);
```

Date
------------
#### เรื่องเวลา
```php 
<?php

//เซ็ตเวลาให้เป็นเวลาของประเทศไทย
date_default_timezone_set('Asia/Bangkok'); 

// ปี-เดือน-วัน
echo date('Y-m-d');
echo date('H:i:s');

?>
```
RandomString
------------
#### สุ่มตัวอักษร
```php 
<?php
function rand_string($length)
    {
        $chars = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789abcdefghijklmnopqrstuvwxyz@#$&*";
        $size = strlen($chars);
        echo "";
        for ($i = 0; $i < $length; $i++) {
            $str = $chars[rand(0, $size - 1)];
            echo $str;
        }
    }
    rand_string(5);
    ?>
```

Cronjob
------------
#### CronJob คือวิธีตั้งเวลาให้ PHP ทำงานซ้ำๆ
```php 
<?php
function httpGet($url)
{
    $ch = curl_init();  
    curl_setopt($ch,CURLOPT_URL,$url);
    curl_setopt($ch,CURLOPT_RETURNTRANSFER,true);
//  curl_setopt($ch,CURLOPT_HEADER, false); 
    $output=curl_exec($ch);
 
    curl_close($ch);
    return $output;
}
while (true) {
  $curl_content = httpGet("กรอกลิ้งที่ต้องการจะคอนจ๊อบลงตรงนี้เลย");
	echo $curl_content;
	echo "Markk <br>";
	sleep(3);
}
?>
```

SelectJsonApi
------------
#### ดึงข้อมูล Json(API) มาแสดงผล
```php 
<?php

 $jsondata = file_get_contents("https://jsonplaceholder.typicode.com/users");
 $data = json_decode($jsondata, true);
 print_r($data);

?>
```

Randompercent
------------
#### การสุ่มแบบตั้ง %
```php
<?php
// define options with chance weight
$choices = [
  'a' => 10.50,
  'b' => 30,
  'c' => 24.50,
  'd' => 35,
];

$total = array_sum( $choices ); // total chance
$percent = rand( 0, $total*100 ) / 100; // random chance with decimal point
$award = null;
$carry = 0;

// loop through each option
foreach ( $choices as $key => $value ) {
  // calculate for floor/ceil value for current option
  $high = $carry + $value;
  $low = $carry;

  // check if randomized $percent inside the threshold
  if ( $percent > $low && $percent <= $high ) {
    $award = $key;
    
    // value inside the threshold, break loop.
    break;
  }

  // value outside the threshold, save next low value and continue.
  $carry += $value;
}

// output
echo "percent: {$percent}\n";
echo "award: {$award}\n";
```


