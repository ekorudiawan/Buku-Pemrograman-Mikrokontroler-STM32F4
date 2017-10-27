## Kirim Data dengan Sistem Pooling

**Percobaan**

1. Hubungkan board mikrokontroler STM32F4Discovery dengan modul USB to Serial seperti Gambar
2. Buat project baru menggunakan STM32CubeMX dengan memilih STM32F4Dicovery pada menu **Board Selector**. Aktifkan check box **Initialize all peripherals with their default Mode**.  
   ![](/assets/2017-10-27_083629.png)

3. Lakukan konfigurasi pada menu **Pinout** dengan mengaktifkan peripheral USART2 dengan mode Asynchronous  
   ![](/assets/2017-10-27_083655.png)

4. Lakukan konfigurasi pada menu **Configuration** dengan memilih USART2 pada bagian Connectivity  
   ![](/assets/2017-10-27_083716.png)

5. Pada menu Parameter Setting Anda dapat melakukan pengaturan baudrate, ukuran data bit, parity, stop bit dan parameter komunikasi serial lainnya.  
   ![](/assets/2017-10-27_083815.png)

6. Pada menu GPIO Settings rubahlah nama pin pada bagian User Label sesuai dengan nama yang diinginkan. Langkah ini sifatnya tidak wajib, namun dapat memudahkan dalam mengingat nama pin-pin yang digunakan.  
   ![](/assets/2017-10-27_103846.png)

7. Jika konfigurasi telah selesai lakukan generate source code dan simpan project Anda. Buka project Anda menggunakan Keil IDE. Kemudian lakukan modifikasi program pada file main.c seperti berikut ini :  
   Tambahkan header file string.h  
   `#include "string.h`  
   Modifikasi fungsi main seperti berikut ini  
   `int main(void)`

   `{`

   `// Inisialisasi data yang akan dikirim ke UART`

   `char data[] = "Hello World from USART \r\n";`

   `  
   /* MCU Configuration----------------------------------------------------------*/`

   `  
   /* Reset of all peripherals, Initializes the Flash interface and the Systick. */`

   `HAL_Init();`

   `  
   /* Configure the system clock */`

   `SystemClock_Config();`

   `  
   /* Initialize all configured peripherals */`

   `MX_GPIO_Init();`

   `MX_USART2_UART_Init();`

   `  
   /* Infinite loop */`

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

8. 


