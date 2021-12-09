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
  Animal? _selected;

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
              subtitle: Text(item.info, overflow: TextOverflow.ellipsis)
            ),
          );
        }, 
        ... 
        itemCount: _animalList.length
...
```

- Jika berjalan dengan baik, maka tampilan di device Anda akan berisi tiga item binatang yang sudah kita masukkan data list (Tekan __Restart__ jika tampilan belum berubah)

- Untuk melakukan proses seleksi item di list, kita akan menggunakan ```GestureDetector```. Selain itu kita juga akan membuat animasi background item menjadi biru muda ketika item terpilih dengan event longPress. Kode ```ListTile``` di ```itemBuilder``` kita ubah menjadi sebagai berikut:

```dart
...
          return GestureDetector(
            onLongPress: () {
              setState(() => _selected = item);
            },
            onTap: () {
              if (_selected != null) {
                setState(() => _selected = null);
              }
            },
            child: AnimatedContainer(
              duration: const Duration(milliseconds: 300),
              color: _selected != null && _selected?.id == item.id ? Colors.blue.shade100 : Colors.transparent,
              child: ListTile(
                leading: CircleAvatar(
                  backgroundColor: Colors.red,
                  backgroundImage: NetworkImage('https://loremflickr.com/480/480/${item.code}'),
                ),
                title: Text(item.name),
                subtitle: Text(item.info, overflow: TextOverflow.ellipsis),
              ),
            ),
          );
...
```

- Pada event __onLongPress__ terdapat kode ```setState``` yang mengeset isi variabel ```_selected``` menjadi item yang merupakan isi dari list pada index yang aktif. Sedangkan event __onTap__ digunakan untuk melepaskan pilihan agar variabel ```_selected``` menjadi null kembali.

- Dengan isi variabel ```_selected``` yang bisa berisi null dan juga bisa berisi item dari list, maka kita bisa menyembunyikan atau menampilkan tombol Edit dan Hapus di widget ```AppBar```. Kode dari ```AppBar``` bagian ```action``` berubah menjadi seperti berikut:

```dart
...
        actions: <Widget>[
          AnimatedOpacity(
            duration: const Duration(milliseconds: 300),
            opacity: _selected == null ? 0 : 1,
            child: IconButton(
              onPressed: () {
          
              }, 
              icon: const Icon(Icons.edit)
            ),
          ),
          AnimatedOpacity(
            duration: const Duration(milliseconds: 300),
            opacity: _selected == null ? 0 : 1,
            child: IconButton(
              onPressed: () {
                setState(() {
                  _animalList.removeWhere((a) => a.id == _selected?.id);
                  _selected = null;
                });
              },
              icon: const Icon(Icons.delete)
            ),
          )
        ],
...
```

- Jika berhasil maka tombol Edit dan Hapus akan muncul jika ada item yang terpilih. Untuk menghapus item di List sudah bisa dilakukan yaitu menggunakan method ```removeWhere``` yang mana akan menghapus item yang id nya sama dengan id di variabel ```_selected```.

### Tambah dan Edit Form

- Untuk dapat menambah dan mengedit data Animal, maka diperlukan tampilan form. Buat file baru di folder __screen__ dan beri nama __add.dart__ dengan isi sebagai berikut:

```dart
import 'package:flutter/material.dart';

