# Flutter Stateful Widget

Tujuan pembelajaran: Mahasiswa memahami dan dapat menggunakan Stateful Widget di Flutter untuk membuat interaksi halaman dengan baik

## Persiapan

- Jika Anda belum mempunyai project Flutter maka Anda dapat membuat project baru dengan menggunakan _Command Pallete_ (Ctrl + Shift + P) di Visual Studio Code lalu pilih perintah: __Flutter: New Project__. Ikuti petunjuk berikutnya sampai selesai dan VSCode membuka file __lib/main.dart__.
- Kita akan buat layout aplikasi sederhana untuk mengubah-ubah warna background dan ukuran sebuah widget. Ketikkan kode berikut di file __lib/main.dart__:

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        primarySwatch: Colors.green
      ),
      home: Scaffold(
        appBar: AppBar(
          title: Text('Stateful Widget'),
        ),
        body: Column(
          children: <Widget>[
            Container(
              height: 70,
              child: Row(
                children: <Widget>[
                  Container(
                    child: Expanded(
                      child: Center(
                        child: Text('1', style: TextStyle(fontSize: 23)),
                      ),
                    ),
                  ),
                  Container(
                    width: 120,
                    height: 70,
                    child: Column(
                      mainAxisAlignment: MainAxisAlignment.center,
                      crossAxisAlignment: CrossAxisAlignment.center,
                      children: <Widget>[
                        RaisedButton(
                          child: Text('Angka +1'),
                          onPressed: () {},
                          color: Colors.yellow,
                        )
                      ],
                    ),
                  )
                ],
              ),
            ),
            Container(
              height: 140,
              child: Row(
                children: <Widget>[
                  Expanded(
                    child: AnimatedContainer(
                      margin: EdgeInsets.all(10),
                      duration: Duration(milliseconds: 200),
                      color: Colors.green
                    ),
                  ),
                  Container(
                    width: 120,
                    height: 140,
                    child: Column(
                      mainAxisAlignment: MainAxisAlignment.center,
                      children: <Widget>[
                        RaisedButton(
                          child: Text('Ubah Warna'),
                          color: Colors.blue,
                          textColor: Colors.white,
                          onPressed: () {},
                        )
                      ],
                    ),
                  )
                ],
              ),
            ),
            Container(
              height: 200,
              child: Row(
                children: <Widget>[
                  Expanded(
                    child: Center(
                      child: AnimatedContainer(
                        duration: Duration(milliseconds: 700),
                        width: 40, height: 40,
                        color: Colors.red,
                      ),
                    )
                  ),
                  Container(
                    width: 120, height: 200,
                    child: Column(
                      mainAxisAlignment: MainAxisAlignment.center,
                      children: <Widget>[
                        RaisedButton(
                          color: Colors.orange,
                          textColor: Colors.white,
                          onPressed: () {},
                          child: Text('Ubah Ukuran'),
                        )
                      ],
                    ),
                  )
                ],
              )
            )
          ],
        ),
      )
    );
  }
}
```

* Terdapat dua baris dengan masing-masing widget yang akan dimanipulasi dan tombol yang diklik akan mengubah state. Kita akan _extract_ masing-masing baris dan buat widget sendiri sendiri dengan nama: __UbahAngka__, __UbahWarna__ dan __UbahUkuran__.
* Lalu ubah masing-masing widget hasil ekstrak tadi menjadi Stateful Widget
* Ketikkan kode berikut di widget __UbahAngka__:

```dart
class UbahAngka extends StatefulWidget {
  const UbahAngka({
    Key key,
  }) : super(key: key);

  @override
  _UbahAngkaState createState() => _UbahAngkaState();
}

class _UbahAngkaState extends State<UbahAngka> {
  int _angka = 0; // buat variabel angka

