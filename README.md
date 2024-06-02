# Sistem Pengukur Suhu dan Kelembaban Menggunakan Mikrokontroler ATtiny85

## Deskripsi Proyek
Proyek ini adalah sebuah sistem pengukur suhu dan kelembaban yang menggunakan mikrokontroler ATtiny85, sensor DHT22, dan layar OLED kecil untuk menampilkan data. Sistem ini dirancang untuk mengukur dan menampilkan suhu dan kelembaban secara real-time.

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

Fungsi Splash Screen
```cpp
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

Fungsi Animasi Heartbeat
```cpp
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

