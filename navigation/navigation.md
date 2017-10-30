# Navigation

Tujuan pembelajaran: Mahasiswa memahami dan dapat menggunakan Ionic navigation dengan baik.

## Persiapan

* Ketikkan perintah berikut ini:
```sh
ionic start navigasi blank
```

* Tunggu beberapa saat hingga proses selesai. Jika sudah ketikkan perintah berikut:
```sh
cd component
ionic serve -l
```

## List Master

* Buka file ```src/pages/home/home.ts``` dan buat sebuah _array_ yang akan ditampilkan di aplikasi. Dalam _class HomePage_ masukkan property __list__:

```typescript
lists: Array<any> = [];
```

* Selanjutnya dalam _constructor_ ubah list dengan nilai-nilai berikut:

```typescript
this.lists = [
  { text: 'HTML5' },
  { text: 'CSS3' },
  { text: 'JS' },
  { text: 'Typescript' }
];
```

* Untuk menampilkan daftar tersebut ke aplikasi, buka file ```src/pages/home/home.html``` dan masukkan _component_ __List__ dalam __ion-content__

```html
<ion-list>
  <ion-item *ngFor="let list of lists" (click)="lihatDetail(list.text)">
    <ion-avatar item-start>
      <img src="http://lorempixel.com/400/400/">
    </ion-avatar>
    <h2>{{ list.text }}</h2>
  </ion-item>
</ion-list>
```

* Pada kode HTML diatas, kita menggunakan _directive_ __ngFor__ untuk menampilkan data __lists__ kedalam sebuah daftar. Selain itu kita menambahkan _event_ __click__ dengan nama _method_ __lihatDetail__.

* Kita akan membuat _page_ baru, ketikkan perintah ini di __command line_:

```sh
ionic generate page detail --no-module
```

* Sebuah folder dengan isi tiga file akan terbuat di folder _page_. Kita masukkan definisi _page_ yang baru kita di file ```src/app/app.component.ts```

```typescript
import { DetailPage } from '../pages/detail/detail';
```

* dan masukkan __DetailPage__ di bagian __declarations__ dan __entryComponents__.


* Selanjutnya edit __home.ts__ dan tambahkan _method_ ___lihatDetail__ seperti berikut:

```typescript
// masukkan di bagian import
import { DetailPage } from '../detail/detail';
``` 

```typescript
// tambahkan di dalam class HomePage
lihatDetail(text: string) {
  this.navCtrl.push(DetailPage, {
    data: text
  });
}
```

* Kita melewatkan sebuah data dengan nama __data__ dengan isi text yang berasal dari list yang kita buat sebelumnya.

* Coba klik list yang sudah dibuat, jika berhasil maka Anda akan dipindah ke halaman Detail yang sementara ini masih kosong.

* Data yang dikirim dari halaman __Home__ akan kita tangkap di halaman __Detail__, karena itu kita perlu memastikan telah meng_import_ ___NavParams__ dari __ionic-angular__. Buka file ```src/pages/detail/detail.ts``` dan masukkan kode import seperti berikut (ubah hanya jika berbeda) dan masukkan dalam _constructor_:

```typescript
import { NavController, NavParams } from 'ionic-angular';
```

```typescript
constructor(public navCtrl: NavController, public navParams: NavParams) {
}
```

* Selanjutnya kita buat properti untuk menampung data dari halaman sebelumnya sekaligus properti untuk menampung teks yang akan dimunculkan sebagai deskripsinya.

```typescript
text: string;
description: string;
```

* Buka file ```src/pages/detail/detail.html``` dan masukkan __Card__ _component_ dalam __ion-content__ seperti berikut:

```html
<ion-card>
  <img src="http://lorempixel.com/400/400/"/>
  <ion-card-content>
    <ion-card-title>
      {{ text }}
      </ion-card-title>
    <p>
      {{ description }}
    </p>
  </ion-card-content>
</ion-card>
```

* Kembali ke file ```src/pages/detail/detail.ts``` dan ubah __constructor__ menjadi seperti berikut:

```typescript
this.text = navParams.get('data');

switch(this.text) {
  case 'HTML5':
    this.description = 'HTML5 adalah ...'; break;
  case 'CSS3':
    this.description = 'CSS3 adalah ...'; break;
  case 'JS':
    this.description = 'JS adalah ...'; break;
  case 'Typescript':
    this.description = 'Typescript adalah ...'; break;
}
```

* Sekarang coba buat tombol untuk kembali ke halaman __Home__
