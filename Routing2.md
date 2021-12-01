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
              subtitle: Text('Kucing disebut juga kucing domestik atau kucing rumah (nama ilmiah: Felis silvestris catus atau Felis catus)', overflow: TextOverflow.ellipsis)
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

- Selanjutnya ubah class ```ListScreen``` menjadi Stateful Widget dengan mengklik kanan class, pilih menu Refactor dan pilih __Convert to StatefulWidget__.

- Kita akan menjadi class Animal yang akan menjadi tipe dari item di list dan juga parameter yang akan dikirimkan ke form ketika menambahkan data baru. Buat folder ```class``` dan kemudian buat file ```animal.dart``` dengan isi sebagai berikut:

```dart
class Animal {
  final int id;
  final String code;
  final String name;
  final String info;

  Animal({
    required this.id,
    required this.code,
    required this.name,
    required this.info
  });
}
```

- Tambahkan kode berikut tepat diatas method ```build(BuildContext context)``` di class ```ListScreenState```:

```dart
...
  List<Animal> _animalList = [];
  late Animal _selected;

  @override
  void initState() {
    super.initState();

    setState(() {
      _animalList.add(Animal(id: 1, code: 'cat', name: 'Kucing', info: 'Kucing disebut juga kucing domestik atau kucing rumah (nama ilmiah: Felis silvestris catus atau Felis catus)'));
      _animalList.add(Animal(id: 2, code: 'dog', name: 'Anjing', info: 'Anjing atau anjing domestik (Canis lupus familiaris) adalah mamalia yang mengalami domestikasi dari serigala sejak 15.000 tahun yang lalu'));
      _animalList.add(Animal(id: 3, code: 'fish', name: 'Ikan', info: 'Ikan adalah anggota vertebrata poikilotermik (berdarah dingin) yang hidup di air dan bernapas dengan insang'));
    });
  }
...
```

- Berikutnya kita sesuaikan ListView agar bisa menampilkan data yang kita masukkan di fungsi ```setState``` dengan mengubah bagian berikut:

```dart
...
        itemBuilder: (context, index) {
          Animal item = _animalList[index];

          return GestureDetector(
            ...
            child: ListTile(
              leading: CircleAvatar(
                backgroundColor: Colors.red,
                backgroundImage: NetworkImage('https://loremflickr.com/480/480/${item.code}'),
              ),
              title: Text(item.name),
              subtitle: Text(item.info, overflow: TextOverflow.ellipsis),
              isThreeLine: true,
            ),
          );
        }, 
        ... 
        itemCount: _animalList.length
...
```

- Jika berjalan dengan baik, maka tampilan di device Anda akan berisi tiga item binatang yang sudah kita masukkan data list (Tekan __Restart__ jika tampilan belum berubah)