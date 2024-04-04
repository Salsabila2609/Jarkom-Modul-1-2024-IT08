# **Kelompok IT08**

## Anggota

- Salsabila Amalia Harjanto (5027221023)
- Ditya Wahyu ()

## Laporan Resmi Modul 1

### Soal 1: ATM or ATP or FTP ? 🤔
Pradityo mencoba mengembangkan server ftp, tetapi seseorang mencoba melakukan bruteforce login, bisakah Anda menganalisis apa yang terjadi?

- Gunakan filter `ftp.response.code == 230` untuk menemukan user yang berhasil login
![image](https://github.com/Salsabila2609/Jarkom-Modul-1-2024-IT08/assets/128382995/8e504fda-bae3-4a24-936c-7eaf58ff68d1)

- Buka packet details dan ditemukan username serta password yang dicari
![image](https://github.com/Salsabila2609/Jarkom-Modul-1-2024-IT08/assets/128382995/87fda6be-3e92-4fa9-8aa4-aab7a6f1ae51)

Ditemukan packet details pada stream 319 berisi:
```
USER adminJarkom
331 Please specify the password.
PASS m4y_th3_Kn!fe_ch1p_&_sh4tter
230 Login successful.
```
- Input password tersebut ke terminal dan mendapatkan flag
![image](https://github.com/Salsabila2609/Jarkom-Modul-1-2024-IT08/assets/128382995/9d779c9b-0e70-42c9-8092-af364d43ab16)

---

### Soal 2: How Many Packets ?
Sebagai kewajiban untuk laporan, aku diminta untuk mencari tahu berapa kali attempt login yang dilakukan oleh hacker. Dapatkah kamu membantuku untuk menganalisanya?

- Gunakan filter untuk packet details, setting filter menggunakan format string, lalu input `Login`, kemudian klik packet list yang menunjukan status `Login incorrect` dan apply as filter
![image](https://github.com/Salsabila2609/Jarkom-Modul-1-2024-IT08/assets/128382995/859621d7-4516-48c3-a872-229d232d72ae)

- Ditemukan total displayed 933, ditambah 1 yang berhasil login, jadi total = `934`, masukkan ke terminal dan mendapatkan flag.. Input hasil ke terminal dan mendapatkan flag
![image](https://github.com/Salsabila2609/Jarkom-Modul-1-2024-IT08/assets/128382995/7266d39f-b0c1-4e55-8494-daf91b1afeca)

---

### Soal 3: Trace him
Selain menghitung jumlah packet, coba lacak juga ip penyerang tersebut!

- Gunakan filter untuk packet details, setting filter menggunakan format string, lalu input `Login incorrect` karena perlu mencari ip attacker yang melakukan login attempt
![image](https://github.com/Salsabila2609/Jarkom-Modul-1-2024-IT08/assets/128382995/0c116008-487b-44f3-847b-e0c242ae025e)

Ditemukan ip attacker yaitu `10.30.3.4` (10.15.40.20 merupakan ip server, jadi ip attacker sudah pasti yang satunya)

- Input ip tersebut ke terminal dan mendapatkan flag
![image](https://github.com/Salsabila2609/Jarkom-Modul-1-2024-IT08/assets/128382995/aab40a5d-00db-468a-bb32-556ecf351f8c)

---

### Soal 4: Creds
Attacker menyadari jika dia bisa membuat clone ftp server dari target, temukan kredensial dari server ftp yang dibuat oleh attacker

- Gunakan filter `ftp.response.code == 230` untuk menemukan user yang berhasil login
![image](https://github.com/Salsabila2609/Jarkom-Modul-1-2024-IT08/assets/128382995/cc2f6ef3-d83f-45e1-9028-f610ea9a80cd)

- Buka packet list untuk menemukan informasi detail mengenai username dan password
![image](https://github.com/Salsabila2609/Jarkom-Modul-1-2024-IT08/assets/128382995/eb54d946-a470-4024-9f1e-bf7f5ef49a7d)

Ditemukan packet details pada stream 2 berisi:
```
530 Please login with USER and PASS.
USER h3ngk3rTzy
331 Please specify the password.
PASS S!l3ncE
230 Login successful.
```
- Input user dan pass tersebut ke terminal dan mendapatkan flag
![Screenshot 2024-04-03 165358](https://github.com/Salsabila2609/Jarkom-Modul-1-2024-IT08/assets/128382995/c688c3b7-847d-4cc4-800e-ff3b7290281a)

---

### Soal 5: malwleowleo
Dapatkah kamu menemukan file malware yang dikirim oleh attacker melalui ftp?

- Pada stream 2 yang berisi user dan password login attacker pada soal creds, ditemukan pengiriman file malware oleh attacker
![image](https://github.com/Salsabila2609/Jarkom-Modul-1-2024-IT08/assets/128382995/91bd9930-6a38-4d44-8c16-bd637bf7d491)

Ini ditemukan pada bagian:
```
227 Entering Passive Mode (10,15,40,20,82,14).
STOR m4L1c10us_W4re.c
150 Ok to send data.
226 Transfer complete.
```
- Input nama file ke terminal dan mendapatkan flag
![image](https://github.com/Salsabila2609/Jarkom-Modul-1-2024-IT08/assets/128382995/6ac7ed59-5f04-48fc-84e2-b07f0be957e9)

---

### Soal 6: whoami
Dapatkah kamu menemukan siapa identitas attacker?

- Follow TCP stream untuk melihat seluruh packet details, disini ditemukan hal menarik dalam format base64 pada stream 7
![image](https://github.com/Salsabila2609/Jarkom-Modul-1-2024-IT08/assets/128382995/56630c89-1c51-4abb-86d8-7aca6d56ca29)

- Melakukan decode format base64
![image](https://github.com/Salsabila2609/Jarkom-Modul-1-2024-IT08/assets/128382995/4bc53aa3-6f7f-40bf-97f6-cd9da2c1526d)

Hasil decode menunjukan identitas attacker yaitu `Paul Atreides`
- Input nama tersebut ke terminal dan mendapatkan flag
![image](https://github.com/Salsabila2609/Jarkom-Modul-1-2024-IT08/assets/128382995/25d0a5d3-a81b-48ef-9c6f-b99affe1dfe9)

---

### Soal 7: secret
Temukan pesan rahasia dari attacker.

- Follow TCP stream untuk melihat seluruh packet details, disini ditemukan bahwa ada file lain selain dari file malware, dengan nama file `mirza.jpg`
![image](https://github.com/Salsabila2609/Jarkom-Modul-1-2024-IT08/assets/128382995/e2332f2f-29a8-4006-9cac-0e5965f3b3ad)

- Untuk membuka file, gunakan app FileZilla, input host `10.15.40.20`, dan user serta password yang ditemukan di soal creds
![image](https://github.com/Salsabila2609/Jarkom-Modul-1-2024-IT08/assets/128382995/3f90f011-8f38-4513-84ab-dad66911ba42)

- Buka file `mirza.jpg`
![image](https://github.com/Salsabila2609/Jarkom-Modul-1-2024-IT08/assets/128382995/46c2a8a1-b2df-490e-8b15-bd28793b0335)
Ditemukan pesan rahasia pada gambar, yaitu `MIO MIRZA`

- Input ke terminal dan mendapatkan flag
![image](https://github.com/Salsabila2609/Jarkom-Modul-1-2024-IT08/assets/128382995/86555b49-e6cf-4a11-ab20-b910e1c09788)

---

### Soal 8: evidence
Perusahaan nanomate baru saja kebobolan. Mereka menyewamu untuk mencari tahu bagaimana caranya pelaku bisa masuk.

- Gunakan filter untuk packet details, setting filter menggunakan format string, lalu input `Login` karena perlu mencari tahu bagaimana pelaku masuk
![image](https://github.com/Salsabila2609/Jarkom-Modul-1-2024-IT08/assets/128382995/7c002aa5-0508-415b-b5c0-eed15a2240ad)

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
![image](https://github.com/Salsabila2609/Jarkom-Modul-1-2024-IT08/assets/128382995/7c002aa5-0508-415b-b5c0-eed15a2240ad)

---

### Soal 9: fuzz
My website got hacked. Can you analyze this network traffic to help me track the attacker?

- Gunakan filter dan find keyword `Login` untuk menemukan percobaan login oleh attacker.
Ditemukan username dan password dengan status 302 Found. Analisa packet details untuk menemukan jawaban tiap soal.
![image](https://github.com/Salsabila2609/Jarkom-Modul-1-2024-IT08/assets/128382995/bc5de804-a346-4961-9bbb-151365d6f1a0)

port - `80` karena `http`

path - `/`

toolsname-version - `ffuf-v2.0.0-dev`

username:password - `admin:sUp3rSecretp@ssw0rd`

- Input ke terminal dan mendapatkan flag
![image](https://github.com/Salsabila2609/Jarkom-Modul-1-2024-IT08/assets/128382995/56cbf488-195d-4c02-9348-119ec7586ada)

---
