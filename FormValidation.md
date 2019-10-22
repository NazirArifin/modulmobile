# Flutter Form Validation

Tujuan pembelajaran: Mahasiswa memahami dan dapat menggunakan form dan melakukan validasi input dengan baik.

## Persiapan

- Jika Anda belum mempunyai project Flutter maka Anda dapat membuat project baru dengan menggunakan _Command Pallete_ (Ctrl + Shift + P) di Visual Studio Code lalu pilih perintah: __Flutter: New Project__. Ikuti petunjuk berikutnya sampai selesai dan VSCode membuka file __lib/main.dart__.
- Berdasarkan praktikum sebelumnya, kita akan menambahkan font baru yaitu JuliusSansOne, download file __.ttf__ nya dan masukkan ke folder __assets/fonts__, dan kemudian edit file __pubspec.yaml__ dengan menambahkan font assets seperti berikut:

```yaml
...
  assets:
    - assets/images/heart.png

  fonts:
    - family: JuliusSansOne
      fonts:
        - asset: assets/fonts/JuliusSansOne-Regular.ttf
          style: normal
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

* Jalankan aplikasi dengan menggunakan menu __Debug -> Start Debugging__
* Edit file __lib/main.dart__ menjadi seperti berikut:

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
      color: Colors.white
    );
    const labelStyle = TextStyle(
      fontFamily: 'JuliusSansOne',
      color: Colors.white,
      fontSize: 18
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
            fit: StackFit.loose,
            children: <Widget>[
              Opacity(
                opacity: 0.1,
                child: Align(
                  alignment: Alignment.center,
                  child: Container(
                    height: 450,
                    decoration: BoxDecoration(
                      image: DecorationImage(
                        image: AssetImage('assets/images/heart.png'),
                        fit: BoxFit.cover,
                      ),
                    ),
                  ),
                ),
              ),

              Container(
                constraints: BoxConstraints.expand(),
                child: Column(
                  children: <Widget>[
                    Expanded(
                      child: Container(
                        margin: EdgeInsets.symmetric(horizontal: 65, vertical: 0),
                        child: Form(
                          child: Center(
                            child: Column(
                              mainAxisAlignment: MainAxisAlignment.center,
                              children: <Widget>[
                                Padding(
                                  padding: const EdgeInsets.all(6.0),
                                  child: Text('Person 1', style: labelStyle),
                                ),
                                InputPerson(
                                  index: 1,
                                ),
                                Padding(
                                  padding: EdgeInsets.only(top: 17, bottom: 15),
                                  child: Text('loves', style: textStyle.copyWith(fontSize: 22)),
                                ),
                              ],
                            ),
                          ),
                        ),
                      ),
                    ),
                    Padding(
                      padding: EdgeInsets.only(bottom: 75),
                      child: GestureDetector(
                        onTap: () {
                          print('Hitung!');
                        },
                        child: Container(
                          margin: EdgeInsets.symmetric(horizontal: 65, vertical: 0),
                          width: double.infinity,
                          height: 50,
                          decoration: BoxDecoration(
                            gradient: LinearGradient(
                              begin: Alignment.centerLeft,
                              end: Alignment.centerRight,
                              colors: [
                                Color(0xffbb1f50), Color(0xffe95d83)
                              ]
                            ),
                            border: Border.all(color: Colors.black26),
                            borderRadius: BorderRadius.circular(30),
                            boxShadow: [ BoxShadow(
                              color: Colors.white.withAlpha(60),
                              blurRadius: 7,
                              spreadRadius: 2
                            )]
                          ),
                          child: Center(
                            child: Text('Calculate', style: textStyle.copyWith(fontSize: 20))
                          ),
                        ),
                      ),
                    )
                  ],
                ),  
              )
            ]
          ),
        ),
      ),
    );
  }
}

class InputPerson extends StatefulWidget {
  const InputPerson({
    Key key,
    @required this.index,
  }) : _index = index, super(key: key);
  
  final int index;

  @override
  _InputPersonState createState() => _InputPersonState();
}

class _InputPersonState extends State<InputPerson> {
  FocusNode _focusNode;

  @override
  void dispose() {
    super.dispose();
    _focusNode.dispose();
  }

  @override
  void initState() {
    super.initState();
    _focusNode = FocusNode();
    _focusNode.addListener(() {
      setState(() {});
    });
  }

  @override
  Widget build(BuildContext context) {
    const inputStyle = TextStyle(
      fontFamily: 'JuliusSansOne',
      fontSize: 16, 
      color: Color(0xff55021e),
      fontWeight: FontWeight.bold
    );
    var _i = widget._index;

    return DecoratedBox(
      decoration: BoxDecoration(
        borderRadius: BorderRadius.circular(30),
        boxShadow: [
          BoxShadow(
            color: _focusNode.hasFocus ? Colors.white : Colors.transparent,
            blurRadius: 7, spreadRadius: 1
          )
        ]
      ),
      child: TextFormField(
        autocorrect: false,
        textCapitalization: TextCapitalization.none,
        style: inputStyle,
        focusNode: _focusNode,
        decoration: InputDecoration(
          filled: true,
          contentPadding: EdgeInsets.symmetric(horizontal: 20, vertical: 13),
          hintStyle: inputStyle,
          hintText: 'Person ${widget.index}',
          fillColor: _focusNode.hasFocus ? Color(0xfff3e3f5) : Color(0xffdea8d0),
          border: OutlineInputBorder(
            borderRadius: BorderRadius.circular(30)
          ),
          focusedBorder: OutlineInputBorder(
            borderRadius: BorderRadius.circular(30),
            borderSide: BorderSide(color: Colors.transparent)
          )
        ),
      ),
    );
  }
}
```

