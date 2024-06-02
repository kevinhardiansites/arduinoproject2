# Sistem Pengukur Suhu dan Kelembaban Menggunakan Mikrokontroler ATtiny85

## Deskripsi Proyek
Proyek ini adalah sebuah sistem pengukur suhu dan kelembaban yang menggunakan mikrokontroler ATtiny85, sensor DHT22, dan layar OLED kecil untuk menampilkan data. Sistem ini dirancang untuk mengukur dan menampilkan suhu dan kelembaban secara real-time.

## Tujuan
Merancang sebuah sistem yang mampu mengukur suhu dan kelembaban dengan akurat menggunakan sensor DHT22.

## Fitur

1. Pengukuran Suhu dan Kelembaban: Menggunakan sensor DHT22 untuk membaca data suhu dan kelembaban lingkungan.
2. Tampilan Data: Menampilkan data suhu dalam derajat Celsius dan kelembaban dalam persen di layar OLED.
3. Animasi Detak Jantung: Menampilkan animasi detak jantung sebagai indikator visual tambahan.
4. Splash Screen: Menampilkan layar sambutan saat perangkat dinyalakan.
5. Pembaruan Data: Memperbarui data suhu dan kelembaban setiap detik.

## Alat dan Bahan
1. ATtiny85: Mikrokontroler yang digunakan untuk mengontrol seluruh sistem.
2. DHT22: Sensor suhu dan kelembaban yang memberikan data suhu dan kelembaban dengan akurasi tinggi.
3. OLED Display digunakan untuk menampilkan data suhu dan kelembaban.
4. Simulator Arduino Wokwi

