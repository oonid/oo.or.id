---
title: "ORM di Flutter dengan moor"
date: 2020-08-08 20:08:08+08:00
publishDate: 2020-08-08 20:08:08+08:00
image: "/images/2020/object_relational_mapping_orm.webp"
url: "/content/orm-flutter-dengan-moor"
tags: ["Dart", Flutter", "ORM", "SQlite"]
draft: false
---

{{< figure src="/images/2020/object_relational_mapping_orm.webp" title="Object Relational Mapping ORM di Flutter dengan moor" >}}


Dengan pengalaman memrogram untuk web backend, untuk interaksi basis data sering memilih untuk menggunakan Object-Relational Mapping (ORM) karena beberapa alasan, antara lain:
* Keamanan, untuk menghindari SQL Injection, ORM sudah dilengkapi dengan sejumlah fitur untuk membersihkan (_sanitize_) masukan ke sintaksisnya
* Kemudahan, ya buat sejumlah orang yang belum banyak berinteraksi dengan SQL, memilih ORM karena menggunakan bahasa (pemrograman) yang sama
* Tambahan fitur seperti _cache_, yang akan mempercepat proses jika diakses secara berulang

Beberapa waktu belakang ini banyak eksplorasi tentang Flutter, nggak hanya tentang [Flutter web](https://oo.or.id/content/dart-web-group-discussion-bahasa-indonesia/), tapi dukungan Flutter juga semakin baik untuk membuat aplikasi Linux. Sayangnya pustaka pendukung belum langsung bisa untuk sekaligus banyak platform.

Contohnya untuk berinteraksi dengan basis data SQLite, di Flutter untuk aplikasi ponsel direkomendasikan plugin `sqflite` [^1], namun paket tersebut tidak/belum mendukung platform web dan Linux.

Sebagai alternatif, **pilihannya adalah `moor`** [^2], yang juga merupakan plugin untuk berinteraksi dengan basis data SQLite. `moor` ini anagram (utak-atik huruf) dari `room`, yang di platform Android merupakan nama pustaka untuk akses basis data. Dan serunya lagi, `moor` tidak hanya mendukung sintaksis SQL, juga secara mendasar mendukung konsep ORM, dimana kita mendefinisikan struktur tabel serta interaksinya dengan bahasa pemrograman Dart. Buat yang memrogram Dart secara reaktif, pustakan ini juga punya dukungan untuk pemrograman reaktif, dengan menghasilkan keluaran dari query berupa _stream_ yang terus-menerus diperbarui. `moor` mendukung untuk pembuatan aplikasi Flutter di banyak platform, termasuk juga di platform web dan Linux yang sedang dieksplorasi.

Di sisi lain, dalam menggunakan moor untuk menyimpan data ini tidak ada cara langsung yang mudah (_straigh forward_), dimana kita perlu melakukan beberapa langkah terlebih dahulu. :thinking:

Menambahkan paket `moor` di `pubspec.yaml` bukan hanya satu atau dua baris seperti kebanyakan paket, tapi 6 baris yang perlu ditambahkan :joy:.

```yaml
dependencies:
  moor: # use the latest version
  sqlite3_flutter_libs: # Also use the latest version.
  path_provider:
  path:

dev_dependencies:
  moor_generator: # use the latest version
  build_runner: 
```

Di daftar paket tersebut ada `moor_generator`, selanjutnya akan dibahas mengenai bagaimana `moor` perlu dilakukan `build` dahulu untuk membangkitkan kode Dart, misalnya definisi dari tabel kita simpan pada file `database.dart` selanjutnya menggunakan perintah `flutter packages pub run build_runner build` akan menghasilkan file `database.g.dart`, dimana `g.dart` di situ sudah menjadi kesepakatan untuk menggunakan ekstensi (akhiran nama file) tersebut untuk kode yang dibangkitkan (_generated_) dari kode dart lain.

Berikut adalah contoh definisi tabel dan basis data menggunakan `moor` yang disimpan pada file bernama `database.dart`.

```dart
import 'package:moor/moor.dart';

// assuming that your file is called database.dart. This will give an error at first,
// but it's needed for moor to know about the generated code
part 'database.g.dart';

// this will generate a table called "todos" for us. The rows of that table will
// be represented by a class called "Todo".
class Todos extends Table {
  IntColumn get id => integer().autoIncrement()();
  TextColumn get title => text().withLength(min: 6, max: 32)();
  TextColumn get content => text().named('body')();
  IntColumn get category => integer().nullable()();
}

// this annotation tells moor to prepare a database class that uses both of the
// tables we just defined. We'll see how to use that database class in a moment.
@UseMoor(tables: [Todos, ])
class MyDatabase {
  
}
```

Nantinya saat file tersebut dituliskan, akan dianggap error dahulu dia awal pada baris `part 'database.g.dart';`, karena belum dibangkitkan file `database.g.dart`.

Bangkitkan (_generate_) dengan perintah `flutter packages pub run build_runner build` atau kalo ingin terus menerus membangkitkan secara otomatis pada saat menuliskan struktur tabel dan basis data, gunakan perintah `flutter packages pub run build_runner watch`.

Setelah selesai membangkitkan file `database.g.dart`, kita sudah siap menggunakan basis data dengan `moor`, misalnya dengan menyiapkan kelengkapan `class MyDatabase` di atas, serta mekanisme untuk koneksi ke basis data. Contoh kodenya seperti berikut.

```dart
// These imports are only needed to open the database
import 'package:moor/ffi.dart';
import 'package:path_provider/path_provider.dart';
import 'package:path/path.dart' as p;
import 'package:moor/moor.dart';
import 'dart:io';

LazyDatabase _openConnection() {
  // the LazyDatabase util lets us find the right location for the file async.
  return LazyDatabase(() async {
    // put the database file, called db.sqlite here, into the documents folder
    // for your app.
    final dbFolder = await getApplicationDocumentsDirectory();
    final file = File(p.join(dbFolder.path, 'db.sqlite'));
    return VmDatabase(file);
  });
}

@UseMoor(tables: [Todos, ])
class MyDatabase extends _$MyDatabase {
  // we tell the database where to store the data with this constructor
  MyDatabase() : super(_openConnection());

  // you should bump this number whenever you change or add a table definition. 
  // Migrations are covered later in this readme.
  @override
  int get schemaVersion => 1;
}
```

Perhatikan bahwa `class MyDatabase` tersebut merupakan turunan dari `class _$MyDatabase` sehingga kita bisa mengakses sejumlah fitur dan tabel yang sudah kita definisikan dengan `moor` sebelumnya (sebelum dibangkitkan).

Untuk contoh menuliskan _query_ basis data menggunakan `moor` seperti berikut.

```dart
// inside the database class, the `todos` getter has been created by moor.
@UseMoor(tables: [Todos, ])
class MyDatabase extends _$MyDatabase {  

  // the schemaVersion getter and the constructor from the previous page
  // have been omitted.
  
  // loads all todo entries
  Future<List<Todo>> get allTodoEntries => select(todos).get();

  // watches all todo entries. The stream will automatically
  // emit new items whenever the underlying data changes.
  Stream<List<Todo>> watchEntries() {
    return select(todos).watch();
  }
}
```

Pemanggilan method `.watch()` akan menghasilkan _stream_ yang secara otomatis akan menginformasikan jika ada perubahan data, misalnya data baru.

Untuk menggunakan `moor` hanya sebagai satu _instance_ saja, atau disebut _singleton_, kita dapat mengikuti pandual yang sudah disediakan [^4], dimana salah satu pilihannya adalah menggunakan `InheritedWidgets` atau menggunakan paket `provider` [^5] terutama bagi yang sudah menggunakan `provider` sebagai state management.

Di bagian `runApp()`.

```dart
void main() {
  runApp(
    Provider<MyDatabase>(
      create: (context) => MyDatabase(),
      child: MyFlutterApp(),
      dispose: (context, db) => db.close(),
   ),
  );
}
```

Di bagian yang membutuhkan (akses) instance dari basis data.

```dart
Provider.of<MyDatabase>(context)
```

Pustaka moor ini salah satunya bergantung pada pustaka sqlite3 [^3] untuk mengakses SQLite melalui `dart:ffi`, sehingga untuk keperluan penggunaan di Linux, perlu instalasi paket tambahan, misalnya sebagai berikut.

```bash
sudo apt install libsqlite3-dev
```

Ini pembahasan di acara "Flutter Boring Show" tentang penggunaan moor untuk menyimpan data artikel favorit, di episode 25.

{{< youtube 9o_M-LjO4no >}}

(kelanjutannya di [episode 27](https://www.youtube.com/watch?v=SJKrtx759Xk))

Artikel ini ditulis dengan menggunakan `moor` versi 3.3.1, dimana proses _development_ dari `moor` ini sangat cepat dan kadang membawa perubahan yang membuat kode sebelumnya tidak bisa lagi digunakan (_breaking changes_), jadi saat ingin mencoba, pastikan sudah menyesuaikan dengan panduan dari versi `moor` yang digunakan.



Foto oleh [NASA](https://unsplash.com/@nasa) di Unsplash

[^1]: https://pub.dev/packages/sqflite
[^2]: https://pub.dev/packages/moor
[^3]: https://pub.dev/packages/sqlite3

[^4]: https://moor.simonbinder.eu/faq/#using-the-database
[^5]: https://pub.dev/packages/provider

