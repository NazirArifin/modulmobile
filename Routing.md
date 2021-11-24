# Flutter Routing & Navigation

Tujuan pembelajaran: Mahasiswa memahami dan dapat menggunakan routing untuk navigasi halaman dengan baik.

## Persiapan

- Jika Anda belum mempunyai project Flutter maka Anda dapat membuat project baru dengan menggunakan _Command Pallete_ (Ctrl + Shift + P) di Visual Studio Code lalu pilih perintah: __Flutter: New Project__. Ikuti petunjuk berikutnya sampai selesai dan VSCode membuka file __lib/main.dart__.

* Buat folder baru dalam folder __lib__ dengan nama __screen__ dan kemudian buat file baru dengan nama file __list.dart__. Setelah itu buat kerangka dokumen sederhana sebagai berikut:

```dart
import 'package:flutter/material.dart';

class ListScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Container(),
    );
  }
}
```

* Ubah file __lib/main.dart__ dengan isi sebagai berikut:

```dart
import 'package:flutter/material.dart';
import 'package:newapp/screen/list.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Routing',
      initialRoute: '/',
      routes: {
        '/': (context) => ListScreen()
      },
    );
  }
}
```

* Pada kode diatas kita buat Routing pada Widget MateriaApp dengan mendaftar beragam route yang akan digunakan dalam __routes__, serta route awal mana yang diload dalam __initialRoute__ yang berisi __ListScreen()__.
* Kita akan menambahkan beberapa bagian di file __lib/screen/list__ menjadi seperti berikut:

```dart

```

