# Flutter Widgets (Layout)

Tujuan pembelajaran: Mahasiswa memahami dan dapat menggunakan beragam widget dasar di Flutter untuk membuat layout tampilan aplikasi

## Persiapan

* Jika Anda belum mempunyai project Flutter maka Anda dapat membuat project baru dengan menggunakan _Command Pallete_ (Ctrl + Shift + P) di Visual Studio Code lalu pilih perintah: __Flutter: New Project__. Ikuti petunjuk berikutnya sampai selesai dan VSCode membuka file __lib/main.dart__.

* Edit file __pubspec.yaml__ dibagian _environment_ diubah menjadi:
```yaml
...
environment:
  sdk: ">=2.2.2 <3.0.0"
...
```

* Jalankan aplikasi pada perangkat / emulator dengan menggunakan menu Debug - Start Debugging

## Hello World

* Sekarang kita akan membuat tampilan Hello World di Flutter, di file __lib/main.dart__ hapus semua kode sisakan dua baris teratas berikut:

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());
```

* Di pojok kiri bawah Anda akan melihat indikator apabila terdapat kesalahan yang diwakili dengan icon silang diikuti angka kesalahan. Selalu perhatikan bagian ini untuk memastikan tidak ada yang salah pada kode yang sedang Anda buat.

* Selain itu di layar kode bagian ```MyApp()``` bergaris bawah merah yang mana jika kita lintaskan _mouse_ maka akan terdapat keterangan kesalahan apa yang ada pada kode tersebut.

* Berikutnya untuk memperbaiki kesalahan kode tersebut kita akan membuat __Class MyApp__ seperti berikut:

```dart
...
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return null;
  }
}
```

_** Ketika Anda menyimpan file maka Flutter akan melakukan Hot Reload sehingga tampilan di perangkat Anda seharusnya sudah berubah secara otomatis._

* __Class MyApp__ merupakan turunan dari __StatelessWidget__ yang mana di Flutter hampir semuanya adalah widget, termasuk mengatur tata letak dan layout.

* Widget utama selalu memiliki _method_ __build()__ yang mendeskripsikan bagaimana menampilkan widget-widget di dalamnya. Sekarang kita lengkapi __Class MyApp__ menjadi seperti berikut:

```dart
...
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Welcome to Flutter',
      home: Scaffold(
        appBar: AppBar(
          title: Text('Welcome to Flutter'),
        ),
        body: Center(
          child: Text('Hello World'),
        ),
      )
    );
  }
}
```

* __MaterialApp__ adalah widget dasar yang menyediakan beragam widget Material sebagai standar untuk mobile dan web. __Scaffold__ menyediakan komponen __appBar__, __body__ dan __title__ sebagai layout dasar dari aplikasi. Sedangkan __Center__, dan __Text__ adalah beberapa widget contoh yang sering digunakan saat membuat aplikasi di Flutter.

## Widget Dasar

* Flutter menyediakan banyak sekali widget yang mungkin diawal-awal akan sangat membingungkan untuk memilih widget mana yang akan digunakan. Namun secara umum terdapat beberapa widget utama yang sering digunakan yaitu:

1. __Text__
Widget Text digunakan untuk menampilkan text di aplikasi

2. __Row, Column__
Widget berbasis flex yang digunakan untuk membuat layout secara horizontal (Row) dan vertikal (Column). Desainnya sama dengan penggunaan flex yang ada di web

3. __Stack__
Disamping disusun secara berjajar, widget juga dapat disusun bertumpuk menggunakan __Stack__

4. __Container__
Widget Container berbentuk kotak (box) dan biasanya digunakan sebagai wadah widget lain. Container dapat "dihias" seperti memberi background, border atau shadow. Container juga memiliki margin, dan padding.

## Latihan

* Ketikkan kode berikut untuk membuat tampilan seperti contoh:

![](https://raw.githubusercontent.com/NazirArifin/modulmobile/master/img/recipe1.png)

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Welcome to Flutter',
      home: Scaffold(
        appBar: AppBar(
          title: Text('Strawberry Pavlova'),
        ),
        body: Center(
          child: FractionallySizedBox(
            widthFactor: 0.8,
            child: Column(
              children: <Widget>[
                Padding(
                  padding: EdgeInsets.only(top: 30, bottom: 20),
                  child: Text('Strawberry Pavlova', style: TextStyle(
                    fontSize: 20
                  )),
                ),
                Container(
                  child: Text("Pavlova is a meringue-based dessert named after the Russian ballerine Anna Pavlova.\nPavlova features a crisp crust and soft, light inside, topped with fruit and whipped cream.", textAlign: TextAlign.center, style: TextStyle(
                    fontSize: 14, height: 1.2
                  )),
                ),
                Padding(
                  padding: EdgeInsets.only(top: 20, bottom: 20),
                  child: Row(
                    mainAxisAlignment: MainAxisAlignment.center,
                    children: <Widget>[
                      Row(
                        children: <Widget>[
                          for (var i = 0; i < 4; i++) Icon(Icons.star),
                          Icon(Icons.star, color: Colors.yellow)
                        ],
                      ),
                      Padding(
                        padding: EdgeInsets.only(left: 30),
                        child: Text('170 Reviews'),
                      )
                    ],
                  ),
                ),
                Container(
                  margin: EdgeInsets.only(top: 5),
                  child: Row(
                    mainAxisAlignment: MainAxisAlignment.center,
                    children: <Widget>[
                      Column(
                        children: <Widget>[
                          Icon(Icons.bookmark_border),
                          Padding(
                            padding: EdgeInsets.only(top: 5, bottom: 10),
                            child: Text('PREP')
                          ),
                          Text('25 min')
                        ],
                      ),
                      Column(
                        children: <Widget>[
                          Icon(Icons.alarm),
                          Padding(
                            padding: EdgeInsets.only(
                              top: 5, bottom: 10,
                              left: 30, right: 30
                            ),
                            child: Text('COOK')
                          ),
                          Text('1 hr')
                        ],
                      ),
                      Column(
                        children: <Widget>[
                          Icon(Icons.local_dining),
                          Padding(
                            padding: EdgeInsets.only(top: 5, bottom: 10),
                            child: Text('FEEDS')
                          ),
                          Text('4-6')
                        ],
                      )
                    ],
                  ),
                )
              ],
            ),
          ),
        ),
      )
    );
  }
}
```

## Tugas

* Hias tampilan kode diatas dengan menambahkan widget __Image.network__ dengan url image: https://ichef.bbci.co.uk/food/ic/food_16x9_832/recipes/strawberrypavlova_88895_16x9.jpg diatas judul dan ubah warna teks agar sesuai dengan gambar berikut:

![](https://raw.githubusercontent.com/NazirArifin/modulmobile/master/img/recipe2.png)