## Terima Data dengan Sistem Pooling

**Kebutuhan Komponen**

1. Sistem Minimum STM32F4Discovery 2 unit
2. Kabel Jumper Male-Female secukupnya

**Gambar Percobaan**

**Langkah Percobaan**

1. Buatlah program pada STM32F4Discovery yang berfungsi sebagai master. Aktifkan konfigurasi SPI2 dengan kecepatan transfer data 21.0Mbps seperti berikut ini
   ![](/assets/2017-11-27_093032.png)  
   ![](/assets/2017-11-27_093038.png)

2. Simpan project Anda dengan nama STM32F4\_SPI\_Master\_Send\_Pooling, kemudian buka project menggunakan Keil MDK ARM 5.
3. Lakukan modifikasi program pada STM32F4Discovery yang berfungsi sebagai master menjadi seperti berikut ini
   Ubahlah fungsi main menjadi seperti berikut ini

   ```c
   int main(void)
   {
     // Deklarasi variabel yang akan dikirimkan ke STM32F4Discovery Slave
     uint8_t sendData[1];
  
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
       // Jika tombol user ditekan variabel sendData[0] bernilai 255
       if(HAL_GPIO_ReadPin(B1_GPIO_Port,B1_Pin)==1) {
         sendData[0] = 255;
       }
       // Jika tombol user tidak ditekan variabel sendData[0] bernilai 0
       else {
         sendData[0] = 0;
       }
       // Kirim variabel sendData ke SPI2
       HAL_SPI_Transmit(&hspi2,sendData,1,1);
     }
   }
   ```

4. Lakukan download program ke STM32F4Discovery yang berfungsi sebagai master
5. Buatlah program pada STM32F4Discovery yang berfungsi sebagai slave menggunakan STM32Cube MX. Aktifkan konfigurasi SPI2 dengan mode Full-Duplex Slave
   ![](/assets/2017-11-27_084412 - Copy.png)

6. Pada menu SPI2 Configuration ubahlah User Label pada pin yang digunakan untuk komunikasi SPI  
   ![](/assets/2017-11-27_084531.png)

7. Simpan project Anda dengan nama STM32F4\_Slave\_Receive\_Pooling dan buka menggunakan Keil
8. Lakukan modifikasi program pada file main.c manjadi berikut ini
   Ubahlah fungsi main menjadi seperti berikut ini

   ```c
   int main(void)
   {
     // Deklarasi variabel yang digunakan untuk menerima data
     uint8_t receivedData[1];

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
       // Menerima data dari SPI2 kemudian disimpan pada variabel receivedData
       HAL_SPI_Receive(&hspi2,receivedData,1,1);
       // Jika variabel receivedData[0] bernilai 255
       // Nyalakan LED
       if(receivedData[0] == 255) {
         HAL_GPIO_WritePin(LD3_GPIO_Port,LD3_Pin,1);
       }
       // Jika variabel receivedData[0] bernilai 0
       // Matikan LED
       else {
         HAL_GPIO_WritePin(LD3_GPIO_Port,LD3_Pin,0);
       }
     }
   }
   ```

9. Lakukan kompilasi dan download program ke mikrokontroler. Hubungkan STM32F4Discovery master dan STM32F4Discovery  slave sesuai koneksi pada gambar percobaan
10. Untuk melakukan ujicoba, tekan tombol pada mikrokontroler master dan perhatikan nyala LED pada mikrokontroler slave. Jika percobaan Anda benar, ketika tombol pada mikrokontroler master ditekan LED pada mikrokontroler slave akan menyala yang menandakan slave menerima data dari mikrokontroler master.



