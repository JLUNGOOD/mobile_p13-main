# Pertemuan_13

Nama : Alwan Alawi

Kelas : TI-3A

Absen : 09


# Praktikum 1: Membangun Layout di Flutter

### Langkah 1: Buat Project Baru
Buatlah sebuah project flutter baru dengan nama books di folder src week-12 repository GitHub Anda.

Kemudian Tambahkan dependensi http dengan mengetik perintah berikut di terminal.
```
flutter pub add http
```

### Langkah 2: Cek file pubspec.yaml
Jika berhasil install plugin, pastikan plugin http telah ada di file pubspec ini seperti berikut.
```
dependencies:
  flutter:
    sdk: flutter
  http: ^1.1.0
```

### Langkah 3: Buka file main.dart

**Soal 1**

*Tambahkan nama panggilan Anda pada title app sebagai identitas hasil pekerjaan Anda.*
Ketiklah kode seperti berikut ini.
```
import 'dart:async';
import 'package:flutter/material.dart';
import 'package:http/http.dart';
import 'package:http/http.dart' as http;

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Alwan Alawi',
      theme: ThemeData(
        primarySwatch: Colors.blue,
        visualDensity: VisualDensity.adaptivePlatformDensity,
      ),
      home: const FuturePage(),
    );
  }
}

class FuturePage extends StatefulWidget {
  const FuturePage({super.key});

  @override
  State<FuturePage> createState() => _FuturePageState();
}

class _FuturePageState extends State<FuturePage> {
  String result=' ';
  @override
  Widget build(BuildContext context){
    return Scaffold(
      appBar: AppBar(
        title: const Text('Back from the Future'),
      ),
      body: Center(
        child: Column(children: [
          const Spacer(),
          ElevatedButton(
            child: const Text('GO!'),
            onPressed: () {},
          ),
          const Spacer(),
          Text(result),
          const Spacer(),
          const CircularProgressIndicator(),
          const Spacer(),
        ]),
      )
    );
  }
}
```
### Langkah 4: Tambah method getData()
Tambahkan method ini ke dalam class _FuturePageState yang berguna untuk mengambil data dari API Google Books.
```
Future<Response> getData() async{
  const authority = 'www.googleapis.com';
  const path = '/books/v1/volumes/junbDwAAQBAJ';
  Uri url = Uri.https(authority, path);
  return http.get(url);
}
```
**Soal 2**

*Carilah judul buku favorit Anda di Google Books, lalu ganti ID buku pada variabel path di kode tersebut. Caranya ambil di URL browser Anda.*

### Langkah 5: Tambah kode di ElevatedButton
Tambahkan kode pada onPressed di ElevatedButton seperti berikut.
```
ElevatedButton(
            child: const Text('GO!'),
            onPressed: () {
              setState(() {});
              getData()
              .then((value) {
                result = value.body.toString().substring(0, 450);
                setState(() { });
              }).catchError((_){
                result = 'An error occurred';
                setState(() { });
              });
            },
          ),
```

Hasil Run : 

