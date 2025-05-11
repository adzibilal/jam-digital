**Jam Alarm Digital - KELOMPOK 5**

- Adzi Bilal M H
- Aprila Nurindah Zulfany
- Ghina Nurul Ardhiani

Jam alarm digital ini menampilkan waktu pada tampilan 7-segment 4-digit. Berikut adalah fitur-fiturnya:

- Dua tombol untuk mengatur waktu saat ini (Jam / Menit)
- Alarm yang dapat diprogram dengan fungsi snooze
- Suara alarm dapat dengan mudah disesuaikan dalam kode, dan bahkan memainkan melodi
- Simbol titik dua berkedip untuk menunjukkan detik
- RTC opsional untuk membuat jam lebih akurat
- Pemulihan setelah kehilangan daya: waktu saat ini dan pengaturan alarm disimpan di RTC


**Cara Menggunakan Jam**

- Untuk mengatur waktu, tekan tombol _Menit_/_Jam_.

- Menekan tombol _Alarm_ mengaktifkan/mematikan alarm. Layar akan menampilkan status alarm dengan menunjukkan kata "on" atau "off".

- Setelah mengaktifkan alarm, waktu alarm saat ini akan ditampilkan selama beberapa detik. Anda dapat menggunakan tombol _Menit_/_Jam_ untuk menyesuaikan waktu alarm. Untuk menyelesaikan, tekan tombol _Alarm_ lagi, atau cukup tunggu beberapa detik.

- Ketika alarm berbunyi, tekan tombol _Alarm_ sebentar untuk menunda alarm selama 9 menit. Layar akan menampilkan empat lingkaran untuk memberi tahu Anda bahwa alarm telah ditunda.

- Untuk menghentikan alarm, tahan tombol _Alarm_ selama satu detik atau lebih.


**Struktur Proyek**

Kode dibagi menjadi beberapa modul:

1. [config.h](#source-config_h) - Opsi konfigurasi untuk jam: apakah menggunakan chip RTC, waktu snooze, dan lainnya. Menggunakan file konfigurasi adalah praktik umum ketika Anda memiliki beberapa modul dalam program Anda.
2. [alarm-clock.ino](#source-alarm_clock_ino) - Kode program utama. Ini mengelola antarmuka pengguna: tampilan 7-segment dan tombol.
3. [Clock](#source-clock_h) - Kelas `Clock` mengelola waktu saat ini dan mesin status alarm. Ini menggunakan pustaka [RTClib](https://github.com/adafruit/RTClib) untuk berkomunikasi dengan chip RTC dan melacak waktu.
4. [AlarmTone](#source-clock_h) - Kelas `AlarmTone` memainkan suara alarm. Anda dapat mengubah nilai array `TONES`, `TONE_TIME`, dan `TONE_SPACING` untuk menyesuaikan alarm dan memainkan nada serta melodi yang berbeda.


**Perangkat Keras**

| Item              | Jumlah | Catatan                               |
| ----------------- | ------ | ------------------------------------- |
| Arduino Uno R3    | 1      |                                       |
| 4-Digit 7-Segment | 4      | Anoda Umum, 14 pin                   |
| Resistor 220Ω     | 8      | Hubungkan ke pin segmen 7-segment    |
| Transistor PNP    | 4      | Opsional, disarankan                 |
| Resistor 4.7kΩ    | 4      | Jika Anda menggunakan transistor PNP  |
| Tombol Push 12mm  | 3      |                                       |
| Buzzer Piezo      | 1      | Digunakan untuk alarm                 |
| DS1307 RTC        | 1      | Opsional                              |

- Anda juga dapat menggunakan tampilan 7-segment Katoda Umum, cukup sesuaikan konstanta `DISPLAY_TYPE` di [config.h](#source-config_h), dan beralih ke transistor NPN.

Untuk menjaga perangkat keras jam tetap minimal, Arduino mengontrol tampilan 7-Segment secara langsung, menggunakan pustaka [SevSeg](https://www.arduinolibraries.info/libraries/sev-seg). Namun, pendekatan ini memiliki kelemahan: menggunakan 12 pin GPIO!

Jika Anda ingin menghemat pin Arduino, Anda dapat menggunakan register geser _74HC595_ untuk mengurangi penggunaan pin menjadi 6, atau bahkan tampilan 7-Segment dengan chip pengontrol terintegrasi, seperti _TM1637_, _HT16K33_, atau _MAX7219_. Dalam hal ini, Anda perlu mengubah kode untuk menggunakan pustaka tampilan yang berbeda (pustaka _SevSeg_ tidak mendukung kasus ini), tetapi ini di luar cakupan proyek ini.


**Koneksi Pin**

| Pin Arduino Uno | Perangkat         | Pin Perangkat |
| ---------------- | ----------------- | ------------- |
| 2                | 7-Segment         | 14 (Dig 1)    |
| 3                | 7-Segment         | 11 (Dig 2)    |
| 4                | 7-Segment         | 10 (Dig 3)    |
| 5                | 7-Segment         | 6 (Dig 4)     |
| 6                | 7-Segment         | 13 (A)        |
| 7                | 7-Segment         | 9 (B)        |
| 8                | 7-Segment         | 4 (C)        |
| 9                | 7-Segment         | 2 (D)        |
| 10               | 7-Segment         | 1 (E)        |
| 11               | 7-Segment         | 12 (F)       |
| 12               | 7-Segment         | 5 (G)        |
| 13               | 7-Segment         | 8 (Colon)    |
| A0               | Tombol Jam        | -            |
| A1               | Tombol Menit      | -            |
| A2               | Tombol Alarm      | -            |
| A3               | Buzzer / Speaker  | -            |
| A4               | DS1307 RTC        | SDA          |
| A5               | DS1307 RTC        | SCL          |

- Nomor pin untuk tampilan 7-segment Anda mungkin berbeda. Silakan konsultasikan datasheet yang relevan untuk perangkat Anda untuk menemukan nomor pin yang sesuai.