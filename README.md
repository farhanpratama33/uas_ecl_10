Uas_E-Commerce-Lanjut
Nama : Farhan Pratama Azhuri,Kelas : ECL-10, Nim : 10515153
Fungsi web program untuk mengatur data seperti data siswa kelas dll seperti fungsi input(create),view(read),edit(updating).hapus(delete)
Cara Pengunaan Import database db_laravel_siswa ke php my admin lalu login Username : admin Password : admin


Step by step penyusunan crud laravel 
A.Instalasi media pemrograman laravel Menginstall Dan Konfigurasi sublime text sebagai media untuk menyusun dan membuat struktur program yang akan dibuat Menginstall Dan Konfigurasi Xampp sebagai database server local pada komputer yang akan dijadikan media untuk menyusun program dengan framework laravel. Untuk melakukan instalasi Laravel melalui Composer, install terlebih dahulu Composer, bisa didapatkan dari https://getcomposer.org/Composer-Setup.exe Masuk ke folder htdocs yang akan dijadikan folder untuk program laravel, lalu tekan Shift + Klik Kanan pada sembarang tempat, pilih Open command windows here untuk membuk command prompt pada folder htdocs. Ketik composer create-project --prefer-dist laravel/laravel belajarLaravel "5.7.*" Proses instalasi Laravel akan berlangsung. Pastikan perangkat yang akan dijadikan media koding laravel terkoneksi dengan internet karena composer akan mengambil package-package laravel nya secara online. Laravel akan di install pada folder belajarLaravel.


B. Integrasi Laravel dengan Template 

Untuk dapat menerapkan template pada laravel contohnya dalam program laravel ini menggunakan template admin lte maka bisa di unduh pada link https://codeload.github.com/almasaeed2010/AdminLTE/zip/v2.3.11 Buat folder assets di folder public laravel. Copy folder dist, bootstrap dan plugins dari template AdminLTE ke folder tersebut. Buat folder baru dengan nama templates pada folder resources/views Didalam folder templates, buat 2 file dengan nama header.blade.php dan footer.blade.php Buat folder baru dengan nama kelas pada folder resources/views di laravel. Didalam folder kelas, buat file dengan nama index.blade.php Buka folder AdminLTE-2.3.11, kemudian cari file blank.html di folder pages/examples Buka Command Prompt (dengan cara tekan Shift + Klik Kanan di sembarang tempat, pilih Open windows command windows here) di folder belajarLaravel, buat Controller baru menggunakan artisan dengan syntax berikut:
php artisan make:controller KelasController 3.Integrasi CRUD – READ DATA Membuat Tabel dengan Migration : 1. Setting database dan setting juga app_url di pada .env (Di root folder project laravel)
Buat database baru dengan nama db_laravel_siswa di phpmyadmin / aplikasi RDBMS yang akan digunakan
Membuat migration baru untuk membuat tabel t_kelas.
php artisan make:migration create_table_kelas
Buka file create_table_kelas.php lalu masukan sintaks yang akan dibutuhkan Buka kembali command and prompt lalu klik > php artisan migrate


C. Menampilkan data di views 

Setelah database, tabel dan data sudah dibuat, sekarang buat tampilan dalam bentuk tabel untuk menampilkan data-data tersebut di web yang akan dibuat. Ikuti langkah berikut: 1. Buka command prompt, ketik sintaks berikut untuk membuat Model
php artisan make:model Kelas 2.Buka file app/Kelas.php kemudian input sintaks yang dibutuhkan 3.Buka file app/Http/Controllers/KelasController.php, kemudian modifikasi sintaks tersebut 4.Buka file resources/views/kelas/index.blade.php, kemudian modifikasi sintaks sintaks dalam file tersebut


D. Integrasi CRUD – Create Data 

Pada pembahasan kali ini akan dijelaskan tentang bagaimana untuk menginput data kedalam database dengan Laravel Eloquent. Ikuti langkah-langkah berikut ini:
Buat file baru di resources/views/kelas/form.blade.php (Copy dari index.blade.php).
Modifikasi file resources/views/kelas/form.blade.php untuk membuat form dengan sintaks yang akan dibutuhkan
Buka file KelasController.php, tambahkan sintaks yang akan dibutuhkan public function create() { return view('kelas/form'); } public function store(Request $request) {
$rules = [ 'nama_kelas' => 'required|max:100', 'jurusan' => 'required|max:100' ]; $this->validate($request, $rules); $input = $request->all(); $status = \App\Kelas::create($input); if ($status) return redirect('/')->with('success', 'Data Berhasil Ditambahkan'); else return redirect('/')->with('error', 'Data Gagal Ditambahkan'); }
Buka file routes/web.php, tambahkan sintaks yang akan dibutuhkan Route::get('kelas/add', 'KelasController@create'); Route::post('kelas/add', 'KelasController@store');


E. Integrasi CRUD – Updating Data

Setelah berhasil melakukan langkah-langkah diatas, lalu mencoba untuk melakukan editing data. Form yang digunakan masih sama dengan form yang dipakai untuk melakukan input data, hanya saja menambahkan logika untuk melakukan cek apabila terdapat data $result, maka artinya form tersebut dalam mode Edit, jika tidak terdapat data, maka form tersebut dalam mode Input.
Buka file resources/views/kelas/form.blade.php, lakukan modifikasi dengan sintaks sintaks yang akan dibutuhkan
2 Buka file app/Http/Controllers/KelasController.php, tambahkan sintaks untuk fungsi edit dan update: public function edit($id) { $data['result'] = \App\Kelas::where('id_kelas', $id)->first(); return view('kelas/form')->with($data); }
public function update(Request $request, $id)
{
    $rules = [
        'nama_kelas'    => 'required|max:100',
        'jurusan'       => 'required|max:100'
    ];
    $this->validate($request, $rules); 

    $input = $request->all();
    $result = \App\Kelas::where('id_kelas', $id)->first();
    $status = $result->update($input);

    if ($status) return redirect('/')->with('success', 'Data Berhasil Diubah');
    else return redirect('/')->with('error', 'Data Gagal Diubah');
}
Buka file routes/web.php, tambahkan sintaks yang akan dibutuhkan Route::get('siswa/{id}/edit', 'SiswaController@edit'); Route::patch('siswa/{id}/edit', 'SiswaController@update');


F. Integrasi CRUD – Delete Data

Pada bagian ini akan dijelaskan langkah-langkah untuk menghapus data, pada tahap sebelumnya sudah membuat tombol untuk hapus dengan menggunakan form dan method field DELETE. Ikuti langkah-langkah berikut ini:
Buka file app/Http/Controllers/KelasController.php, tambahkan sintaks untuk fungsi destroy public function destroy(Request $equest, $id) { $result = \App\Kelas::where('id_kelas', $id)->first(); $status = $result->delete();
if ($status) return redirect('/')->with('success', 'Data Berhasil Dihapus');
else return redirect('/')->with('error', 'Data Gagal Dihapus');
}
