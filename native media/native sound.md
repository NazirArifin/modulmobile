# Ionic Native (Media)

Tujuan pembelajaran: Mahasiswa memahami dan dapat menggunakan Ionic Native untuk menyimpan dan memainkan media audio dengan baik.

## Persiapan

* Ketikkan perintah berikut ini:
```sh
ionic start media blank
```

* Tunggu beberapa saat hingga proses selesai. Jika sudah ketikkan perintah berikut:
```sh
cd media
ionic serve
```

* Berikutnya adalah menginstall ionic camera native plugin menggunakan perintah sebagai berikut:
```sh
ionic cordova plugin add cordova-plugin-media
npm install --save @ionic-native/media

ionic cordova plugin add cordova-plugin-file
npm install --save @ionic-native/file
```

* Jalankan aplikasi di browser dengan perintah ```ionic serve```.

## Media Player

* Sekarang kita buka file ```src/app/app.module.ts``` untuk mengimport ke module:

```typescript
import { Media } from '@ionic-native/media';
import { File } from '@ionic-native/file';
```

* Kemudian masukkan ke bagian __providers__ seperti berikut:

```typescript
providers: [
  StatusBar,
  SplashScreen,
  {provide: ErrorHandler, useClass: IonicErrorHandler},
  Media,
  File
]
```

* Selanjutnya buka file ```src/pages/home/home.ts``` dan lakukan import module seperti berikut:

```typescript
import { NavController, Platform } from 'ionic-angular';
import { Media, MediaObject } from '@ionic-native/media';
import { File } from '@ionic-native/file';
```

* Kemudian masukkan ke dalam __constructor__:

```typescript
constructor(public navCtrl: NavController,
  private media: Media,
  private file: File,
  public platform: Platform) {}
```

* Deklarasikan beberapa variabel yang akan kita butuhkan dan letakkan dalam Class seperti berikut:

```typescript
recording: boolean = false;
filePath: string;
fileName: string;
audio: MediaObject;
audioList: any[] = [];
```

* Selanjutnya kita buat fungsi untuk meload data daftar file yang telah direkam menggunakan __localStorage__ seperti berikut:

```typescript
getAudioList() {
  if(localStorage.getItem("audiolist")) {
    this.audioList = JSON.parse(localStorage.getItem("audiolist"));
    console.log(this.audioList);
  }
}

ionViewWillEnter() {
  this.getAudioList();
}
```

* Selanjutnya buat fungsi untuk memulai perekaman suara seperti berikut:

```typescript
startRecord() {
  if (this.platform.is('android')) {
    this.fileName = 'record'+new Date().getDate()+new Date().getMonth()+new Date().getFullYear()+new Date().getHours()+new Date().getMinutes()+new Date().getSeconds()+'.3gp';
    this.filePath = this.file.externalDataDirectory.replace(/file:\/\//g, '') + this.fileName;
    this.audio = this.media.create(this.filePath);
  }
  this.audio.startRecord();
  this.recording = true;
}
```

* Dan fungsi untuk menghentikan proses perekaman seperti berikut:

```typescript
stopRecord() {
  this.audio.stopRecord();
  let data = { filename: this.fileName };
  this.audioList.push(data);
  localStorage.setItem("audiolist", JSON.stringify(this.audioList));
  this.recording = false;
  this.getAudioList();
}
```

* Kemudian fungsi untuk memainkan atau memeutar file yang telah tersimpan dengan fungsi sebagai berikut:

```typescript
playAudio(file,idx) {
  if (this.platform.is('ios')) {
    this.filePath = this.file.documentsDirectory.replace(/file:\/\//g, '') + file;
    this.audio = this.media.create(this.filePath);
  } else if (this.platform.is('android')) {
    this.filePath = this.file.externalDataDirectory.replace(/file:\/\//g, '') + file;
    this.audio = this.media.create(this.filePath);
  }
  this.audio.play();
  this.audio.setVolume(0.8);
}
```

* Sekarang ubah file ```src/pages/home/home.html``` menjadi seperti berikut:

```html
<ion-header>
  <ion-navbar>
    <ion-title>
      Sound Recorder & Player
    </ion-title>
  </ion-navbar>
</ion-header>

<ion-content padding>
  <ion-card>
    <ion-card-content>
      <ion-card-title>
        <button ion-button primary (click)="stopRecord()" *ngIf="recording"><ion-icon name="mic-off"></ion-icon>&nbsp;&nbsp;Stop Record</button>
        <button ion-button primary (click)="startRecord()" *ngIf="!recording"><ion-icon name="mic"></ion-icon>&nbsp;&nbsp;Start Record</button>
      </ion-card-title>
    </ion-card-content>
  </ion-card>
  <ion-list>
    <ion-item *ngFor="let audio of audioList; index as i;">
      <p>{{audio.filename}}</p>
      <button ion-button clear item-end large (click)="playAudio(audio.filename, i)"><ion-icon name="play"></ion-icon></button>
    </ion-item>
  </ion-list>
</ion-content>
```

## Jalankan dan tes ke Perangkat

* Untuk mencoba aplikasi yang telah dibuat Anda harus menjalankannya di device secara langsung, pastikan SDK Android Anda sudah diset dengan benar kemudian jalankan perintah berikut:

```sh
ionic cordova run android
```

