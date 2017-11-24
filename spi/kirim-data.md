## Kirim Data dengan Sistem Pooling



**Kebutuhan Komponen**

1. Sistem Minimum STM32F4Discovery 1 unit
2. USB Logic Analyzer 1 unit
3. Kabel Jumper Male-Female secukupnya

**Gambar Percobaan**

**Langkah Percobaan**

1. Buatlah project baru menggunakan STM32 CubeMX
2. Pada saat melakukan konfigurasi program. Aktifkan fitur SPI2 pada menu Peripherals dan pilih mode Full-Duplex Master
   ![](/assets/2017-11-24_125634 - Copy.png)

3. Lankutkan dengan melakukan konfigurasi SPI2 pada menu Configuration
   ![](/assets/2017-11-24_125644 - Copy.png)

4. Pada menu tab menu Parameter Settings ubahlah baudrate default dari SPI dengan cara mengganti nilai prescaler menjadi yang lebih besar. Pada percobaan kali ini digunakan nilai prescaler 256. Sehingga baudrate yang digunakan adalah 164.062 Kbps.
   ![](/assets/2017-11-24_134521.png)

5. Pada tab menu GPIO Setting, ubahlah nama user label dari pin yang digunakan untuk SPI menjadi nama-nama berikut ini
   ![](/assets/2017-11-24_125814.png)

6. Generate dan simpan project Anda dengan nama project STM32\_SPI\_MASTER\_Send\_Pooling dengan target IDE MDK-ARM V5
   ![](/assets/2017-11-24_125917.png)

7. Ubahlah program pada file main.c menjadi seperti berikut ini
   Tambahkan header file string.h

   ```c
   #include "string.h"
   ```

   Tambahkan sintaks program berikut ini pada fungsi main

   ```c
   int main(void)
   {
     // Deklarasi variabel yang akan dikirimkan melalui SPI
     uint8_t data[] = "Hello";

     /* MCU Configuration----------------------------------------------------------*/
     /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
     HAL_Init();

     /* Configure the system clock */
     SystemClock_Config();

     /* Initialize all configured peripherals */
     MX_GPIO_Init();
     MX_SPI2_Init();

     /* Infinite loop */
     while (1)
     {
       // Mengirimkan data melalui SPI2
       HAL_SPI_Transmit(&hspi2,data,strlen(data),100);
       HAL_Delay(100);
     }
   }
   ```

8. Simpan project dan lakukan download program ke mikrokontroler.
9. Untuk melakukan ujicoba, hubungkan pin SPI pada STM32F4Discovery ke USB Logic Analyzer sesuai dengan gambar percobaan. Kemudian capture sinyal dengan cara menekan tombol Start pada software Saleae Logic. Tampilan sinyal MOSI dan SCK dari STM32F4Discovery akan ditampilkan pada software Saleae Logic.
   ![](/assets/2017-11-24_134558 - Copy.png)

10. Lakukan analisa sinyal lebih lanjut dengan cara men-decode sinyal menggunakan protokol SPI. Klik simbol + pada menu Analyzer. Kemudian pilih SPI
    ![](/assets/2017-11-24_134624 - Copy.png)

11. Pada window Analyzer Settings sesuaikan konfigurasinya seperti berikut ini. Pada percobaan kali ini yang dilakukan analisa hanya sinyal MOSI dan SCK dari STM32F4Discovery yang berfungsi sebagai perangkat master. Klik Save untuk mengakhiri proses konfigurasi
    ![](/assets/2017-11-24_134642.png)

12. Setelah melakukan konfigurasi diatas maka pada bagian sinyal akan muncul data karakter dan kode ASCII dari data yang dikirimkan melalui SPI.
    ![](/assets/2017-11-24_134716.png)



