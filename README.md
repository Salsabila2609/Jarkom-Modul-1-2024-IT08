# **Kelompok IT08**

## Anggota

- Salsabila Amalia Harjanto (5027221023)
- Ditya Wahyu ()

## Laporan Resmi Modul 1

#### Soal 1: ATM or ATP or FTP ? 🤔
Pradityo mencoba mengembangkan server ftp, tetapi seseorang mencoba melakukan bruteforce login, bisakah Anda menganalisis apa yang terjadi?

- Gunakan filter untuk packet details, setting filter menggunakan format string, lalu input `Login successful` karena perlu mencari password yang berhasil digunakan untuk login
Ditemukan packet details pada stream 319 berisi:
```
USER adminJarkom
331 Please specify the password.
PASS m4y_th3_Kn!fe_ch1p_&_sh4tter
230 Login successful.
```
- Input password tersebut ke terminal dan mendapatkan flag

#### Soal 2: How Many Packets ?
Sebagai kewajiban untuk laporan, aku diminta untuk mencari tahu berapa kali attempt login yang dilakukan oleh hacker. Dapatkah kamu membantuku untuk menganalisanya?

- Untuk stream 1-304, terdapat 3 attempt login di tiap stream
- Untuk stream 305-311, terdapat 2 attempt login di tiap stream
- Untuk stream 312-318, terdapat 1 attempt login di tiap stream
- Untuk stream 319, terdapat 1 attempt login yang sukses
- Dilakukan perhitungan manual: `304x3 + 7x2 + 7x1 + 1 = 934`. Input hasil ke terminal dan mendapatkan flag

#### Soal 3: Trace him
Selain menghitung jumlah packet, coba lacak juga ip penyerang tersebut!

- Gunakan filter untuk packet details, setting filter menggunakan format string, lalu input `Login incorrect` karena perlu mencari ip attacker yang melakukan login attempt
Ditemukan ip attacker yaitu `10.30.3.4` (10.15.40.20 merupakan ip server, jadi ip attacker sudah pasti yang satunya)
- Input ip tersebut ke terminal dan mendapatkan flag

#### Soal 4: Creds
Attacker menyadari jika dia bisa membuat clone ftp server dari target, temukan kredensial dari server ftp yang dibuat oleh attacker

- Gunakan filter untuk packet details, setting filter menggunakan format string, lalu input `Successful` karena perlu mencari username dan password yang berhasil digunakan untuk login
Ditemukan packet details pada stream 2 berisi:
```
530 Please login with USER and PASS.
USER h3ngk3rTzy
331 Please specify the password.
PASS S!l3ncE
230 Login successful.
```
- Input user dan pass tersebut ke terminal dan mendapatkan flag

#### Soal 5: malwleowleo
Dapatkah kamu menemukan file malware yang dikirim oleh attacker melalui ftp?

- Pada stream 2 yang berisi user dan password login attacker pada soal creds, ditemukan pengiriman file malware oleh attacker
Ini ditemukan pada bagian:
```
227 Entering Passive Mode (10,15,40,20,82,14).
STOR m4L1c10us_W4re.c
150 Ok to send data.
226 Transfer complete.
```
- Input nama file ke terminal dan mendapatkan flag

#### Soal 6: whoami
Dapatkah kamu menemukan siapa identitas attacker?

- Follow TCP stream untuk melihat seluruh packet details, disini ditemukan hal menarik dalam format base64 pada stream 7
- Melakukan decode format base64
Hasil decode menunjukan identitas attacker yaitu `Paul Atreides`
- Input nama tersebut ke terminal dan mendapatkan flag

#### Soal 7: secret
Temukan pesan rahasia dari attacker.

- Follow TCP stream untuk melihat seluruh packet details, disini ditemukan bahwa ada file lain selain dari file malware, dengan nama file `mirza.jpg`
- Untuk membuka file, gunakan app FileZilla, input host `10.15.40.20`, dan user serta password yang ditemukan di soal creds
- Buka file `mirza.jpg`
Ditemukan pesan rahasia pada gambar, yaitu `MIO MIRZA`
- Input ke terminal dan mendapatkan flag

#### Soal 8: evidence
Perusahaan nanomate baru saja kebobolan. Mereka menyewamu untuk mencari tahu bagaimana caranya pelaku bisa masuk.

- Gunakan filter untuk packet details, setting filter menggunakan format string, lalu input `Login` karena perlu mencari tahu bagaimana pelaku masuk
Pada stream 1240 ditemukan status `Login successful`. 
```
POST /app/includes/process_login.php HTTP/1.1
Host: nanomate-solutions.com

email=tareq@gmail%2ecom&password=tareq@nanomate&submit_btn=SIGN+INHTTP/1.1 302 Found
Date: Tue, 18 Jul 2023 22:01:38 GMT
Server: Apache/2.4.56 (Unix) OpenSSL/1.1.1t PHP/8.0.28 mod_perl/2.0.12 Perl/v5.34.1
```
Analisa untuk menemukan jawaban.

path - `/app/includes/process_login.php HTTP/1.1`

host - `nanomate-solutions.com`

server - `apache-2.4.56`

email:pass - `tareq@gmail.com:tareq@nanomate`

- Input ke terminal dan mendapatkan flag

#### Soal 9: fuzz
My website got hacked. Can you analyze this network traffic to help me track the attacker?

- Gunakan filter dan find keyword `Login` untuk menemukan percobaan login oleh attacker.
Ditemukan username dan password dengan status 302 Found. Analisa packet details untuk menemukan jawaban tiap soal.

port - `80` karena `http`

path - `/`

toolsname-version - `ffuf-v2.0.0-dev`

username:password - `admin:sUp3rSecretp@ssw0rd`

- Input ke terminal dan mendapatkan flag