![image](https://github.com/JLUNGOOD/mobile_p13-main/assets/106043734/bc92ad47-0526-409a-992d-3e2e88a45d98)

**Soal 3**

*Jelaskan maksud kode langkah 5 tersebut terkait substring dan catchError!*

JAWAB :

- Fungsi substring digunakan untuk mengambil sebagian dari suatu string berdasarkan indeks awal dan akhir tertentu. Dengan mengidentifikasi indeks awal dan indeks akhir, kita dapat memotong bagian dari string tersebut.

- Sedangkan fungsi catchError digunakan untuk menangkap dan menangani kesalahan yang mungkin terjadi selama eksekusi kode di dalam blok try. Ini memberikan cara untuk menangani kesalahan tanpa menghentikan eksekusi program.

# Praktikum 2 Menggunakan await/async untuk menghindari callbacks
### Langkah 1: Buka file main.dart
Tambahkan tiga method berisi kode seperti berikut di dalam class _FuturePageState.
```
Future<int> returnOneAsync() async {
  await Future.delayed(const Duration(seconds: 3));
  return 1;
}

Future<int> returnTwoAsync() async {
  await Future.delayed(const Duration(seconds: 3));
  return 2;
}

Future<int> returnThreeAsync() async {
  await Future.delayed(const Duration(seconds: 3));
  return 3;
}
```
### Langkah 2: Tambah method count()
Lalu tambahkan lagi method ini di bawah ketiga method sebelumnya.
```
Future count() async {
    int total = 0;
    total = await returnOneAsync();
    total += await returnTwoAsync();
    total += await returnThreeAsync();
    setState(() {
      result = total.toString();
    });
  }
```

### Langkah 3: Panggil count()
Lakukan comment kode sebelumnya, ubah isi kode onPressed() menjadi seperti berikut.
```
ElevatedButton(
            child: const Text('GO!'),
            onPressed: () {
              // setState(() {});
              // getData()
              // .then((value) {
              //   result = value.body.toString().substring(0, 450);
              //   setState(() { });
              // }).catchError((_){
              //   result = 'An error occurred';
              //   setState(() { });
              // });
              count();
            },
          ),
```

### Langkah 4: Run
Akhirnya, run atau tekan F5 jika aplikasi belum running. Maka Anda akan melihat seperti gambar berikut, hasil angka 6 akan tampil setelah delay 9 detik.

![image](https://github.com/JLUNGOOD/mobile_p13-main/assets/106043734/8dcf8abc-e42e-4ebb-adce-64aef80eeca6)

**Soal 4**

*Jelaskan maksud kode langkah 1 dan 2 tersebut!*

JAWAB :

-Kode langkah 1 Kode tersebut adalah contoh implementasi fungsi-fungsi asynchronous dalam Dart menggunakan Future. Setiap fungsi (returnOneAsync, returnTwoAsync, dan returnThreeAsync) menunda eksekusi selama 3 detik (simulasi waktu yang membutuhkan penanganan asinkron) dan mengembalikan nilai integer yang berbeda (1, 2, dan 3). Keyword async menandakan bahwa fungsi tersebut adalah asynchronous, dan await digunakan untuk menunggu penundaan waktu sebelum melanjutkan eksekusi kode berikutnya. Fungsi-fungsi ini berguna untuk menangani tugas-tugas yang membutuhkan waktu tanpa menghentikan eksekusi program secara keseluruhan.

-Kode Langkah 2 Kode tersebut merupakan sebuah fungsi asynchronous bernama count, yang menggunakan Future untuk menggabungkan hasil dari tiga fungsi asynchronous lainnya (returnOneAsync, returnTwoAsync, dan returnThreeAsync).

# Praktikum 3: Menggunakan Completer di Future

### Langkah 1: Buka main.dart
Pastikan telah impor package async berikut.
```
import 'package:async/async.dart';
```

### Langkah 2: Tambahkan variabel dan method
Tambahkan variabel late dan method di class _FuturePageState seperti ini.
```
late Completer completer;

Future getNumber() {
  completer = Completer<int>();
  calculate();
  return completer.future;
}

Future calculate() async {
  await Future.delayed(const Duration(seconds : 5));
  completer.complete(42);
}
```

### Langkah 3: Ganti isi kode onPressed()
Tambahkan kode berikut pada fungsi onPressed(). Kode sebelumnya bisa Anda comment
```
getNumber().then((value) {
                setState(() {
                  result = value.toString();
                });
              });
```

### Langkah 4:
Terakhir, run atau tekan F5 untuk melihat hasilnya jika memang belum running. Bisa juga lakukan hot restart jika aplikasi sudah running. Maka hasilnya akan seperti gambar berikut ini. Setelah 5 detik, maka angka 42 akan tampil.

![image](https://github.com/JLUNGOOD/mobile_p13-main/assets/106043734/45cb020b-cb2f-45e8-8181-b3020c6d0f2e)

**Soal 5**

*Jelaskan maksud kode langkah 2 tersebut!*

JAWAB :

-Kode tersebut adalah contoh penggunaan Completer dalam Dart, yang digunakan untuk mengontrol atau menyelesaikan suatu Future secara manual.

### Langkah 5: Ganti method calculate()
Gantilah isi code method calculate() seperti kode berikut, atau Anda dapat membuat calculate2()
```
Future calculate2() async {
    try {
      await new Future.delayed(cost Duration(seconds: 5));
      completer.complete(42);
     }
     catch (_){
      completer.completeError({});
     }
  }
```

### Langkah 6: Pindah ke onPressed()
Ganti menjadi kode seperti berikut.
```
getNumber().then((value) {
  setState(() {
    result = value.toString();
  });
}).catchError((e) {
  result = 'An error occurred';
});
```
**Soal 6**

*Jelaskan maksud perbedaan kode langkah 2 dengan langkah 5-6 tersebut!*

JAWAB :

- Dalam langkah 2, kode membuat objek Completer dan menyelesaikannya dengan nilai 42 setelah penundaan 5 detik tanpa menangani kesalahan secara eksplisit. Sementara dalam langkah 5-6, kode menambahkan blok try-catch pada fungsi calculate untuk menangani potensial kesalahan selama penundaan, dan pada bagian penggunaan hasil (getNumber().then(...)) ditambahkan catchError untuk menangkap kesalahan yang mungkin terjadi, memberikan pesan kesalahan default 'An error occurred'. Perbedaannya terletak pada pendekatan penanganan kesalahan yang lebih komprehensif pada langkah 5-6.

  
# Praktikum 4: Memanggil Future secara paralel

### Langkah 1: Buka file main.dart
Tambahkan method ini ke dalam class _FuturePageState
```
void returnFG(){
    FutureGroup<int> futureGroup = FutureGroup <int>();
    futureGroup.add(returnOneAsync());
    futureGroup.add(returnTwoAsync());
    futureGroup.add(returnThreeAsync());
    futureGroup.close();
    futureGroup.future.then((List <int> value){
      int total = 0;
      for (var element  in value){
        total += element;
      }
        setState(() {
          result = total.toString();
        });
    });
  }
```

### Langkah 2: Edit onPressed()
Anda bisa hapus atau comment kode sebelumnya, kemudian panggil method dari langkah 1 tersebut.
```
onPredes(){
  returnFG();
};
```

### Langkah 3: Run
Anda akan melihat hasilnya dalam 3 detik berupa angka 6 lebih cepat dibandingkan praktikum sebelumnya menunggu sampai 9 detik.

**Soal 7**

*Capture hasil praktikum Anda berupa GIF dan lampirkan di README. Lalu lakukan commit dengan pesan "W12: Soal 7".*
Hasil Run:

![image](https://github.com/JLUNGOOD/mobile_p13-main/assets/106043734/9e055328-67d4-40f1-9ac9-ab5639a74a4d)

### Langkah 4: Ganti variabel futureGroup
Anda dapat menggunakan FutureGroup dengan Future.wait seperti kode berikut.
```
final futures = Future.wait<int>([
  returnOneAsync(),
  returnTwoAsync(),
  returnThreeAsync(),
]);
```
**Soal 8**

*Jelaskan maksud perbedaan kode langkah 1 dan 4!*

JAWAB:

- Perbedaan utama terletak pada penggunaan alat bantu yang berbeda (FutureGroup vs. Future.wait) untuk mengelola dan menggabungkan hasil dari beberapa future. Penggunaan tergantung pada kebutuhan dan preferensi pengembang. FutureGroup memberikan fleksibilitas lebih besar untuk menambah atau menghapus future dari grup secara dinamis sebelum menutupnya, sedangkan Future.wait menyederhanakan proses untuk menunggu sekelompok future sekaligus.

# Praktikum 5: Menangani Respon Error pada Async Code

### Langkah 1: Buka file main.dart
Tambahkan method ini ke dalam class _FuturePageState
```
Future returnEror() async {
    await Future.delayed(const Duration(seconds: 2));
    throw Exception('Something terrible happened');
  }

```

### Langkah 2: ElevatedButton
Ganti dengan kode berikut
```
returnEror()
                .then((value) {
                  setState(() {
                    result = 'Success';
                  });
                }).catchError((onError){
                  setState(() {
                    result = onError.toString();
                  });
                }).whenComplete(() => print('Complete'));
```

### Langkah 3: Run

Lakukan run dan klik tombol GO! maka akan menghasilkan seperti gambar berikut.

**Soal 9**

*Capture hasil praktikum Anda berupa GIF dan lampirkan di README. Lalu lakukan commit dengan pesan "W12: Soal 9".*

![image](https://github.com/JLUNGOOD/mobile_p13-main/assets/106043734/9741dfe6-e7f7-4b01-b4e1-2c7d5bfbeab7)


### Langkah 4: Tambah method handleError()
Tambahkan kode ini di dalam class _FutureStatePage
```
Future handleError() async{
    try {
      await returnError();
    }
    catch (error) {
      setState(() {
        result = error.toString();
      });
    }
    finally{
      print('Complete');
    }
  }
```

**Soal 10**

*Panggil method handleError() tersebut di ElevatedButton, lalu run. Apa hasilnya? Jelaskan perbedaan kode langkah 1 dan 4!*

Jawab:

-Perbedaan antara kedua kode tersebut bertujuan untuk menangani kesalahan (error handling) saat melakukan operasi asynchronous, namun mereka menggunakan pendekatan yang berbeda. Kode pertama (returnError) menggambarkan suatu future yang menunda eksekusi selama 2 detik sebelum melempar kesalahan (Exception). Kode kedua (handleError) mencoba mengeksekusi returnError menggunakan blok try-catch. Jika terjadi kesalahan, hasil kesalahan dikonversi menjadi string dan diupdate pada state (result). Blok finally dijalankan setelah blok try-catch selesai, mencetak 'Complete'. Dengan demikian, kode kedua memberikan struktur yang lebih lengkap untuk menangani dan memberikan respons terhadap kesalahan, sedangkan kode pertama lebih fokus pada pembuatan future dengan kesalahan

# Praktikum 6: Menggunakan Future dengan StatefulWidget

### Langkah 1: install plugin geolocator
Tambahkan plugin geolocator dengan mengetik perintah berikut di terminal.
```
flutter pub add geolocator
```

### Langkah 2: Tambah permission GPS
Jika Anda menargetkan untuk platform Android, maka tambahkan baris kode berikut di file android/app/src/main/androidmanifest.xml
```
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>
```

### Langkah 3: Buat file geolocation.dart
Tambahkan file baru ini di folder lib project Anda.

### Langkah 4: Buat StatefulWidget
Buat class LocationScreen di dalam file geolocation.dart

### Langkah 5: Isi kode geolocation.dart
**Soal 11**

*Tambahkan nama panggilan Anda pada tiap properti title sebagai identitas pekerjaan Anda.*
```
import 'package:flutter/material.dart';
import 'package:geolocator/geolocator.dart';

class LocationScreen extends StatefulWidget {
  const LocationScreen({super.key});

  @override
  State<LocationScreen> createState() => _LocationScreenState();
}

class _LocationScreenState extends State<LocationScreen> {

  Future<Position>? position;

  String myPosition = ''; 
  @override
  void initState(){
    super.initState();
    position = getPosition();
    getPosition().then((Position myPos){
      myPosition = 
        'Latitude: ${myPos.latitude.toString()} - longitude: {myPos.longitude.toString()}';
        setState(() {
          myPosition = myPosition;
        });
    });
  }
  @override 
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Rizky Angkata')),
      body: Center(child: FutureBuilder(
        future: position,
        builder: (BuildContext context, AsyncSnapshot<Position>
        snapshot) {
          if (snapshot.connectionState == ConnectionState.waiting) {
            return const CircularProgressIndicator();
          }
          else if (snapshot.connectionState == ConnectionState.done) {
            if (snapshot.hasError){
              return Text('Something terrible happened!');
            }
            return Text(snapshot.data.toString());
          }
          else {
            return const Text('');
          }
        }
      )),
    );
  }

  Future<Position> getPosition() async {
    await Future.delayed(const Duration(seconds: 3));
    await Geolocator.isLocationServiceEnabled();
    Position position = await Geolocator. getCurrentPosition();
    return position;
  }

}
```

### Langkah 6: Edit main.dart
Panggil screen baru tersebut di file main Anda seperti berikut.
```
home: LocationScreen(),
```

### Langkah 7: Run
Run project Anda di device atau emulator (bukan browser), maka akan tampil seperti berikut ini.

![image](https://github.com/JLUNGOOD/mobile_p13-main/assets/106043734/51253118-35cc-4455-9ca4-f07d8868bc4a)


### Langkah 8: Tambahkan animasi loading
Tambahkan widget loading seperti kode berikut. Lalu hot restart, perhatikan perubahannya.
```@override
Widget build(BuildContext context) {
  final myWidget = myPosition == ''
  ? const CircularProgressIndicator()
  : const Text(myPosition);;

  return Scaffold(
    appBar : AppBar(title: Text('Current Location')),
    body: Center(child:myWidget),
  );
  }
```
**Soal 12***

- *Jika Anda tidak melihat animasi loading tampil, kemungkinan itu berjalan sangat cepat. Tambahkan delay pada method getPosition() dengan kode await Future.delayed(const Duration(seconds: 3));*

- *Apakah Anda mendapatkan koordinat GPS ketika run di browser? Mengapa demikian?*

JAWAB :

Tidak

Capture hasil praktikum Anda berupa GIF dan lampirkan di README. Lalu lakukan commit dengan pesan "W12: Soal 12".

![image](https://github.com/JLUNGOOD/mobile_p13-main/assets/106043734/c1140013-4974-4ece-8fe8-787fa749ce7a)


# PRAKTIKUM 7

### Langkah 1: Modifikasi method getPosition()

Buka file geolocation.dart kemudian ganti isi method dengan kode ini.
```
Future<Position> getPosition() async {
    await Future.delayed(const Duration(seconds: 3));
    await Geolocator.isLocationServiceEnabled();
    Position position = await Geolocator. getCurrentPosition();
    return position;
  }
```

### Langkah 2: Tambah variabel
Tambah variabel ini di class _LocationScreenState
```
Future<Position>? position;
```
### Langkah 3: Tambah initState()
Tambah method ini dan set variabel position
```
@override
  void initState(){
    super.initState();
    position = getPosition();
}
```

### Langkah 4: Edit method build()
```
Ketik kode berikut dan sesuaikan. Kode lama bisa Anda comment atau hapus.

@override 
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Rizky Angkata')),
      body: Center(child: FutureBuilder(
        future: position,
        builder: (BuildContext context, AsyncSnapshot<Position>
        snapshot) {
          if (snapshot.connectionState == ConnectionState.waiting) {
            return const CircularProgressIndicator();
          }
          else if (snapshot.connectionState == ConnectionState.done) {
            if (snapshot.hasError){
              return Text('Something terrible happened!');
            }
            return Text(snapshot.data.toString());
          }
          else {
            return const Text('');
          }
        }
      )),
    );
  }
```
**Soal 13**

*Apakah ada perbedaan UI dengan praktikum sebelumnya? Mengapa demikian?*

JAWAB :

Ada, karena menggunakan FutureBuilder lebih efisien, clean, dan reactive dengan Future bersama UI.

- Capture hasil praktikum Anda berupa GIF dan lampirkan di README. Lalu lakukan commit dengan pesan "W12: Soal 13".

- Seperti yang Anda lihat, menggunakan FutureBuilder lebih efisien, clean, dan reactive dengan Future bersama UI.

Hasil :

![image](https://github.com/JLUNGOOD/mobile_p13-main/assets/106043734/1ffcfe75-1f36-4264-b804-59392fccfd6a)


### Langkah 5: Tambah handling error
Tambahkan kode berikut untuk menangani ketika terjadi error. Kemudian hot restart.
```
else if (snapshot.connectionState == ConnectionState.done) {
  if (snapshot.hasError) {
     return Text('Something terrible happened!');
  }
  return Text(snapshot.data.toString());
}
```

**Soal 14**

*Apakah ada perbedaan UI dengan langkah sebelumnya? Mengapa demikian?*

JAWAB :

`Ada

`Capture hasil praktikum Anda berupa GIF dan lampirkan di README. Lalu lakukan commit dengan pesan "W12: Soal 14".

Hasil :

![image](https://github.com/JLUNGOOD/mobile_p13-main/assets/106043734/48d608d1-6a4f-41cb-acef-e2e9483ecbab)

# Praktikum 8: Navigation route dengan Future Function
### Langkah 1: Buat file baru navigation_first.dart
Buatlah file baru ini di project lib Anda.

### Langkah 2: Isi kode navigation_first.dart
```
import 'package:flutter/material.dart';

class NavigationFirst extends StatefulWidget {
  const NavigationFirst({Key? key}) : super(key: key);

  @override
  State<NavigationFirst> createState() => _NavigationFirstState();
}

class _NavigationFirstState extends State<NavigationFirst> {
  Color color = Colors.blue.shade700;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: color,
      appBar: AppBar(
        title: const Text('Rizky Angkata'),
      ),
      body: Center(
        child: ElevatedButton(
          child: const Text('Change Color'),
          onPressed: () async {
             _navigateAndGetColor(context);

            // Update the color if a new color is selected
            if (resultColor != null) {
              setState(() {
                color = resultColor;
              });
            }
          },
        ),
      ),
    );
  }
```

**Soal 15**
- *Tambahkan nama panggilan Anda pada tiap properti title sebagai identitas pekerjaan Anda.*
- *Silakan ganti dengan warna tema favorit Anda.*

### Langkah 3: Tambah method di class _NavigationFirstState
Tambahkan method ini.
```
Future _navigateAndGetColor(BuildContext context) async {
   color = await Navigator.push(context,
        MaterialPageRoute(builder: (context) => const NavigationSecond()),) ?? Colors.blue;
   setState(() {});
   });
}
```
### Langkah 4: Buat file baru navigation_second.dart
Buat file baru ini di project lib Anda. Silakan jika ingin mengelompokkan view menjadi satu folder dan sesuaikan impor yang dibutuhkan.

### Langkah 5: Buat class NavigationSecond dengan StatefulWidget
```
import 'package:flutter/material.dart';

class NavigationSecond extends StatefulWidget {
  const NavigationSecond({Key? key}) : super(key: key);
  @override 
  State<NavigationSecond> createState() => _NavigationSecondState();
}

class _NavigationSecondState extends State<NavigationSecond> {
  @override
  Widget build(BuildContext context) {
    Color color;
    return Scaffold(
      appBar: AppBar(
        title: const Text('Rizky Angkata'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.spaceEvenly,
          children: [
            ElevatedButton(
              child: const Text('Merah'),
              onPressed: () {
                color = Colors.red.shade700;
                Navigator.pop(context, color);
              }),
             ElevatedButton(
              child: const Text('Abu-abu'),
              onPressed: () {
                color = Colors.grey.shade700;
                Navigator.pop(context, color);
              }),
            ElevatedButton(
              child: const Text('Kuning'),
              onPressed: () {
                color = Colors.yellow.shade700;
                Navigator.pop(context, color);
              }),
          ],
        )
      )
    );
  }
}
```

### Langkah 6: Edit main.dart

Lakukan edit properti home.
```
home: const NavigationFirst(),
```
### Langkah 8: Run

Lakukan run, jika terjadi error silakan diperbaiki.

**Soal 16**

*Cobalah klik setiap button, apa yang terjadi ? Mengapa demikian ?*

- JAWAB : Saat button diklik maka akan backround akan ikut berubah mengikuti warna yang telah dipilih

- Gantilah 3 warna pada langkah 5 dengan warna favorit Anda!

- Capture hasil praktikum Anda berupa GIF dan lampirkan di README. Lalu lakukan commit dengan pesan "W12: Soal 16".

Hasil :

![image](https://github.com/JLUNGOOD/mobile_p13-main/assets/106043734/c4607257-b7f3-4d71-ac00-3e883f394341)

![image](https://github.com/JLUNGOOD/mobile_p13-main/assets/106043734/a50f6327-5479-47f0-bd09-2421c43494b4)

# PRAKTIKUM 9

### Langkah 1: Buat file baru navigation_dialog.dart

Buat file dart baru di folder lib project Anda.

### Langkah 2: Isi kode navigation_dialog.dart
```
import 'package:flutter/material.dart';

class NavigationDialogScreen extends StatefulWidget {
  const NavigationDialogScreen({Key? key}) : super(key: key);

  @override
  State<NavigationDialogScreen> createState() => _NavigationDialogScreenState();
}

class _NavigationDialogScreenState extends State<NavigationDialogScreen> {
  Color color = Colors.blue.shade700;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: color,
      appBar: AppBar(
        title: const Text('Navigation Dialog Screen'),
      ),
      body: Center(
        child: ElevatedButton(
          child: const Text('Change Color'),
          onPressed: () {
            _showColorDialog(context);
          },
        ),
      ),
    );
  }
```

### Langkah 3: Tambah method async
```
_showColorDialog(BuildContext context) async {
    Color selectedColor = await showDialog(
      barrierDismissible: false,
      context: context,
      builder: (_) {
        return AlertDialog(
          title: const Text('Very important question'),
          content: const Text('Please choose a color'),
          actions: <Widget>[
            TextButton(
              child: const Text('Red'),
              onPressed: () {
                Navigator.pop(context, Colors.red.shade700);
              },
            ),
            TextButton(
              child: const Text('Green'),
              onPressed: () {
                Navigator.pop(context, Colors.green.shade700);
              },
            ),
            TextButton(
              child: const Text('Blue'),
              onPressed: () {
                Navigator.pop(context, Colors.blue.shade700);
              },
            ),
          ],
        );
      },
    );

    // Set the selected color using setState
    setState(() {
      color = selectedColor;
    });
  }
}
```

### Langkah 4: Panggil method di ElevatedButton
```
onPressed: () {
  _showColorDialog(context);
}),
```
### Langkah 5: Edit main.dart
Ubah properti home
```
home : const NavigationDialog(),
```
### Langkah 6: Run

Coba ganti warna background dengan widget dialog tersebut. Jika terjadi error, silakan diperbaiki. Jika berhasil, akan tampil seperti gambar berikut.

Hasil :

![image](https://github.com/JLUNGOOD/mobile_p13-main/assets/106043734/68c6e25e-6a21-4fad-bb05-29d5bdf65355)

**Soal 17**

*Cobalah klik setiap button, apa yang terjadi ? Mengapa demikian ?*

JAWAB :

- Saat button diklik maka akan backround akan ikut berubah mengikuti warna yang telah dipilih

- Gantilah 3 warna pada langkah 3 dengan warna favorit Anda!

JAWAB :
```
return AlertDialog(
        title: const Text('Very important question'),
        content: const Text('Please choose a color'),
        actions: <Widget>[
          TextButton(
            child: const Text('Red'),
            onPressed: () {
              Navigator.pop(context, Colors.orange.shade700);
            },
          ),
          TextButton(
            child: const Text('Green'),
            onPressed: () {
              Navigator.pop(context, Colors.pink.shade700);
            },
          ),
          TextButton(
            child: const Text('Blue'),
            onPressed: () {
              Navigator.pop(context, Colors.grey.shade700);
            },
          ),
        ],
      );
```
- Capture hasil praktikum Anda berupa GIF dan lampirkan di README. Lalu lakukan commit dengan pesan "W12: Soal 17".

Hasil :

![image](https://github.com/JLUNGOOD/mobile_p13-main/assets/106043734/6de6cf75-f325-4ea5-8b41-5eccfc8702fe)