  @override
  Widget build(BuildContext context) {
    return Container(
      height: 70,
      child: Row(
        children: <Widget>[
          Container(
            child: Expanded(
              child: Center(
                // angka sekarang berasal dari state dan dinamis
                child: Text('$_angka', style: TextStyle(fontSize: 23)),
              ),
            ),
          ),
          Container(
            width: 120,
            height: 70,
            child: Column(
              mainAxisAlignment: MainAxisAlignment.center,
              crossAxisAlignment: CrossAxisAlignment.center,
              children: <Widget>[
                RaisedButton(
                  child: Text('Angka +1'),
                  onPressed: () {
                    // setState untuk mengubah angka
                    setState(() => _angka = _angka + 1);
                  },
                  color: Colors.yellow,
                )
              ],
            ),
          )
        ],
      ),
    );
  }
}
```

* Yang harus Anda perhatikan adalah fungsi __setState__ dimana fungsi ini akan memberi tahu Flutter bahwa kita akan mengubah suatu nilai dari variabel dan Flutter akan merender ulang User Interface nya jika ada perubahan. Pastikan Anda tidak lupa memanggil fungsi __setState__ ini saat mengubah nila suatu variabel yang akan mengubah tampilan.
* Ketikkan kode berikut di widget __UbahWarna__:

```dart
class UbahWarna extends StatefulWidget {
  const UbahWarna({
    Key key,
  }) : super(key: key);

  @override
  _UbahWarnaState createState() => _UbahWarnaState();
}

class _UbahWarnaState extends State<UbahWarna> {
  final Random _random = Random();
  Color _color = Color(0xffffff00);

  // fungsi ubah warna
  void _gantiBackground() {
    setState(() {
      _color = Color.fromARGB(
        255, 
        _random.nextInt(256), 
        _random.nextInt(256), 
        _random.nextInt(256)
      );  
    });
  }
  
  @override
  void initState() {
    super.initState();
    _gantiBackground();
  }

  @override
  Widget build(BuildContext context) {
    return Container(
      height: 140,
      child: Row(
        children: <Widget>[
          Expanded(
            child: AnimatedContainer(
              margin: EdgeInsets.all(10),
              duration: Duration(milliseconds: 200),
              color: _color
            ),
          ),
          Container(
            width: 120,
            height: 140,
            child: Column(
              mainAxisAlignment: MainAxisAlignment.center,
              children: <Widget>[
                RaisedButton(
                  child: Text('Ubah Warna'),
                  color: _color,
                  textColor: Colors.white,
                  onPressed: () {
                    _gantiBackground();
                  },
                )
              ],
            ),
          )
        ],
      ),
    );
  }
}
```

* Pada kode diatas kita membuat fungsi khusus yang mengubah nilai warna menjadi acak dan fungsi __setState__ nya ada di dalam fungsi __gantiBackground__.
* Selain itu juga terdapat fungsi __initState__ yang mana fungsi ini akan dieksekusi saat widget pertamakali muncul, biasanya fungsi ini digunakan untuk mempersiapkan variabel atau untuk meload data dari server.
* Ketikkan kode berikut di widget __UbahUkuran__:

```dart
class UbahUkuran extends StatefulWidget {
  const UbahUkuran({
    Key key,
  }) : super(key: key);

  @override
  _UbahUkuranState createState() => _UbahUkuranState();
}

class _UbahUkuranState extends State<UbahUkuran> {
  final Random _random = Random();
  double _width = 40;
  double _height = 40;

  void _gantiUkuran() {
    setState(() {
     _width =  _random.nextInt(200).toDouble();
     _height = _random.nextInt(200).toDouble(); 
    });
  }

  @override
  Widget build(BuildContext context) {
    return Container(
      height: 200,
      child: Row(
        children: <Widget>[
          Expanded(
            child: Center(
              child: AnimatedContainer(
                duration: Duration(milliseconds: 500),
                width: _width, height: _height,
                color: Colors.red,
              ),
            )
          ),
          Container(
            width: 120, height: 200,
            child: Column(
              mainAxisAlignment: MainAxisAlignment.center,
              children: <Widget>[
                RaisedButton(
                  color: Colors.orange,
                  textColor: Colors.white,
                  onPressed: () {
                    _gantiUkuran();
                  },
                  child: Text('Ubah Ukuran'),
                )
              ],
            ),
          )
        ],
      )
    );
  }
}
```

* Jalankan aplikasi, Restart bila diperlukan

## Tugas

* Buat aplikasi dengan tombol yang bisa menampilkan gambar secara acak dari website [picsum.photo](https://picsum.photo). Contoh URLnya adalah: __https://picsum.photos/id/{image}/200/300__ dimana __{image}__ nya diganti dengan angka id gambar di https://picsum.photos/images