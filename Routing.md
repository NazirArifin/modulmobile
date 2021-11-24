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

## Routing Tanpa Parameter

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

- Sekarang coba tekan tombol __Pindah Halaman Tanpa Parameter__ di screen first, jika semua benar maka aplikasi akan pindah ke screen second. Selanjutnya di screen second jika ditekan __Kembali__ akan kembali ke halaman awal.

## Routing dengan Parameter

- Adakalanya kita ingin mengirimkan data ke halaman tujuan, sebelumnya kita buat Class yang menampung data yang akan dikirimkan. Buat file __ScreenArgument.dart__ di folder ```class``` (buat foldernya lebih dahulu) dengan isi sebagai berikut:

```dart
class ScreenArgument {
  final String title;
  final String message;

  ScreenArgument(this.title, this.message);
}
```

- Selanjutnya buat file ```third.dart``` di folder __screen__ dengan isi sebagai berikut:

```dart
class Third extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final ScreenArgument args = ModalRoute.of(context)!.settings.arguments as ScreenArgument;
    
    return Scaffold(
      appBar: AppBar(
        title: Text(args.title),
      ),
      body: Center(
        child: Text(args.message),
      )
    );
  }
}
```

- Kita lihat bahwa untuk untuk menangkap data yang dikirim oleh halaman sebelumnya digunakan ```ModalRoute.of(context)!.settings.arguments as ScreenArgument``` yang dimasukkan ke variabel ```args```. Data yang ditangkap ditampilkan di AppBar dan Body.

- Sekarang kita modifikasi file ```main.dart``` seperti berikut:
```dart
...
      routes: {
        '/': (context) => First(),
        '/second': (context) => Second(),
        '/third': (context) => Third(),
      },
...
```

- Dan modifikasi file ```first.dart``` menjadi seperti berikut:
```dart
...
            ElevatedButton(
              onPressed: () {
                Navigator.pushNamed(context, '/third', arguments: ScreenArgument('Ini judulnya', 'ini pesan dari page 1'));
              }, 
              child: Text('Pindah Halaman Dengan Parameter')
            ),
...
```

- Sekarang coba tekan tombol __Pindah Halaman Dengan Parameter__ di screen first, jika semua benar maka aplikasi akan pindah ke screen third. Selanjutnya di screen third muncul teks sesuai dengan yang dikirim oleh screen sebelumnya.

## Mendapatkan Data dari Screen Tujuan

- Selanjutnya untuk beberapa kasus kita perlu menunggu data dari screen tujuan. Kita akan membuat file baru ```forth.dart``` di folder ```screen``` dengan isi sebagai berikut:

```dart
import 'package:flutter/material.dart';

class Forth extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Pilih Ya atau Tidak'),
      ),
      body: Center(
        child: Column(
          children: <Widget>[
            Padding(
              padding: EdgeInsets.all(8),
              child: ElevatedButton(
                onPressed: () {
                  Navigator.pop(context, 'Ya!');
                },
                child: Text('Ya'),
              ),
            ),
            Padding(
              padding: EdgeInsets.all(8),
              child: ElevatedButton(
                onPressed: () {
                  Navigator.pop(context, 'Tidak!');
                },
                child: Text('Tidak'),
              ),
            )
          ],
        )
      ),
    );
  }
}
```

- Selanjutnya kita modifikasi file ```main.dart``` menjadi seperti berikut:

```dart
      routes: {
        '/': (context) => First(),
        '/second': (context) => Second(),
        '/third': (context) => Third(),
        '/forth': (context) => Forth(),
      },
```

- Kita modifikasi file ```first.dart``` dengan menambah tombol dalam ```Column``` dan fungsi seperti berikut:

```dart
...
          ElevatedButton(
              onPressed: () {
                _navigateShowAndWaitSelection(context);
              }, 
              child: Text('Pilih Ya atau Tidak!')
            )
          ],
        ),
      ),
    );
  }

  Future<void> _navigateShowAndWaitSelection(BuildContext context) async {
    final result = await Navigator.pushNamed(context, '/forth');
    ScaffoldMessenger.of(context)
      ..removeCurrentSnackBar()
      ..showSnackBar(SnackBar(content: Text('$result')));
  }
}
```

- Jika semua berjalan dengan baik, waktu tombol ```Pilih Ya atau Tidak!``` maka muncul screen forth yang berisi pilihan. Ketika pilihan diklik maka muncul pesan dari pilihan tersebut di aplikasi

## Tugas

- Gabungkan screen di materi __The Love Calculator__ dengan Route dan Navigator

