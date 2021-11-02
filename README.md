# Write Your First Flutter App

Oke, jadi kali ini saya akan belajar membuat aplikasi android menggunakan Flutter dengan mengikuti tutorial ini 
[Lab: Write your first Flutter app](https://flutter.dev/docs/get-started/codelab) dan ini [Write Your First Flutter App, part 2](https://codelabs.developers.google.com/codelabs/first-flutter-app-pt2/)
Aplikasi yang akan dibuat adalah aplikasi sederhana yang menghasilkan nama yang diusulkan untuk perusahaan startup. Pengguna dapat memilih dan membatalkan pilihan nama, menyimpan yang terbaik. Saat pengguna menggulir, lebih banyak nama dihasilkan. Tidak ada batasan seberapa jauh pengguna dapat menggulir.
Oke langsung saja, berikut langkah-langkah dalam membuat aplikasinya:
1. Membuat aplikasi baru memnggunakan terminal dengan command "flutter create hello_world"
2. Membuka file main.dart, menghapus semua isinya dan mengganti dengan kode berikut : 
```
   import 'package:flutter/material.dart';

   void main() => runApp(MyApp());

  class MyApp extends StatelessWidget {
    @override
    Widget build(BuildContext context) {
      return MaterialApp(
        title: 'Welcome to Flutter',
        home: Scaffold(
          appBar: AppBar(
            title: const Text('Welcome to Flutter'),
          ),
          body: const Center(
            child: Text('Hello World'),
          ),
        ),
      );
    }
  }
```
3. Output dari kode diatas <br><br> <img src="https://user-images.githubusercontent.com/57280697/139542824-aab72b8e-8a80-4db9-a200-0c332298a5f3.png" width="150">

4. Selanjutnya, kita akan menggunakan paket sumber terbuka bernama english_words , yang berisi beberapa ribu kata bahasa Inggris yang paling sering digunakan ditambah beberapa fungsi utilitas.Anda dapat menemukan english_wordspaket tersebut, serta banyak paket open source lainnya, di pub.dev . The pubspec.yamlberkas mengelola aset dan dependensi untuk aplikasi Flutter. Di pubspec.yaml, tambahkan english_words (3.1.5 atau lebih tinggi) ke daftar dependensi:

Pada langkah ini, Anda akan menambahkan widget stateful, RandomWords, yang membuat Statekelasnya, _RandomWordsState. Anda kemudian akan menggunakan RandomWordssebagai anak di dalam MyAppwidget stateless yang ada .

Buat kode boilerplate untuk widget stateful. Di lib/main.dart, posisikan kursor Anda setelah semua kode, masukkan Return beberapa kali untuk memulai pada baris baru. Di IDE Anda, mulailah mengetik stful. Editor menanyakan apakah Anda ingin membuat Statefulwidget. Tekan Kembali untuk menerima. Kode boilerplate untuk dua kelas muncul, dan kursor diposisikan bagi Anda untuk memasukkan nama widget stateful Anda.

Lalu memasukkan RandomWordssebagai sebagai nama widget yang akan kita gunakan. 

Setelah memasukkan RandomWordsnama widget stateful, IDE secara otomatis memperbarui Statekelas yang menyertainya , menamainya _RandomWordsState. Secara default, nama Statekelas diawali dengan underbar. Awalan pengenal dengan garis bawah memberlakukan privasi dalam bahasa Dart dan merupakan praktik terbaik yang direkomendasikan untuk Stateobjek.

IDE juga secara otomatis memperbarui kelas status ke extend State, yang menunjukkan bahwa Anda menggunakan State kelas generik yang dikhususkan untuk digunakan denganRandomWords. Sebagian besar logika aplikasi berada di sini—ini mempertahankan status RandomWordswidget. Kelas ini menyimpan daftar pasangan kata yang dihasilkan, yang tumbuh tanpa batas saat pengguna menggulir dan, di bagian 2 lab ini, pasangan kata favorit saat pengguna menambahkan atau menghapusnya dari daftar dengan mengalihkan ikon hati.

Membuat stateful widget :
```
class MyApp extends StatelessWidget {
    @override
    Widget build(BuildContext context) {
  	      final wordPair = WordPair.random();
      return MaterialApp(
        title: 'Welcome to Flutter',
        home: Scaffold(

            title: const Text('Welcome to Flutter'),
          ),
          body: Center(
        child: Text(wordPair.asPascalCase),
        child: RandomWords(),
          ),
        ),
      );
    }

```

5. Selanjutnya, kita akan membuat ListView bergulir yang tak terbatas Pada langkah ini, ini akan memperluas _RandomWordsState untuk menghasilkan dan menampilkan daftar kata. Saat pengguna menggulir daftar (ditampilkan dalam ListViewwidget) kata-kata baru akan muncul tanpa batas.

6. Selanjutnya kita akan menambhkan icon "Hati" disamping tiap kata yang muncul, fungsi icon ini nantinya akan menandai mana nama yg menjadi favorite kita. Oke langsung saja, kita akan menambahkan variabel dengan nama "_saved" untuk menampung nama favorite kedalam class _RandomWordsState seperti berikut :
```
class _RandomWordsState extends State<RandomWords> {
  final _suggestions = <WordPair>[];
  final _saved = <WordPair>{};     // NEW
  final _biggerFont = TextStyle(fontSize: 18.0);
  ...
}
```

7. Dalam fungsi _buildRowfungsi, kita akan menambahkan tanda alreadySavedcentang untuk memastikan bahwa pasangan kata belum ditambahkan ke favorit.
```
Widget _buildRow(WordPair pair) {
  final alreadySaved = _saved.contains(pair);  // NEW
  ...
}
```

8. Pada fungsi _buildRow(), ktia juga akan menambahkan ikon berbentuk hati ke ListTileobjek untuk mengaktifkan favorit. Seperti kode dibawah
```
Widget _buildRow(WordPair pair) {
  final alreadySaved = _saved.contains(pair);
  return ListTile(
    title: Text(
      pair.asPascalCase,
      style: _biggerFont,
    ),
    trailing: Icon(   // NEW from here... 
      alreadySaved ? Icons.favorite : Icons.favorite_border,
      color: alreadySaved ? Colors.red : null,
      semanticLabel: alreadySaved ? 'Remove from saved' : 'Save',
    ),                // ... to here.
  );
}
```

9. Hasilnya setelah semua yg kita lakukan diatas : <br><br>
<img src="https://user-images.githubusercontent.com/57280697/139816128-084d49f2-9587-4eb7-bea9-f1d90e068181.png" width="150">


10. Okee. Selanjutnya kita akan membuat ikon hati dapat diketuk. Saat pengguna mengetuk entri dalam daftar, mengubah status favoritnya, pasangan kata itu ditambahkan atau dihapus dari kumpulan favorit yang disimpan.

Untuk melakukannya, kita akan memodifikasi _buildRowfungsi. Jika entri kata telah ditambahkan ke favorit, mengetuknya lagi akan menghapusnya dari favorit. Saat icon telah diketuk, fungsi memanggil setState()untuk memberi tahu framework bahwa status telah berubah.

Tambahkan onTapke _buildRowmetode, seperti yang ditunjukkan di bawah ini:
```
Widget _buildRow(WordPair pair) {
  final alreadySaved = _saved.contains(pair);
  return ListTile(
    title: Text(
      pair.asPascalCase,
      style: _biggerFont,
    ),
    trailing: Icon(
      alreadySaved ? Icons.favorite : Icons.favorite_border,
      color: alreadySaved ? Colors.red : null,
      semanticLabel: alreadySaved ? 'Remove from saved' : 'Save',
    ),
    onTap: () {      // NEW lines from here...
      setState(() {
        if (alreadySaved) {
          _saved.remove(pair);
        } else { 
          _saved.add(pair); 
        } 
      });
    },               // ... to here.
  );
}
```

11. Hasilnya setelah semua yg kita lakukan diatas : <br><br>
<img src="https://user-images.githubusercontent.com/57280697/139816883-20f9ad8d-9ed1-4ff9-839e-bf3f10242402.png" width="150">

Kini icon "hati" kita sudah bisa diklik :heart: :grin: 

12. Oke lanjut lagi kita.. Sekarang kita akan menambahkan navigasi yang akan mengalihkna kita ke halaman baru yang berisi kata yg telah kita favoritkan.
Selanjutnya, kita akan menambahkan ikon daftar ke AppBardalam buildmetode untuk _RandomWordsState. Saat pengguna mengklik ikon daftar, rute baru yang berisi favorit yang disimpan didorong ke Navigator, menampilkan ikon.

Menambahkan ikon dan tindakan terkaitnya ke metode build:
```
class _RandomWordsState extends State<RandomWords> {
  ...
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Startup Name Generator'),
        actions: [
          IconButton(
            icon: const Icon(Icons.list),
            onPressed: _pushSaved,
            tooltip: 'Saved Suggestions',
          ),
        ],
      ),
      body: _buildSuggestions(),
    );
  }
  ...
}
```

Menambahkan fungsi baru yaitu _pushSaved() kedalam class _RandomWordsState
```
void _pushSaved() {
  }
```

13. Memanggil Navigator.push, seperti yang ditunjukkan di bawah, yang mendorong rute ke tumpukan Navigator. 
```
void _pushSaved() {
  Navigator.of(context).push(
  );
}
```

14. Selanjutnya, Kita akan menambahkan MaterialPageRoute dan buildernya. Untuk saat ini, tambahkan kode yang menghasilkan ListTilebaris. The divideTiles() metode ListTile menambah jarak horisontal antara masing-masing ListTile. Variabel divided memegang baris akhir dikonversi ke daftar dengan fungsi kenyamanan, toList().
```
void _pushSaved() {
    Navigator.of(context).push(
      // Add lines from here...
      MaterialPageRoute<void>(
        builder: (context) {
          final tiles = _saved.map(
            (pair) {
              return ListTile(
                title: Text(
                  pair.asPascalCase,
                  style: _biggerFont,
                ),
              );
            },
          );
          final divided = tiles.isNotEmpty
              ? ListTile.divideTiles(
                  context: context,
                  tiles: tiles,
                ).toList()
              : <Widget>[];

          return Scaffold(
            appBar: AppBar(
              title: const Text('Saved Suggestions'),
            ),
            body: ListView(children: divided),
          );
        },
      ), // ...to here.
    );
  }
```

Properti builder mengembalikan Scaffold yang berisi bilah aplikasi untuk rute baru bernama SavedSuggestions. Tubuh rute baru terdiri dari ListView yang berisi baris ListTiles. Setiap baris dipisahkan oleh pembatas.

15. Hasilnya setelah di hot reload : <br><br>
<img src="https://user-images.githubusercontent.com/57280697/139819188-44a27d16-256b-4c0b-8ca9-f7c3b29be0e4.png" width="150px" style="display:inline-block;" >
<img src="https://user-images.githubusercontent.com/57280697/139819385-28b6643b-55cb-40da-98df-bbf92679f2f8.png" width="150px" style="display:inline-block;">

Kini nama yang telah kita favoritkan dikumpulkan didalm 1 halaman khusus



## Getting Started

This project is a starting point for a Flutter application.

A few resources to get you started if this is your first Flutter project:

- [Lab: Write your first Flutter app](https://flutter.dev/docs/get-started/codelab)
- [Cookbook: Useful Flutter samples](https://flutter.dev/docs/cookbook)

For help getting started with Flutter, view our
[online documentation](https://flutter.dev/docs), which offers tutorials,
samples, guidance on mobile development, and a full API reference.
