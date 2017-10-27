## Kirim Data dengan Sistem Pooling

**Percobaan**

1. Hubungkan board mikrokontroler STM32F4Discovery dengan modul USB to Serial seperti Gambar
2. Buat project baru menggunakan STM32CubeMX dengan memilih STM32F4Dicovery pada menu **Board Selector**. Aktifkan check box **Initialize all peripherals with their default Mode**.  
   ![](/assets/2017-10-27_083629.png)

3. Lakukan konfigurasi pada menu **Pinout** dengan mengaktifkan peripheral USART2 dengan mode Asynchronous  
   ![](/assets/2017-10-27_083655.png)

4. Lakukan konfigurasi pada menu **Configuration** dengan memilih USART2 pada bagian **Connectivity**  
   ![](/assets/2017-10-27_083716.png)

5. Pada menu **Parameter Setting** Anda dapat melakukan pengaturan baudrate, ukuran data bit, parity, stop bit dan parameter komunikasi serial lainnya.  
   ![](/assets/2017-10-27_083815.png)

6. Pada menu **GPIO Settings** rubahlah nama pin pada bagian **User Label** sesuai dengan nama yang diinginkan. Langkah ini sifatnya tidak wajib, namun dapat memudahkan dalam mengingat nama pin-pin yang digunakan.  
   ![](/assets/2017-10-27_103846.png)

7. Jika konfigurasi telah selesai lakukan generate source code dan simpan project Anda dengan nama STM32F4\_USART\_Send\_Pooling dengan target IDE MDK-ARM5.  
   ![](/assets/2017-10-27_084025.png)

8. Buka project Anda menggunakan Keil IDE. Kemudian lakukan modifikasi program pada file main.c seperti berikut ini :  
   Tambahkan header file string.h  
   `#include "string.h`  
   Modifikasi fungsi main seperti berikut ini  
   `int main(void)`

   `{`

   `// Inisialisasi data yang akan dikirim ke UART`

   `char data[] = "Hello World from USART \r\n";`

   `/* MCU Configuration----------------------------------------------------------*/`

   `/* Reset of all peripherals, Initializes the Flash interface and the Systick. */`

   `HAL_Init();`

   `/* Configure the system clock */`

   `SystemClock_Config();`

   `/* Initialize all configured peripherals */`

   `MX_GPIO_Init();`

   `MX_USART2_UART_Init();`

   `/* Infinite loop */`

   `while (1)`

   `{`

   `// Pengiriman data menggunakan USART2`

   `// Data yang dikirim adalah variabel data`

   `// Panjang data yang dikirim dikalkulasi dengan fungsi strlen`

   `// Timeout pengiriman data 10ms`

   `HAL_UART_Transmit(&huart2,(uint8_t*)data,strlen(data),10);`

   `// Jeda pengiriman data setiap 500ms`

   `HAL_Delay(500);`

   `}`

   `}`

9. Untuk melakukan ujicoba program, hubungkan kabel USB to Serial ke komputer. Kemudian cek Device Manager untuk melihat nama port dari kabel USB to Serial.  
   ![](/assets/2017-10-27_114200.png)

10. Buka software HTerm pada komputer Anda, kemudian samakan nama port dengan yang tertera pada Device Manager. Pastikan juga konfigurasi baudrate, databit, parity dan stop bit pada HTerm sama dengan yang sudah Anda setting pada saat membuat program sebelumnya \(Lihat langkah 5\). Pada combo box **Newline at** pilih CR+LF , pilihan ini membuat data akan ditampilkan ke bawah setiap ada karakter enter \(CR+LF\).   
    ![](/assets/2017-10-27_085452.png)

11. Klik tombol Connect, kemudian perhatikan Received Data pada HTerm. Data yang dikirimkan oleh mikrokontroler akan muncul pada kolom ini. Pastikan data yang dikirim oleh mikrokontroler sama dengan yang ditampilkan pada kolom Received Data. Jika data yang muncul tidak sama kemungkinan terdapat kesalahan pada saat melakukan konfigurasi project.  
    ![](/assets/2017-10-27_091006.png)

12. Anda dapat melihat sinyal-sinyal data yang dikirimkan melalui USART dengan menggunakan USB Logic Analyzer. Hubungkan pin PA2 pada board STM32F4Discovery dengan CH0 pada USB Logic Analyzer.

13. Buka software Saleae Logic, tekan tombol Start untuk mulai mengcapture sinyal. Pada layar akan muncul sinyal digital yang dikirimkan melalui pin transmitter USART \(PA2\).   
    ![](/assets/2017-10-27_091710.png)

14. Untuk melakukan analisa sinyal digital, pilih menu Analyzer kemudian pilih Async Serial.  
    ![](/assets/2017-10-27_091749.png)

15.  Lakukan konfigurasi parameter serial sesuai dengan konfigurasi pada project   
    ![](/assets/2017-10-27_091816.png)

16. Pada bagian atas akan muncul karakter ASCII hasil decode dari sinyal digital yang dikirimkan melalui USART  
    ![](/assets/2017-10-27_091841.png)



