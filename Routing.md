# Flutter Routing & Navigation

Tujuan pembelajaran: Mahasiswa memahami dan dapat menggunakan routing untuk navigasi halaman dengan baik.

## Persiapan

- Jika Anda belum mempunyai project Flutter maka Anda dapat membuat project baru dengan menggunakan _Command Pallete_ (Ctrl + Shift + P) di Visual Studio Code lalu pilih perintah: __Flutter: New Project__. Ikuti petunjuk berikutnya sampai selesai dan VSCode membuka file __lib/main.dart__. Hapus semua isi file tersebut dan ubah menjadi seperti berikut:

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Routing & Navigation',
      debugShowCheckedModeBanner: false,
      initialRoute: '/',
      routes: {},
    );
  }
}
```

- Untuk menggunakan Route, di widget MaterialApp ditambahkan property ```initialRoute``` dan ```routes```. Daftar screen dan route yang digunakan ditambahkan di bagian ```routes```.

### Routing Tanpa Parameter

- Selanjutnya kita buat folder ```screen``` di dalam folder ```lib```, dan kemudian ditambahkan file ```first.dart``` di dalamnya. Isi dari file ```first.dart``` adalah sebagai berikut:

```dart
import 'package:flutter/material.dart';

class First extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('First Route'),
      ),
      body: Container(
        constraints: BoxConstraints.expand(),
        padding: EdgeInsets.all(20),
        child: Column(
          mainAxisAlignment: MainAxisAlignment.spaceAround,
          crossAxisAlignment: CrossAxisAlignment.stretch,
          children: <Widget>[
            ElevatedButton(
              onPressed: () {
                Navigator.pushNamed(context, '/second');
              }, 
              child: Text('Pindah Halaman Tanpa Parameter')
            ),
          ],
        ),
      ),
    );
  }
}
```

- Pada file ```first.dart``` tersebut terdapat sebuah tombol yang jika ditekan akan memindahkan aplikasi ke screen ```second```. Untuk menampilkan screen first sebagai default maka ```main.dart``` diubah menjadi:

```dart
...
      routes: {
        '/': (context) => First(),
        '/second': (context) => Second(),
      }
...
```

- Kita buat file baru ```second.dart``` di folder screen dengan isi sebagai berikut:

```dart
import 'package:flutter/material.dart';

class Second extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Second Route'),
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            Navigator.pop(context);
          }, 
          child: Text('Kembali')
        ),
      ),
    );
  }
}
```

