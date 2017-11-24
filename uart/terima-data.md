## Terima Data Menggunakan Sistem Pooling

**Kebutuhan Komponen**

1. Sistem Minimum STM32F4Discovery 1 unit
2. Modul USB to Serial Converter 1 unit
3. Kabel Jumper Male-Female secukupnya

**Gambar Percobaan**

**Percobaan**

1. Buat _project_ baru dengan konfigurasi yang sama seperti percobaan 1. _Generate source code_ dan simpan _project_ Anda dengan nama **STM32F4\_USART\_Receive\_Pooling**.
2. Lakukan modifikasi program pada file main.c seperti berikut ini :

   Tambahkan baris kode berikut ini pada fungsi main.c

   ```c
   int main(void)
   {
     // Deklarasi variabel untuk menampung data dari USART
     // Variabel harus dalam bentuk array 
     char receiveData[1] ;

     /* MCU Configuration----------------------------------------------------------*/
     /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
     HAL_Init();

     /* Configure the system clock */
     SystemClock_Config();

     /* Initialize all configured peripherals */
     MX_GPIO_Init();
     MX_USART2_UART_Init();

     /* Infinite loop */
     /* USER CODE BEGIN WHILE */
     while (1)
     {
       // Menunggu sampai ada data yang masuk melalui USART
       // Data akan disimpan pada variabel receivedData
       if(HAL_UART_Receive(&huart2,(uint8_t*)receiveData,1,0)==HAL_OK) {
         // Mengirim data menggunakan pooling
         // Data yang dikirimkan adalah variabel receivedData
         HAL_UART_Transmit(&huart2,(uint8_t*)receiveData,1,0);
       }
     }
   }
   ```

3. Untuk melakukan ujicoba program diatas, buka software HTerm. Lakukan konfigurasi pada parameter komunikasi serial yang digunakan.

4. Lakukan pengiriman data pada software HTerm, dengan cara mengetikkan sembarang karakter pada bagian input control pada software HTerm. Kemudian tekan tombol ASend  
   ![](/assets/2017-11-24_095833.png)

5. Akan muncul informasi tentang data yang akan dikirimkan oleh komputer ke Serial Port. Klik Start untuk mulai mengirimkan data.  
   ![](/assets/2017-11-24_095930.png)

6. Data yang dikirimkan oleh komputer melalui software HTerm akan diterima oleh STM32F4Discovery melalui pin USART\_RX dan akan dikirimkan kembali ke komputer. Sehingga Anda dapat memastikan percobaan berhasil jika data yang dikirimkan sebelumnya diterima kembali oleh komputer.  
   ![](/assets/2017-11-24_101602.png)

## Terima Data Menggunakan Sistem Interupsi

**Kebutuhan Komponen**

1. Sistem Minimum STM32F4Discovery 1 unit
2. Modul USB to Serial Converter 1 unit
3. Kabel Jumper Male-Female secukupnya

**Gambar Percobaan**

**Percobaan**

1. Buat _project_ baru dengan konfigurasi yang sama seperti percobaan 1. Aktifkan USART2 Global Interrupt pada bagian NVIC Settings. _Generate source code_ dan simpan _project_ Anda dengan nama **STM32F4\_USART\_Receive\_Interrupt**.  
   ![](/assets/2017-11-24_102354.png)

2. Lakukan modifikasi program pada file main.c seperti berikut ini :

   Tambahkan global variabel untuk menampung data dari USART

   ```c
   char receivedData[1];
   ```

   Tambahkan fungsi callback untuk meng-handle interrupt jika ada yang masuk pada UART

   ```c
   void HAL_UART_RxCpltCallback(UART_HandleTypeDef *huart)
   {
       UNUSED(huart);
       HAL_UART_Transmit(&huart2,(uint8_t *) receivedData, 1, 0);
   }
   ```

   Pada fungsi main tambahkan fungsi untuk mengaktifkan interupsi pada saat terjadi penerimaan data tiap 1 byte pada UART

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

     while (1)
     {
       // Mengaktifkan interupsi jika ada 1 byte data yang diterima pada USART2 
       HAL_UART_Receive_IT(&huart2,(uint8_t *)receivedData,1);
     }
   }
   ```

3. Lakukan ujicoba seperti pada Percobaan xx . Hasilnya akan sama seperti pada percobaan sebelumnya. Namun pada percobaan kali ini penerimaan data tanpa menunggu data masuk pada USART.



