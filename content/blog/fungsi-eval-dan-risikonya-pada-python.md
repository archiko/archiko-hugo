---
Title: Fungsi eval dan risikonya pada python
Date: 2021-09-04
Tags: 
- python
- module
Author: Ardho Ainullah
Description: "Fungsi dari eval ini sendiri untuk memparsing atau merubah sebuah string menjadi sebuah sintaks murni python."
---

```python
eval(expression, globals=None, locals=None)
```

Fungsi dari eval ini sendiri untuk memparsing atau merubah sebuah string menjadi sebuah sintaks murni python.

• paramater dalam fungsi eval()

expression - string<br>
globals (opsional) - dictionary<br>
locals (opsional) - dictionary

nah dalam kasus seperti apa, kita bisa menggunakan fungsi ini? oke

coba lihat contoh kode dibawah ini

```python
a,b = 3,5

#pilihan: + / - *
opsi = input('nilai a sama b ingin diapakan? ')
print (a, opsi, b)
```


output
<pre><code class="plaintext">$ python3 eval.py

nilai a sama b ingin diapakan? +
3 + 5
</code></pre>


Tentu kita tidak bisa bukan, untuk menampilkan hasil dari 3 + 5 tersebut. disinilah kita bisa menggakali nya dengan eval. lihat kode dibawah ini
```python
a,b = 3,5
#pilihan: + / - *
opsi = input('nilai a sama b ingin diapakan? ')

#f: format string
print (eval(f'{a} {opsi} {b}'))
```


output
<pre><code class="plaintext">$ python3 eval.py
nilai a sama b ingin diapakan? +
8
</code></pre>


Atau untuk lebih mudah memahaminya
```python
var = "1+1"
print (var)
print (eval(var))
```

output
<pre><code class="plaintext">$ python3 eval.py
1+1
2
</code></pre>
paham? paham lah masa gaa. jadi intinya dia itu bisa ngerubah sebuah string itu menjadi sintaks murni. 
btw Saya juga pernah implementasikan eval ini ke program simple random quiz math buatan saya, bisa cek <a href="https://pastebin.com/ZepZcygQ" style="color:#80ED99;">disini</a> untuk kodingannya.

<br>
<script type="text/javascript">
	atOptions = {
		'key' : 'e00dc4387e6c63802d3ac0af944c2eb6',
		'format' : 'iframe',
		'height' : 250,
		'width' : 300,
		'params' : {}
	};
	document.write('<scr' + 'ipt type="text/javascript" src="http' + (location.protocol === 'https:' ? 's' : '') + '://www.highperformancedformats.com/e00dc4387e6c63802d3ac0af944c2eb6/invoke.js"></scr' + 'ipt>');
</script>
<br>

**Risk/Risiko**

Tapi fungsi ini kurang bagus untuk digunakan jika tidak dipenuhi dengan validasi input atau penanganan yang mumpuni, jadi saya contohin seperti dibawah ini.

semisal dalam program kita itu ada module os yang sudah diimport, bisa saja user itu memasukan sebuah sintaks python untuk memanipulasi program yang sudah kita buat. 

```python
import os

nama = input('masukan nama: ')
print (eval(nama))
```


output
<pre><code class="plaintext">$ python3 eval.py

masukan nama: os.system("ls ../")
bin   dev  home  media  opt   root  sbin  sys  usr
boot  etc  lib   mnt    proc  run   srv   tmp  var
</pre></code>


tentu sangat berisiko tinggikan? apalagi program kita itu berbasis web seperti menggunakan framework django, dalam form yang kita buat itu menggunakan fungsi eval, tentu akan mudah heker mengexploitasi web kita.




oke mungkin segini dulu, semoga bermanfaat.

