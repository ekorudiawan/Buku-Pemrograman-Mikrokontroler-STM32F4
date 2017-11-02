## Kirim Data dengan Sistem Pooling

Percobaan kali ini akan dilakukan uji coba program untuk mengirimkan data serial USART dengan sistem pooling. Periferal USART yang digunakan adalah USART2 yang terhubung dengan pin PA2 \(_transmitter_\) dan PA3 \(_receiver_\). Data akan dikirimkan ke komputer melalui kabel USB to Serial. Parameter USART yang digunakan pada percobaan kali ini adalah sebagai berikut :

* _Baudrate_ 115200
* _Data bits_ 8
* _Parity none_
* _Stop bits_ 1

Data yang akan dikirimkan ke komputer merupakan data ASCII dari teks "Hello World from USART" dan diakhiri dengan karakter _enter_ \(CR+LF\). Jeda tiap pengiriman data adalah 500ms agar data yang diterima di komputer dapat diamati dengan baik.

**Kebutuhan Komponen**

1. Sistem Minimum STM32F4Discovery 1 unit
2. Modul USB to Serial Converter 1 unit
3. USB Logic Analyzer 1 unit
4. Kabel Jumper Male-Female secukupnya

**Gambar Percobaan**

**Langkah Percobaan**

1. Hubungkan sistem minimum STM32F4Discovery dengan modul USB to Serial Converter seperti pada Gambar Percobaan.
2. Buat _project_ baru menggunakan STM32CubeMX dengan memilih **STM32F4DISCOVERY** pada menu **Board Selector**. Aktifkan _check box_ **Initialize all peripherals with their default Mode** agar STM32CubeMX melakukan konfigurasi standar terhadap periferal dan distribusi _clock_ sesuai dengan konfigurasi STM32F4Discovery.  
   ![](/assets/2017-10-27_083629.png)

3. Lakukan konfigurasi pada menu **Pinout** dengan mengaktifkan periferal **USART2** dengan mode **Asynchronous**.  
   ![](/assets/2017-10-27_083655.png)

4. Lakukan konfigurasi pada menu **Configuration** dengan memilih **USART2** pada bagian **Connectivity.**  
   ![](/assets/2017-10-27_083716.png)

5. Pada menu **Parameter Setting** Anda dapat melakukan pengaturan _baudrate_, ukuran _data bits_, _parity_, _stop bits_ dan parameter komunikasi serial lainnya.  
   ![](/assets/2017-10-27_083815.png)

6. Pada menu **GPIO Settings** rubahlah nama pin pada bagian **User Label** sesuai dengan nama yang diinginkan. Langkah ini sifatnya tidak wajib, namun dapat memudahkan dalam mengingat nama pin-pin yang digunakan ketika membuat program.  
   ![](/assets/2017-10-27_103846.png)

7. Jika konfigurasi telah selesai lakukan _generate source code_ dan simpan _project_ Anda dengan nama **STM32F4\_USART\_Send\_Pooling** dengan target IDE **MDK-ARM5**.  
   ![](/assets/2017-10-27_084025.png)

8. Buka _project_ Anda menggunakan Keil IDE. Kemudian lakukan modifikasi program pada _file_ main.c seperti berikut ini :

   Tambahkan _header file_ **string.h**

   ```c
   #include "string.h"
   ```

   Modifikasi fungsi **main** menjadi seperti berikut ini

   ```c
   int main(void)
   {
       // Inisialisasi data yang akan dikirim ke UART
       char data[] = "Hello World from USART \r\n";

       /* MCU Configuration----------------------------------------------------------*/

       /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
       HAL_Init();

       /* Configure the system clock */
       SystemClock_Config();

       /* Initialize all configured peripherals */
       MX_GPIO_Init();
       MX_USART2_UART_Init();

       /* Infinite loop */
       while (1)
       {
           // Pengiriman data menggunakan USART2
           // Data yang dikirim adalah variabel data
           // Panjang data yang dikirim dikalkulasi dengan fungsi strlen
           // Timeout pengiriman data 10ms
           HAL_UART_Transmit(&huart2,(uint8_t*)data,strlen(data),10);
           // Jeda pengiriman data setiap 500ms
           HAL_Delay(500);
       }
   }
   ```

9. Lakukan uji coba program dengan cara menghubungkan kabel USB to Serial ke komputer. Kemudian cek **Device Manager** untuk melihat nama _port_ dari kabel USB to Serial.  
   ![](/assets/2017-10-27_114200.png)

10. Buka _software_ HTerm pada komputer Anda, kemudian samakan nama _port_ dengan yang tertera pada **Device Manager**. Pastikan juga konfigurasi _baudrate_, _data bits_, _parity_ dan _stop bits_ pada HTerm sama dengan yang sudah Anda atur pada saat membuat program sebelumnya \(lihat langkah 5\). Pada _combo box_ **Newline at** pilih **CR+LF** , pilihan ini membuat data akan ditampilkan ke bawah setiap ada karakter _enter_ \(CR+LF\).  
    ![](/assets/2017-10-27_085452.png)

11. Klik tombol **Connect**, kemudian perhatikan kolom **Received Data** pada HTerm. Data yang dikirimkan oleh mikrokontroler akan muncul pada kolom ini. Pastikan data yang dikirimkan oleh mikrokontroler sama dengan yang ditampilkan pada kolom **Received Data**. Jika data yang muncul tidak sama kemungkinan terdapat kesalahan pada saat melakukan konfigurasi _project \_atau kesalahan pengaturan parameter serial \_port_.  
    ![](/assets/2017-10-27_091006.png)

