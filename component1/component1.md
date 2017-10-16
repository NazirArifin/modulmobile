# Component (1)

Tujuan pembelajaran: Mahasiswa mengenal dan dapat menggunakan Ionic components dengan baik.

## Persiapan

* Ketikkan perintah berikut ini:
```sh
ionic start component blank
```

* Tunggu beberapa saat hingga proses selesai. Jika sudah ketikkan perintah berikut:
```sh
cd component
ionic serve
```

## Pembuatan Aplikasi "Kalkulator Cinta"

* Untuk mengubah judul di header Anda bisa melakukannya dengan membuka file ```src/pages/home/home.html```, edit bagian ```ion-header``` seperti berikut:

```html
<ion-header>
  <ion-navbar>
    <ion-title>
      Kalkulator Cinta
    </ion-title>
  </ion-navbar>
</ion-header>
```

### Memasukkan Component

* Selanjutnya kita ingin memasukkan componen __Input__ dimana kita dapat memasukkan nama yang akan diolah. Masukkan kode berikut tepat setelah ```ion-content```.

```html
<ion-list>
    
  <ion-item>
    <ion-label floating>Nama1</ion-label>
    <ion-input type="text"></ion-input>
  </ion-item>
  
  <ion-item>
    <ion-label floating>Nama2</ion-label>
    <ion-input type="text"></ion-input>
  </ion-item>
  
</ion-list>
```

* Daftar component lengkap untuk ionic dapat dilihat di [Ionic Components]()

* Untuk menampilkan hasil perhitungan kita menggunakan component __Card__ yang berupa _container_ dengan _drop shadow_. Tambahkan kode berikut setelah ```ion-list```

```html
<ion-card>
    
  <ion-card-header>
    NAMA1 dan NAMA2
  </ion-card-header>
  
  <ion-card-content>
    <div class="score">
      90%
    </div>
  </ion-card-content>
  
</ion-card>
```

### Styling Component

* Agar teks di Card menjadi rata tengan, kita dapat menggunakan _class utilites_ __text-center__ yang ditambahkan di tag ```ion-card```. Edit ```ion-card``` menjadi:

```html
<ion-card text-center>

  ...

</ion-card>
```

* Daftar lengkap class utilities yang dapat Anda gunakan dapat dilihat di [Ionic Utilities]()

* Untuk memperbesar teks di dalam _class_ __score__, Anda dapat mengubah file ```src/pages/home/home.scss``` menjadi seperti berikut:

```scss
page-home {
  .score{
    font-size: 500%;
    font-weight: bold;
  }
}
```

### Angular One Way Data Binding

* Kita ingin agar teks yang ada di component card menjadi dinamis, karena itu kita membuat variabel yang menjadi _model_ di class __. Nilai dari variabel ini dapat ditampilkan di _view_ (tampilan) secara langsung menggunakan tanda __{{ namaVariabel }}__.

* Untuk membuat model, edit file ```src/pages/home/home.ts``` menjadi seperti berikut:

```typescript
import { Component } from '@angular/core';
import { NavController } from 'ionic-angular';

@Component({
  selector: 'page-home',
  templateUrl: 'home.html'
})
export class HomePage {

  nama1: string = 'Romeo'; // variabel untuk nama1
  nama2: string = 'Juliet'; // variabel untuk nama2
  // getter untuk nilai score
  get score() {
    return 90;
  }
  
  constructor(public navCtrl: NavController) {

  }

}
```

* Sedangkan untuk menampilkan nilai dari variabel di tampilan, Anda ubah file ```src/pages/home/home.html``` dibagian Card menjadi:

```html
<ion-card-header>
  {{ nama1 || '???' }}
  dan
  {{ nama2 || '???' }}
</ion-card-header>
  
<ion-card-content>
  <div class="score">
    {{ score }}%
  </div>
</ion-card-content>
```

* Sekarang coba Anda mengubah nilai variabel dalam class maka otomatis tampilannya juga akan berubah.

### Angular Two Way Data Binding

* Jika Anda ingin selain menampilkan nilai variabel tapi juga mengubah nilainya melalui tampilan maka Anda dapat melakukannya dengan menambahkan tambahan atribut __[(ngModel)]__ pada input di tampilan. Edit file ```src/pages/home/home.html``` di bagian input menjadi seperti berikut:

```html
    <ion-item>
      <ion-label floating>Nama1</ion-label>
      <ion-input type="text" [(ngModel)]="nama1"></ion-input>
    </ion-item>
  
    <ion-item>
      <ion-label floating>Nama2</ion-label>
      <ion-input type="text" [(ngModel)]="nama2"></ion-input>
    </ion-item>
```

* Sekarang coba Anda mengetikkan di input nama1 maka teks di bagian Card juga akan berubah.

* Selanjutnya adalah bagian perhitungan yang menghasilkan angka antara 1-100 berdasarkan teks yang dimasukkan dalam aplikasi. Ubah file ```src/pages/home/home.ts``` di fungsi __get score() menjadi seperti berikut:

```typescript
  get score() {
    // teks digabung lalu dinormalisasi dengan mengubah menjadi lowercase
    let full: string = (this.nama1 + this.nama2).toLowerCase();
    let total = 0;
    // hitung total dengan kode ASCII karakter ke i
    for (let i = 0; i < full.length; i++) {
      total += full.charCodeAt(i);
    }
    // return hasilnya
    return total % 101;
  }
```

* Selamat, aplikasi sederhana dengan ionic sudah berhasil Anda buat.
* Sekarang coba Anda ubah kata __dan__ menjadi icon-heart