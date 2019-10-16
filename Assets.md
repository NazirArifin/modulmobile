# Flutter Local Assets

Tujuan pembelajaran: Mahasiswa memahami dan dapat menggunakan assets berupa font dan gambar untuk membuat aplikasi lebih menarik.

## Persiapan

- Jika Anda belum mempunyai project Flutter maka Anda dapat membuat project baru dengan menggunakan _Command Pallete_ (Ctrl + Shift + P) di Visual Studio Code lalu pilih perintah: __Flutter: New Project__. Ikuti petunjuk berikutnya sampai selesai dan VSCode membuka file __lib/main.dart__.
- Download dan letakkan file berikut:

| File                  | Lokasi                             |
| --------------------- | ---------------------------------- |
| heart.png             | assets/images/heart.png            |
| Pacifico-Regular.ttf  | assets/fonts/Pacifico-Regular.ttf  |
| Poppins-Regular.ttf   | assets/fonts/Poppins-Regular.ttf   |
| Poppins-ExtraBold.ttf | assets/fonts/Poppins-ExtraBold.ttf |

* Kemudian edit file __pubspec.yaml__ dengan menambahkan kode berikut dibagian assets:

```yaml
...
  assets:
    - assets/images/heart.png

  fonts:
    - family: Pacifico
      fonts:
        - asset: assets/fonts/Pacifico-Regular.ttf
          style: normal
    - family: Poppins
      fonts:
        - asset: assets/fonts/Poppins-Regular.ttf
          style: normal
        - asset: assets/fonts/Poppins-ExtraBold.ttf
          style: normal
          weight: 900
...
```

* Jalankan aplikasi dengan menekan tombol F5
* Ubah file __lib/main.dart__ menjadi seperti berikut:

```dart
import 'package:flutter/material.dart';
import 'package:flutter/services.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    SystemChrome.setEnabledSystemUIOverlays([]);

    const textStyle = TextStyle(
      fontFamily: 'Pacifico',
      fontSize: 40
    );

    return MaterialApp(
      home: Scaffold(
        body: Container(
          constraints: BoxConstraints.expand(),
          decoration: BoxDecoration(
            gradient: LinearGradient(
              begin: Alignment.topCenter,
              end: Alignment.bottomCenter,
              colors: [Color(0xffb81d46), Color(0xff804ad9)]
            )
          ),
          child: Stack(
            fit: StackFit.expand,
            children: <Widget>[
              Opacity(
                opacity: 0.1,
                child: Container(
                  decoration: BoxDecoration(
                    image: DecorationImage(
                      image: AssetImage('assets/images/heart.png'),
                      fit: BoxFit.fitWidth
                    ),
                  ),
                ),
              ),
              Center(
                child: RichText(
                  text: TextSpan(
                    children: <TextSpan>[
                      TextSpan(text: 'The', style: textStyle),
                      TextSpan(text: "\nLOVE\n", style: TextStyle(
                        color: Colors.white,
                        fontFamily: 'Poppins',
                        fontSize: 70, fontWeight: FontWeight.w900,
                        height: 0.8
                      )),
                      TextSpan(text: 'Calculator', style: textStyle.copyWith(
                        height: 1
                      ))
                    ]
                  ),
                ),
              )
            ],
          ),
        ),
      ),
    );
  }
}
```

## Tugas

* Desain halaman ketiga dari aplikasi The LOVE Calculator!

