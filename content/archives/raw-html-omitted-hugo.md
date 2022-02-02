---
Title: Render tag HTML dalam file markdown hugo
Date: 2021-10-08
Tags:
- ssg
- hugo
categories:
- web development
Author: Hiko
Thumbnail: "/images/htmlhugo.jpg"
Description: "karena saya males buat benerin satu persatu postingannya saya coba searching dan ternyata dihugo support untuk menambahkan tag-tag html dalam markdownnya. terdapat dua cara untuk menggunakan tag html dalam file markdown dihugo"
---

{{<raw>}}
<img src="/images/htmlhugo.jpg" class="img-fluid bg-dark">
<br>
{{</raw>}}
belum lama ini saya mulai migrasi dari static site `pelican` ke `hugo`. terdapat banyak alasan kenapa saya pindah ke hugo yaps terlalu banyak kendala saat menggunakan pelican jadi saya putusin buat migrasi aja.

ketika saya melakukan pemindahan bertahap dari peli ke hugo ternyata beberapa file postingan markdown saya itu menggunakan tag html yang mana semuanya jadi berantakan karena hugo gabisa nge'embed html di file markdownnya.

karena saya males buat benerin satu persatu postingannya saya coba searching dan ternyata dihugo support untuk menambahkan tag-tag html dalam markdownnya.

terdapat dua cara untuk menggunakan tag html dalam file markdown dihugo
1. Menambahkan opsi unsafe di konfigurasi goldmark renderer
2. Menambahkan shortcodes rawhtml
<br>
### **Cara pertama**
Cara ini cukup mudah kalian bisa masukan beberapa baris kode dibawah ini kedalam `config.toml` kalian.
```toml
[markup.goldmark.renderer]
  unsafe = true
```

kita sudah bisa memberikan tag html di postingan markdown kita
```plaintext
<p style="color:red">damn</p>
```
**output:**
<p style="color:red">damn</p>



### **Cara Kedua**
untuk cara kedua buatlah sebuah folder `layouts/shortcodes/` diproject hugo kalian, lalu kalian tambahkan satu file dengan nama `rawhtml.html` didalam folder tersebut.

masukan satu baris kode ini kedalam `rawhtml.html`
```
{{.Inner}}
```

untuk memanggil shortcodenya kalian bisa gunakan seperti ini didalam file markdownnya:
```bash
{\{\< rawhtml >}}
<p style="color:red">Raw Html</p>
{\{\< /rawhtml >}}
```

**info**: Hilangkan `\` karena kalo ga saya tambahin itu nanti bakalan muncul outputnya disini wkwk

**output:**
<p style="color:red">Raw Html</p>



