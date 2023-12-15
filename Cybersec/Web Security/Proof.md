
# Vulnerable Site


![](https://i.imgur.com/k6yaaQD.png)



![](https://i.imgur.com/7FboBUU.png)


![](https://i.imgur.com/khR26lS.png)



![](https://i.imgur.com/SA0JMH5.png)


![](https://i.imgur.com/CDufh2g.png)




![](https://i.imgur.com/nBJDtXT.png)

![](https://i.imgur.com/Rphy3Al.jpg)




![](https://i.imgur.com/g4gz7c7.jpg)

![](https://i.imgur.com/xzVEhhm.png)


![](https://i.imgur.com/xYBttVL.png)

```php
if ($email && $password1) {

    $sql = "SELECT * FROM credential WHERE email = '$email'";

    $result = mysqli_query($conn, $sql);

  

    if (mysqli_num_rows($result) > 0) {

        $row = mysqli_fetch_assoc($result);

  

        // Email exists, now check the password

        if (password_verify($password1, $row['password'])) {

            // Password is correct, log the user in

            session_start();

            $_SESSION['username'] = $row['username'];

            $_SESSION['email'] = $row['email'];

            $_SESSION['password'] = $row['password'];

            header('Location: index.php');

            exit();

        } else {

            // Password is wrong, display an error message

            $errors['password'] = "Wrong password";

            // echo "Entered pass: $password1<br>";

            // echo "Stored: {$row['password']}<br>";

  

            var_dump("entered pass:$password1");

            var_dump("entered pass:{$row['password']}");

            var_dump($result);

            // var_dump($_SESSION);

        }

    } else {

        // Email doesn't exist, display an error message

        $errors['email'] = "Email doesn't exist";

    }

}

  
  

}
```


```html
<?php

//A random Code for the request to the API

$random_code = '200311';

//Random Code can be anything static, or you can generate it through //built-in random functions

//The below url accepts AppName, AppInfo & SecretCode

$cURLConnection = curl_init('https://www.authenticatorapi.com/pair.aspx?AppName=YourAppName&AppInfo=YourAppInfo&SecretCode='.$random_code);

//Setting Options for the cURL Request

curl_setopt($cURLConnection, CURLOPT_RETURNTRANSFER, true);

//Execute the Request

$apiResponse = curl_exec($cURLConnection);

//Close

curl_close($cURLConnection);

//Displaying the QR Code

echo $apiResponse;

?>

<form method="post" action="index.php">

    <div class="row">

      <div class="col-lg-4"></div>

<div class="form-group col-lg-4" style="float:center">

  <input type="text" class="form-control" min="4" name="pin" max="6" required/>

  <br><br>

  <button name="submit-pin" type="submit" class="btn btn-primary">Enter</button>

</div>

<div class="col-lg-4"></div>

    </div>

    </form>
```


---
# Secure Site

## doesnt trigger alert

![](https://i.imgur.com/1yW38LE.png)

## attempt

![](https://i.imgur.com/iDHGgTY.png)


![](https://i.imgur.com/vjpD7Ya.png)


## sanitization of feedback

![](https://i.imgur.com/zRMOBsu.png)

# Features


## Login page

![](https://i.imgur.com/Iw6i2p4.png)

## sign up

![](https://i.imgur.com/kmJrnwn.png)


## nav bar

![](https://i.imgur.com/lsVzONf.png)


## cars

![](https://i.imgur.com/mrprbUa.png)

### Registration

![](https://i.imgur.com/aZySMuM.png)


### new

![](https://i.imgur.com/Km8Nz13.jpg)



### used

![](https://i.imgur.com/brzakW6.jpg)



### rental

![](https://i.imgur.com/nyvucyQ.png)




## Search

![](https://i.imgur.com/s1LPwg7.png)


![](https://i.imgur.com/ZyLsckG.png)


## Profile panel

![](https://i.imgur.com/Bq7KPJN.png)

### if admin

![](https://i.imgur.com/8qrlJLK.png)



## Feedback section

![](https://i.imgur.com/CKa39ld.png)


## Account 

![](https://i.imgur.com/jPc3Zrx.png)


## Admin dashboard

![](https://i.imgur.com/Qw3ipVc.png)


![](https://i.imgur.com/4rD1zsg.png)












# Not Secure

- Why not add 2FA in secure? **because after trying to secure 2FA it ruined the website's functionality hence kept it as a vulnerable feature only**


---
[2FA](https://medium.com/@murtazaahmad/implementing-google-authenticator-two-factor-auth-in-php-a604d858efd7)