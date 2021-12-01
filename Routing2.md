# Flutter Routing & Navigation (2)

Tujuan Pembelajaran: Mahasiswa memahami dan dapat menggunakan routing untuk manajemen data dengan baik.

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

- Pages pertama yang akan kita buat adalah page list yang berisi list binatang sebagai template awal. Buat folder ```screens``` lalu buat file ```list.dart``` dengan isi file seperti berikut:

```dart
import 'package:flutter/material.dart';

class ListSceen extends StatelessWidget {
  const ListSceen({Key? key}) : super(key: key);
  
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      floatingActionButton: FloatingActionButton(
        onPressed: () {
          
        },
        child: const Icon(Icons.add),
        backgroundColor: Colors.green,
      ),
      appBar: AppBar(
        title: const Text('Animals'),
        actions: <Widget>[
          IconButton(
            onPressed: () {

            }, 
            icon: const Icon(Icons.edit)
          ),
          IconButton(
            onPressed: () {

            },
            icon: const Icon(Icons.delete)
          )
        ],
      ),
      body: ListView.separated(
        itemBuilder: (context, index) {
          return GestureDetector(
            onLongPress: () {

            },
            onTap: () {

            },
            child: ListTile(
              leading: CircleAvatar(
                backgroundColor: Colors.red,
                backgroundImage: NetworkImage('https://loremflickr.com/480/480/cat'),
              ),
              title: Text('Kucing'),
              subtitle: Text('Kucing disebut juga kucing domestik atau kucing rumah (nama ilmiah: Felis silvestris catus atau Felis catus)'),
              isThreeLine: true,
            ),
          );
        }, 
        separatorBuilder: (context, index) {
          return const Divider();
        }, 
        itemCount: 1
      ),
    );
  }
}
```

- Selanjutnya modifikasi file ```main.dart``` di bagian ```routes``` menjadi seperti berikut:

```dart
...
import 'package:myapp/screens/list.dart';
...

      routes: {
        '/': (context) => const ListSceen()
      },
```

- Jalankan aplikasi, jika semua berjalan dengan baik maka Anda akan melihat list yang berisi satu item dengan isi foto dan data tentang Kucing