## Gambar Rangkaian
![alt text](https://github.com/kevinhardiansites/arduinoproject2/blob/main/Daftar%20Gambar/dalam%20keadaan%20mati.png?raw=true)

Link Simulasi: https://wokwi.com/projects/399569563109358593

## Deskripsi Program

Library yang digunakan

```cpp
#include <dht.h>
#include <TinyWireM.h>
#include <Tiny4kOLED.h>
```

```cpp
#define DHT22_PIN PB1 //Mendefinisikan pin PB1 pada ATtiny85 sebagai pin untuk sensor DHT22.
```

Data Gambar (Hati & Termometer)
```cpp
const unsigned char  img_heart_small[] PROGMEM = {
  0x00, 0x00, 0xc0, 0xe0, 0xe0, 0xe0, 0xc0, 0x80, 0x80, 0x80, 0xc0, 0xe0, 0xe0, 0xe0, 0xc0, 0x00, 0x00, 0x00, 0x00, 0x00, 0x01, 0x03, 0x07, 0x0f, 0x1f, 0x3f, 0x1f, 0x0f, 0x07, 0x03, 0x01, 0x00, 0x00, 0x00
}; //Gambar Hati Kecil

const unsigned char  img_heart_big[] PROGMEM = {
  0xe0, 0xf0, 0xf8, 0xf8, 0xf8, 0xf8, 0xf0, 0xe0, 0xe0, 0xe0, 0xf0, 0xf8, 0xf8, 0xf8, 0xf8, 0xf0, 0xe0, 0x00, 0x01, 0x03, 0x07, 0x0f, 0x1f, 0x3f, 0x7f, 0xff, 0x7f, 0x3f, 0x1f, 0x0f, 0x07, 0x03, 0x01, 0x00
}; //Gambar Hati Besar

const unsigned char  img_thermometer[] PROGMEM = {
  0x00, 0xfe, 0x03, 0xfe, 0x50,
  0x00, 0xff, 0x00, 0xff, 0x55,
  0x60, 0x9f, 0x80, 0x9f, 0x65,
}; //Gambar Termometer
```
```cpp
dht DHT; //Mengakses fungsi library DHT
```

Fungsi Splash
```cpp
// Fungsi untuk menampilkan layar splash
void splash() {
  oled.clear();
  oled.setCursor(15, 1);
  oled.print(F("Hi Human"));
  oled.setCursor(35, 3);
  oled.print(F("I'll show you"));
  oled.setCursor(35, 5);
  oled.print(F("Temperature and"));
  oled.setCursor(35, 7);
  oled.print(F("Humidity"));
}
```

Fungsi Heartbeat
```cpp
// Fungsi untuk menampilkan animasi detak jantung di layar OLED
void heartBeat() {
  static char big = 1;
  static long startTime = 0;
  long currentTime;
  currentTime = millis();
  if ((currentTime - startTime) > 200) {
    startTime = currentTime;
    big = 1 - big;
    if (big) {
      oled.bitmap(20, 4, 37, 6, img_heart_big);
    } else {
      oled.bitmap(20, 4, 37, 6, img_heart_small);
    }
  }
}
```

Fungsi Menyiapkan Tampilan
```cpp
// Fungsi untuk menyiapkan tampilan layar OLED dengan teks dan gambar
void prepareDisplay() {
  oled.clear();
  oled.begin();
  oled.setCursor(18, 0);
  oled.print(F("Kevin Hardian"));
  oled.setCursor(1, 2);
  oled.print(F("Temperature-Humidity"));
  oled.bitmap(105, 4, 110, 7, img_thermometer);
  oled.setCursor(57, 4);
  oled.print(F("24.0C"));
  oled.setCursor(57, 5);
  oled.print(F("40.0%"));
  oled.bitmap(10, 5, 17, 2, img_heart_small);
}
```

Fungsi Mendapatkan Suhu dan Kelembaban
```cpp
float getTemperature() {
  return DHT.temperature;
} //Mengembalikan suhu yang dibaca dari sensor DHT22

float getHumidity() {
  return DHT.humidity;
} //Mengembalikan kelembaban yang dibaca dari sensor DHT22
```

Fungsi Setup
```cpp
void setup() {
  pinMode(DHT22_PIN, INPUT);
  oled.begin(128, 64, sizeof(tiny4koled_init_128x64br), tiny4koled_init_128x64br);
  oled.setFont(FONT6X8);
  oled.clear();
  oled.on();
  splash();
  delay(3000);
  prepareDisplay();
} // Dijalankan sekali saat perangkat dinyalakan, untuk inisialisasi pin, layar OLED, dan menampilkan splash screen.
'''

Fungsi Loop
```cpp
void loop() {
  static long startTime = 0;
  long currentTime;
  DHT.read22(DHT22_PIN);
  currentTime = millis();
  if ((currentTime - startTime) > 1000) {
    startTime = currentTime;
    float temperature = getTemperature();
    oled.setCursor(57, 4);
    oled.print(temperature, 1);
    oled.print("C  ");
    float humidity = getHumidity();
    oled.setCursor(57, 5);
    oled.print(humidity, 1);
    oled.print("%  ");
    oled.bitmap(105, 4, 110, 7, img_thermometer);
  }
  heartBeat();
} //Fungsi loop akan dijalankan terus menerus, membaca data sensor DHT22 setiap detik dan menampilkan nilai suhu dan kelembaban di layar OLED, serta menampilkan animasi detak jantung
```

## Cara Kerja Program

1. Saat simulasi dijalankan, Layar OLED akan menampilkan pesan sambutan fungsi splash.
![alt text](https://github.com/kevinhardiansites/arduinoproject2/blob/main/Daftar%20Gambar/dalam%20keadaan%20hidup.png?raw=true)

2. Lalu setelahnya akan menampilkan nilai suhu dan kelembapan disekitar.
![alt text](https://github.com/kevinhardiansites/arduinoproject2/blob/main/Daftar%20Gambar/pengaturan%20input%20sensor.png?raw=true)

3. Sensor DHT22 membaca suhu dan kelembaban dari lingkungan sekitar. Data ini diambil setiap detik pada fungsi loop(). Kita dapat mengubah-ubah input suhu dan kelembapan pada simulasi dengan menggeser-geser slider.

https://github.com/kevinhardiansites/arduinoproject2/assets/141954008/30e1883e-07e6-443e-86c4-d14053204efe
  
4. Data suhu dan kelembaban diperbarui setiap detik pada posisi yang telah ditentukan di layar OLED.

## Kesimpulan
Proyek ini berhasil mengintegrasikan mikrokontroler ATtiny85 dengan sensor DHT22 dan layar OLED untuk mengukur dan menampilkan suhu serta kelembaban lingkungan. Dengan menampilkan data suhu dan kelembaban secara real-time serta menambahkan animasi detak jantung, proyek ini menciptakan antarmuka pengguna yang tidak hanya informatif tetapi juga menarik.

## Saran
1. Perlu dilakukan kalibrasi berkala sensor DHT22 untuk meningkatkan akurasi pengukuran. Tambahkan juga sensor kalibrasi eksternal yang dapat memberikan pembacaan yang lebih akurat dan membandingkan hasilnya untuk melakukan penyesuaian.
2. Penambahan modul penyimpanan seperti SD card atau EEPROM untuk menyimpan data suhu dan kelembaban secara periodik. Hal ini memungkinkan analisis data historis dan pemantauan tren perubahan lingkungan dalam jangka waktu yang lebih panjang.
3. Penambahan sensor-sensor lain seperti sensor kualitas udara (CO2, VOC), sensor tekanan atmosfer, atau sensor cahaya. Ini akan membuat perangkat lebih serbaguna dalam pemantauan lingkungan.