* Tambahkan input kedua untuk memasukkan nama orang kedua dengan menambahkan "Label" dan __InputPerson__ baru seperti berikut:

```dart
...
  Padding(
    padding: const EdgeInsets.all(6.0),
    child: Text('Person 2', style: labelStyle),
  ),
  InputPerson(
    index: 2,
  ),
...
```

* Refactor Widget __Container__ children kedua dari __Stack__ dengan menjadikannya menjadi widget baru dengan nama __FormPerson__ dan sekaligus jadikan __Stateful Widget__
* Tambahkan kode berikut di __FormPersonState__:

```dart
  final _formKey = GlobalKey<FormState>();
  final _person1Controller = TextEditingController();
  final _person2Controller = TextEditingController();

  @override
  void dispose() {
    super.dispose();
    _person1Controller.dispose();
    _person2Controller.dispose();
  }

  @override
  void initState() {
    super.initState();
  }

...
  
              child: Form(
                key: _formKey,
...
```

* Tambahkan property dan argumen baru di class __InputPerson__ seperti berikut:

```dart
...
  const InputPerson({
    Key key,
    @required this.index,
    @required this.controller // tambah ini
  }) : super(key: key);
  
  final int index;
  final TextEditingController controller; // tambah ini
...
```

* Di __FormPersonState__ akan meminta field __controller__ di Widget __InputPerson__ karena itu perlu diubah menjadi:

```dart
    ...
    InputPerson(
      index: 1,
      controller: _person1Controller,
    ),
    ...
    InputPerson(
      index: 2,
      controller: _person2Controller,
    ),
    ...
```



* Di class __InputPersonState__ tambahkan controller dan validator di __TextFormField__ sebagai berikut:

```dart
...      
      child: TextFormField(
        controller: widget.controller,
        validator: (value) {
          if (value.isEmpty) {
            return 'Person ${widget.index} must not be empty!';
          }
          return null;
        },
...
```

* Di widget tombol _Calculate_ bagian __onTap__ tambahkan kode berikut:

```dart
...
    onTap: () {
      if (_formKey.currentState.validate()) {
        // _calculateLove();
      }
    },
...
```

* Ketika Anda tekan seharusnya muncul error kesalahan tepat dibawah input karena kita belum memasukkan input dengan lengkap.
* Sekarang kita akan buat method yang melakukan perhitungan di dalam class __FormPersonState__ seperti berikut:

```dart
  ...
  void _calculateLove() {
    // sembunyikan keypad
    SystemChannels.textInput.invokeMethod('TextInput.hide');
    final full = (_person1Controller.text + _person2Controller.text).toLowerCase();
    var total = 0;
    for (var i = 0; i < full.length; i++) {
      total += full.codeUnitAt(i);
    }
    final result = total % 101;
    final snackBar = SnackBar(content: Text('Hasilnya Adalah: $result'));
    Scaffold.of(context).showSnackBar(snackBar);
  }
  ...
```

* Buang komentar di __onTap__ dan kemudian coba jalankan aplikasi dengan mengisi nama Person1 dan Person2!

## Tugas

* Gabungkan tampilan dari praktikum sebelumnya, hasil tugas sebelumnya dengan kode praktikum saat ini, gunakan widget __PageView__ untuk berpindah-pindah tampilan!