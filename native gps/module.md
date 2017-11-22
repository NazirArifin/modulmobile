# Ionic Native (Geolocation)

Tujuan pembelajaran: Mahasiswa memahami dan dapat menggunakan Ionic Native untuk mengakses geolocation (GPS) dengan baik.

## Persiapan

* Ketikkan perintah berikut ini:
```sh
ionic start gps blank
```

* Tunggu beberapa saat hingga proses selesai. Jika sudah ketikkan perintah berikut:
```sh
cd gps
ionic serve
```

* Berikutnya adalah menginstall ionic geolocation native plugin menggunakan perintah sebagai berikut:
```sh
ionic cordova plugin add cordova-plugin-geolocation --variable GEOLOCATION_USAGE_DESCRIPTION="Lokasi Saya"
npm install --save @ionic-native/geolocation
```

## Menggunakan Geolocation

* Masukkan modul Geolocation ke ionic app module dengan cara mengedit file ```src/app/app.module.ts``` dan mengimport serta memasukkan dalam __providers__:

```typescript
import { Geolocation } form '@ionic-native/geolocation';

...

  providers: [
	StatusBar,
    SplashScreen,
    {provide: ErrorHandler, useClass: IonicErrorHandler},
    Geolocation
  ]

...
```

* Kita edit view yaitu file ```src/pages/home.home.html``` untuk menampilkan angka __latitude__ dan __longitude__ yang didapatkan dari perangkat. Masukkan kode berikut di dalam __ion-content__:

```html
  <ion-list>
    <ion-list-header>
      Latitude
    </ion-list-header>
    <ion-item>
      {{ latitude }}
    </ion-item>
    <ion-list-header>
      Longitude
    </ion-list-header>
    <ion-item>
      {{ longitude }}
    </ion-item>
  </ion-list>
```

* Setelah view selesai kita dapat mulai menuliskan kode _typescript_ di file ```src/pages/home.home.ts```. Pertama kita import __Geolocation__ dan kemudian dimasukkan ke _constructor_.

```typescript
import { Geolocation } form '@ionic-native/geolocation';

...

  constructor(public navCtrl: NavController, private geolocation: Geolocation) {
    
    this.loadLocation();

  }

...
```

* Selanjutnya kita buat property untuk menampung data latitude dan longitude beserta fungsi untuk mendapatkan nilainya dengan memasukkan kode berikut:

```typescript

...
public latitude: string;
public longitude: string;

loadLocation() {
  let options = {
    enableHighAccuracy: true,
    timeout: 5000,
    maximumAge: 0
  };
  
  const watch = this.geolocation.watchPosition(options);
  watch.subscribe((data) => {
    if (data.coords !== undefined) {
      this.latitude = data.coords.latitude;
      this.longitude = data.coords.longitude;
    } else {
      console.log('Error lokasi');
    }
  });
}

...
```

## Ujicoba

* Dengan menggunakan browser Anda sudah dapat mendapatkan nilai latitude dan longitude serta muncul di view, namun jika Anda ingin mengujinya di device atau emulator maka Anda dapat menggunakan perintah berikut:

```sh
ionic cordova run android
```

* Pastikan device Anda terhubung ke komputer.