12. Anda dapat melihat sinyal-sinyal data yang dikirimkan melalui USART dengan menggunakan USB Logic Analyzer. Hubungkan pin PA2 pada STM32F4Discovery dengan CH0 pada USB Logic Analyzer.

13. Buka _software_ Saleae Logic, tekan tombol **Start** untuk mulai meng-_capture_ sinyal. Pada layar akan muncul sinyal digital yang dikirimkan melalui pin _transmitter_ USART \(PA2\).  
    ![](/assets/2017-10-27_091710.png)

14. Untuk melakukan analisa sinyal digital, pilih menu **Analyzer** kemudian pilih **Async Serial**.  
    ![](/assets/2017-10-27_091749.png)

15. Lakukan konfigurasi parameter serial sesuai dengan konfigurasi pada \_project \_yang telah Anda buat.  
    ![](/assets/2017-10-27_091816.png)

16. Pada bagian atas akan muncul karakter ASCII hasil _decode_ dari sinyal digital yang dikirimkan melalui USART.  
    ![](/assets/2017-10-27_091841.png)

## Kirim Data dengan Interupsi

**Kebutuhan Komponen**

1. Sistem Minimum STM32F4Discovery 1 unit
2. Modul USB to Serial Converter 1 unit
3. Kabel Jumper Male-Female secukupnya

**Gambar Percobaan**

**Percobaan**

1. Buat _project_ baru dengan konfigurasi seperti percobaan sebelumnya. Pada pengaturan **USART Configuration** aktifkan **USART2 global interrupt** pada tab **NVIC Settings**. Ini bertujuan untuk mengaktifkan interupsi pada periferal USART.  
   ![](/assets/2017-10-27_132307.png)

2. Lakukan _generate source code_ dan simpan _project_ Anda dengan nama **STM32F4\_USART\_Send\_Interrupt**.  
   ![](/assets/2017-10-27_132437.png)

3. Buka _project_ Anda kemudian lakukan modifikasi program pada _file_ **main.c** seperti berikut ini :

   Tambahkan _header file_ **string.h**

   ```c
   #include "string.h"
   ```

   Tambahkan fungsi _callback_ di bawah ini pada _file_ **main.c** di lokasi sebelum fungsi main. Fungsi _callback_ merupakan fungsi yang akan dieksekusi apabila terjadi iterupsi yang bersumber dari USART. Pada percobaan kali ini interupsi akan terjadi jika proses pengiriman data telah komplit. Fungsi _callback_ akan tetap dieksekusi apabila terjadi interupsi, walupun tidak pernah dipanggil pada fungsi main.

   ```c
   void HAL_UART_TxCpltCallback(UART_HandleTypeDef *huart)
   {
       UNUSED(huart); 
       HAL_GPIO_TogglePin(LD3_GPIO_Port,LD3_Pin);
   }
   ```

   Lakukan modifikasi fungsi main menjadi seperti di bawah ini :

   ```c
   int main(void)
   {
       char *data = "Hello World From USART - Interrupt Transmit Mode \r\n";

       /* MCU Configuration----------------------------------------------------------*/
       /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
       HAL_Init();

       /* Configure the system clock */
       SystemClock_Config();

       /* Initialize all configured peripherals */
       MX_GPIO_Init();
       MX_USART2_UART_Init();

       while (1)
       {
           HAL_UART_Transmit_IT(&huart2,(uint8_t *)data,strlen(data));
           HAL_Delay(500);
       }
   }
   ```

4. Lakukan kompilasi dan _upload_ program ke mikrokontroler. Buka _software_ HTerm dan perhatikan data yang dikirimkan oleh mikrokontroler. Perhatikan juga nyala LED3 pada STM32F4Discovery. LED3 akan berkedip jika transmisi data telah komplit atau telah selesai.

## Kirim Data Menggunakan Fungsi Printf

**Kebutuhan Komponen**

1. Sistem Minimum STM32F4Discovery 1 unit
2. Modul USB to Serial Converter 1 unit
3. Kabel Jumper Male-Female secukupnya

**Gambar Percobaan**

**Percobaan**

1. Buat _project_ baru dengan konfigurasi yang sama seperti percobaan 1. _Generate source code_ dan simpan _project_ Anda dengan nama **STM32F4\_USART\_Send\_Printf**.
2. Lakukan modifikasi program seperti berikut :

   Tambahkan header file string.h

   ```c
   #include "string.h"
   ```

   Tambahkan baris code berikut sebelum fungsi main

   ```c
   #ifdef __GNUC__
       #define PUTCHAR_PROTOTYPE int __io_putchar(int ch)
   #else
       #define PUTCHAR_PROTOTYPE int fputc(int ch, FILE *f)
   #endif

   PUTCHAR_PROTOTYPE
   {
       HAL_UART_Transmit(&huart2, (uint8_t *)&ch, 1, 0xFFFF);  
       return ch;
   }
   ```

   Modifikasi fungsi main menjadi seperti baris source code berikut ini

   ```c
   int main(void)
   {
       /* MCU Configuration----------------------------------------------------------*/
       /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
       HAL_Init();

       /* Configure the system clock */
       SystemClock_Config();

       /* Initialize all configured peripherals */
       MX_GPIO_Init();
       MX_USART2_UART_Init();

       int i = 0;

       while (1)
       {
           printf("Hello From USART \r\n");
           printf("Loop = %d \r\n",i);
           HAL_Delay(500);
       }
   }
   ```

3. Lakukan kompilasi dan uji coba program menggunakan _software_ HTerm.



