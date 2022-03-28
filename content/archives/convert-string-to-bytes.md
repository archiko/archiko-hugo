---
Title: merubah string python menjadi bytes dan sebaliknya
Date: 2022-03-28
Tags:
- python
- string
- bytes
Categories:
- programming
Author: 0xHynz
---

merubah string dipython menjadi sebuah bytes itu cukup mudah, dibeberapa case terkadang
kita diharuskan untuk memberi sebuah nilai kembalian dengan tipe data bytes. contohnya
saat ingin mengirim sebuah pesan tcp disocket kita diharuskan mengirim dengan tipe data bytes.


#### string to bytes

dalam case ini kita bisa menggunakan fungsi `bytes()` yang ada dipython

```
text = "hello world"
textBytes = bytes(text,"utf-8")
print (textBytes,'\n')
print ("Type: ",type(textBytes))

```

maka outputnya akan seperti ini

```
b'hello world'

Type:  <class 'bytes'>
```
sedangkan untuk opsi `utf-8` atau 8-bit Unicode Transformation Format (UTF-8) 
adalah konvensi kode untuk pengkodean berbagai karakter. <a href="https://id.theastrologypage.com/8-bit-unicode-transformation-format">baca lebih..</a>

#### bytes to string
sebaliknya jika ingin merubah bytes menjadi sebuah string kalian hanya cukup menganti
fungsi `bytes()` menjadi `str()`.

```
bytesText = b'hello world'
text = str(bytesText,'utf-8')
print (text,'\n')
print ("Type: ",type(text))
```

maka outputnya akan seperti ini

```
hello world

Type:  <class 'str'>
```
