# Ionic Native (Akses Kamera)

Tujuan pembelajaran: Mahasiswa memahami dan dapat menggunakan Ionic Native untuk mengakses kamera dengan baik.

## Persiapan

* Ketikkan perintah berikut ini:
```sh
ionic start camera blank
```

* Tunggu beberapa saat hingga proses selesai. Jika sudah ketikkan perintah berikut:
```sh
cd camera
ionic serve
```

* Berikutnya adalah menginstall ionic camera native plugin menggunakan perintah sebagai berikut:
```sh
ionic cordova plugin add cordova-plugin-camera
npm install --save @ionic-native/camera
```

## Menggunakan Kamera

* Masukkan modul Camera ke ionic app module dengan cara mengedit file ```src/app/app.module.ts``` dan mengimport dan memasukkan dalam __providers__:

```typescript
import { Camera } from '@ionic-native/camera';

...

  providers: [
    StatusBar,
    SplashScreen,
    {provide: ErrorHandler, useClass: IonicErrorHandler},
    Camera
  ]

...
```

* Kita edit view yaitu file ```src/pages/home/home.html``` dengan menambahkan tombol yang ketika diklik akan memunculkan __ActionSheet__ yang berisi pilihan sumber gambar yang akan ditampilkan ke aplikasi apakah menggunakan Kamera atau menggunakan Pustaka. Masukkan kode berikut di dalam __ion-content__:

```html
<button ion-button icon-left full large (click)="presentActionSheet()">
  <ion-icon name="image"></ion-icon>
  Ambil Foto
</button>

<ion-card *ngIf="base64Image">
  <ion-card-content>
    <img [src]="base64Image" alt="">
  </ion-card-content>
</ion-card>
```

* __ion-card__ kita gunakan untuk menampilkan hasil pengambilan foto, perhatikan bahwa kita menggunakan __*ngIf__ _directive_ untuk menampilkan ion-card hanya jika properti __base64Image__ tidak kosong. Kita buat properti __base64Image__ nanti.

* Setelah view selesai kita dapat mulai menuliskan kode typescript di file ```src/pages/home/home.ts```. Pertama kita import __Camera__, __CameraOptions__ dan __ActionSheetController__ dan memasukkannya kedalam __constructor__:

```typescript
import { NavController, ActionSheetController } from 'ionic-angular';
import { Camera, CameraOptions } from '@ionic-native/camera';

...
  
  constructor(public navCtrl: NavController, 
              public actionSheetCtrl: ActionSheetController, 
              private camera: Camera) {
  }

...
```

* Kita kerjakan ActionSheet terlebih dahulu, buat menu Actionsheet dengan menambahkan kode seperti berikut:

```typescript
  
  ...
  public presentActionSheet() {
    let actionSheet = this.actionSheetCtrl.create({
      title: 'Pilih Sumber Gambar',
      buttons: [
        {
          text: 'Dari Pustaka',
          icon: 'image',
          handler: () => {
            this.ambilPustaka();
          }
        },
        {
          text: 'Dari Kamera',
          icon: 'camera',
          handler: () => {
            this.ambilFoto();
          }
        }, {
          text: 'Batal',
          role: 'cancel'
        }
      ]
    });
    actionSheet.present();
  }
  ...


```

* Selanjutnya kita buat _property_ dan _methods_ yang kita butuhkan yaitu __base64Image__, __ambilPustaka()__, dan __ambilFoto()__ dengan memasukkan kode berikut:

```typescript

  ...
  public base64Image: string;

  ambilFoto() {
    const options: CameraOptions = {
      quality: 100,
      targetWidth: 500,
      targetHeight: 500,
      destinationType: this.camera.DestinationType.DATA_URL,
      encodingType: this.camera.EncodingType.JPEG,
      mediaType: this.camera.MediaType.PICTURE,
      saveToPhotoAlbum: true
    }

    this.camera.getPicture(options).then((imageData) => {
      this.base64Image = 'data:image/jpeg;base64,' + imageData;
    }, (err) => {
      console.log(err);
    });
  }

  ambilPustaka() {
    const options: CameraOptions = {
      quality: 100,
      targetWidth: 500,
      targetHeight: 500,
      destinationType: this.camera.DestinationType.DATA_URL,
      sourceType: this.camera.PictureSourceType.PHOTOLIBRARY,
      encodingType: this.camera.EncodingType.JPEG
    }

    this.camera.getPicture(options).then((ImageData) => {
      this.base64Image = 'data:image/jpeg;base64,' + ImageData;
    }, (err) => {
      console.log(err);
    });
  }
  ...

```

* Fungsi utama yang digunakan dalam plugin Kamera ini adalah fungsi __camera.getPicture()__ yang menggunakan beragam _option_ sebagai argumennya. Beragam option yang dapat Anda gunakan bisa dilihat di: [https://ionicframework.com/docs/native/camera/](https://ionicframework.com/docs/native/camera/)

## Ujicoba di Device

* Untuk melakukan ujicoba Anda tidak dapat menggunakan _browser_, gunakan _device_ Anda untuk menjalankan aplikasi yang Anda buat. Untuk Android maka Anda setidaknya laptop/pc Anda dilengkapi dengan Java JDK (minimal versi 8), Android Studio, SDK Manager [https://ionicframework.com/docs/intro/deploying/](https://ionicframework.com/docs/intro/deploying/)

* Untuk menjalankan aplikasi maka Anda harus meng_enable_ USB Debuggin di perangkat Android Anda dan pastikan Developer Mode sudah aktif/on. Kemudian ketikkan perintah berikut:

```sh
ionic cordova run android --device
```

## Ujicoba dengan Ionic DevApp

* Anda bisa install Ionic DevApp di PlayStore pastikan laptop/pc dan hp Anda berada di jaringan yang sama. Setelah ```ionic serve``` jalankan Ionic DevApp, begitu aplikasi Anda muncul di hp tekan dan otomatis perubahan di laptop/pc akan terjadi pula di hp Anda.