class AddScreen extends StatelessWidget {
  const AddScreen({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Form Animal'),
      ),
      body: Container(
        constraints: const BoxConstraints.expand(),
        margin: const EdgeInsets.symmetric(horizontal: 20, vertical: 0),
        child: Form(
          child: SingleChildScrollView(
            child: Column(
              children: <Widget>[
                const Padding(
                  padding: EdgeInsets.symmetric(vertical: 30),
                  child: Text('Edit Animal', style: TextStyle(
                    fontSize: 24
                  ), textAlign: TextAlign.center),
                ),
                // input kode
                TextFormField(
                  decoration: const InputDecoration(
                    labelText: 'Masukkan Kode'
                  )
                ),
                // input nama
                TextFormField(
                  decoration: const InputDecoration(
                    labelText: 'Masukkan Nama'
                  ),
                ),
                // input keterangan
                TextFormField(
                  decoration: const InputDecoration(
                    labelText: 'Masukkan Keterangan',
                  ),
                  maxLines: 2, minLines: 1
                ),
                // tombol
                Padding(
                  padding: const EdgeInsets.symmetric(vertical: 20.0),
                  child: SizedBox(
                    width: double.infinity,
                    child: ElevatedButton(
                      onPressed: () {},
                      child: const Text('SIMPAN'),
                    ),
                  ),
                )
              ],
            ),
          ),
        ),
      ),
    );
  }
}
```

- Sekarang jadikan class AddScreen menjadi StatefulWidget dengan menggunakan menu __Refactor__ dan pilih __Convert to StatefulWidget__. Tambahkan FormState dan TextEditingController seperti berikut di class _AddScreenState_:

```dart
...
class _AddScreenState extends State<AddScreen> {
  final _formKey = GlobalKey<FormState>();
  final _namaController = TextEditingController();
  final _kodeController = TextEditingController();
  final _infoController = TextEditingController();
  int id = 0;

  @override
  void dispose() {
    _kodeController.dispose();
    _namaController.dispose();
    _infoController.dispose();
    super.dispose();
  }
...
         child: Form(
          key: _formKey,
...
                TextFormField(
                  controller: _kodeController,
                  decoration: const InputDecoration(
...
                TextFormField(
                  controller: _namaController,
                  decoration: const InputDecoration(
...
                TextFormField(
                  controller: _infoController,
                  decoration: const InputDecoration(
...
                    child: ElevatedButton(
                      onPressed: () {
                        // untuk id kita gunakan angka random
                        Random rand = Random();
                        String code = _kodeController.text;
                        String nama = _namaController.text;
                        String info = _infoController.text;
                        Animal animal = Animal(
                          id: rand.nextInt(1000), 
                          code: code, 
                          name: nama, info: info
                        );
                        Navigator.pop(context, animal);
                      },
                      child: const Text('SIMPAN'),
                    ),
```

- Setelah selesai kita tambahkan route di file __main.dart__ menjadi seperti berikut:

```dart
...
      routes: {
        '/': (context) => const ListSceen(),
        '/add': (context) => const AddScreen(),
      },
...
```

- Kemudian kembali ke file __list.dart__, di widget ```floatingActionButton``` di event ```onPresssed``` kita beri kode seperti berikut:

```dart
...
        onPressed: () async {
          final dynamic hasil = await Navigator.pushNamed(context, '/add');
          if (hasil != null) {
            setState(() => _animalList.add(hasil));
          }
        },
...
```

- Jika berhasil maka ketika tombol floatingAction ditekan maka akan muncul form input Animal. Setelah selesai ditambahkan dan ditekan tombol SIMPAN maka form akan kembali ke tampilan list dengan data yang baru dimasukkan sudah ada di List.

- Untuk bisa mengedit, kita perlu mengedit tombol Edit di appBar menjadi seperti berikut:

```dart
...
              onPressed: () async {
                final dynamic result = await Navigator.pushNamed(context, '/add', arguments: _selected);
                  if (result != null) {
                    setState(() {
                      // remove yang idnya sama
                      final index = _animalList.indexWhere((v) => result.id == v.id);
                      _animalList[index] = result;
                      _selected = null;
                    });
                  }
              }, 
...
```

- Selanjutnya di file __add.dart__ kita perlu kode untuk menangkap data yang akan diedit. Di bagian method ```build``` kita tambahkan kode berikut:

```dart
...
  Widget build(BuildContext context) {
    final dynamic args = ModalRoute.of(context)!.settings.arguments;
    if (args != null) {
      setState(() {
        id = args.id;
        _kodeController.text = args.code;
        _namaController.text = args.name;
        _infoController.text = args.info;
      });
    }
...
                    child: ElevatedButton(
                      onPressed: () {
                        // untuk id kita gunakan angka random
                        ...
                        Animal animal = Animal(
                          id: id > 0 ? id : rand.nextInt(1000),
...
```

- Jika berhasil maka data yang diedit setelah tombol SIMPAN ditekan akan berubah.

## Tugas

- Hafalkan secara konsep, dan tulis ulang waktu UAS!