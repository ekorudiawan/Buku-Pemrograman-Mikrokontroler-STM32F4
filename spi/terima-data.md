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

## Terima Data dengan Sistem Interupsi

**Kebutuhan Komponen**

1. Sistem Minimum STM32F4Discovery 2 unit
2. Kabel Jumper Male-Female secukupnya

**Gambar Percobaan**

**Langkah Percobaan**

1. Buatlah project pada mikrokontroler master dengan mengaktifkan fitur SPI2 mode Full-Duplex Master. Aturlah konfigurasi pada SPI2 dengan kecepatan transfer data 21Mbps dan mengaktifkan SPI2 Global interrupt.  
   ![](/assets/2017-11-27_103225.png)  
   ![](/assets/2017-11-27_103312.png)  
   ![](/assets/2017-11-27_103303.png)

2. Simpan project Anda dengan nama STM32F4\_SPI\_Master\_Send\_Interrupt.

3. Lakukan modifikasi pada file main.c seperti berikut ini  
   Tambahkan program berikut ini pada fungsi main

   ```c
   int main(void)
   {
     // Inisialisasi variabel sendData 
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
       // Kirim variabel sendData ke SPI2 menggunakan interrupt
       HAL_SPI_Transmit_IT(&hspi2,sendData,1);
     }
   }
   ```

4. Lakukan kompilasi program dan download program ke mikrokontroler master

5. Buatlah project baru pada mikrokontroler slave dengan mengaktifkan fitur SPI2 mode Full-Duplex Slave. Aktifkan mode SPI2 Global interrupt pada konfigurasi SPI2.  
   ![](/assets/2017-11-27_084412 - Copy.png)  
   ![](/assets/2017-11-27_102437.png)  
   ![](/assets/2017-11-27_102444.png)

6. Simpan project Anda dengan nama STM32F4\_SPI\_SLave\_Receive\_Interrupt

7. Lakukan modifikasi program pada file main.c menjadi seperti berikut ini

   Tambahkan global variabel

   ```c
   uint8_t receivedData[1];
   ```

   Buatlah fungsi callback yang berfungsi untuk meng-handle interupsi jika ada 1 byte data yang diterima oleh mikrokontroler slave

   ```c
   void HAL_SPI_RxCpltCallback(SPI_HandleTypeDef *hspi)
   {
     UNUSED(hspi);
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
   ```

   Tambahkan program berikut ini pada fungsi main, yang berfungsi untuk mengaktifkan penerimaan data melalui interrupt

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
     MX_SPI2_Init();

     /* Infinite loop */
     while (1)
     {
       HAL_SPI_Receive_IT(&hspi2,receivedData,1);
     }
   }
   ```

8. Lakukan kompilasi dan download program pada mikrokontroler slave

9. Ujicoba dapat dilakukan dengan cara yang sama seperti pada percobaan sebelumnya yaitu dengan menekan tombol user pada mikrokontroler master dan melihat nyala LED pada mikrokontroler slave

## Komunikasi SPI MAX7219

**Kebutuhan Komponen**

1. Sistem Minimum STM32F4Discovery 1 unit
2. Modul 7 Segment Display MAX7219 1 unit
3. Kabel Jumper Male-Female secukupnya

**Gambar Percobaan**

**Langkah Percobaan**

1. Buatlah project baru menggunakan STM32Cube MX. Aktifkan peripheral SPI2 dengan mode Full-Duplex Master.  
   ![](/assets/2017-11-27_130007 - Copy.png)

2. Lakukan konfigurasi GPIO pada pin PB12 menjadi output  
   ![](/assets/2017-11-27_130014 - Copy.png)

3. Lakukan konfigurasi pada peripheral SPI2. Aturlah nilai prescaler menjadi 256, sehingga kecepatan transfer data menjadi 164.062 KBps   
   ![](/assets/2017-11-27_131912.png)  
   ![](/assets/2017-11-27_130143.png)

4. Pada menu konfigurasi GPIO ubahlah label pada pin PB12 menjadi SPI2\_SS  
   ![](/assets/2017-11-27_130056.png)

5. Ubahlah program pada file main.c menjadi seperti berikut ini  
   Tambahkan definisi alamat register MAX7219

   ```c
   #define OP_NOOP           0
   #define OP_DIGIT0         1
   #define OP_DIGIT1         2
   #define OP_DIGIT2         3
   #define OP_DIGIT3         4
   #define OP_DIGIT4         5
   #define OP_DIGIT5         6
   #define OP_DIGIT6         7
   #define OP_DIGIT7         8
   #define OP_DECODEMODE      9
   #define OP_INTENSITY       10
   #define OP_SCANLIMIT       11
   #define OP_SHUTDOWN        12
   #define OP_DISPLAYTEST     15
   ```

   Tambahkan deklarasi variabel

   ```c
   uint8_t max7219Data[2];
   uint8_t charTable[] = {0x7E,0x30,0x6D,0X79,0X33,0x5B,0x5F,0x70,0x7F,0x7B};
   ```

   Tambahkan fungsi untuk mengirim data ke MAX7219

   ```c
   void max7219Write(uint8_t address, uint8_t data) {
       HAL_GPIO_WritePin(SPI2_SS_GPIO_Port, SPI2_SS_Pin, GPIO_PIN_RESET);
       max7219Data[0] = address;
       max7219Data[1] = data;
       HAL_SPI_Transmit(&hspi2,max7219Data,2,1);
       HAL_GPIO_WritePin(SPI2_SS_GPIO_Port, SPI2_SS_Pin, GPIO_PIN_SET);
       HAL_Delay(1);
   }
   ```

   Ubahlah program pada fungsi main menjadi seperti berikut ini

   ```c
   int main(void)
   {    
     int counter = 0;

     /* MCU Configuration----------------------------------------------------------*/
     /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
     HAL_Init();

     /* Configure the system clock */
     SystemClock_Config();

     /* Initialize all configured peripherals */
     MX_GPIO_Init();
     MX_SPI2_Init();

     max7219Write(OP_SHUTDOWN, 0x00);
     max7219Write(OP_DISPLAYTEST, 0x00);
     max7219Write(OP_INTENSITY, 0x0F);
     max7219Write(OP_SHUTDOWN, 0x01);

     uint8_t digit7, digit6, digit5, digit4, digit3, digit2, digit1, digit0;
     int modDigit7, modDigit6, modDigit5, modDigit4, modDigit3, modDigit2, modDigit1;
     while (1)
     {
       counter++;
       digit7 = counter / 10000000;
       modDigit7 = counter % 10000000;
       digit6 = modDigit7 / 1000000;
       modDigit6 = modDigit7 % 1000000;
       digit5 = modDigit6 / 100000;
       modDigit5 = modDigit6 % 100000;
       digit4 = modDigit5 / 10000;
       modDigit4 = modDigit5 % 10000;
       digit3 = modDigit4 / 1000;
       modDigit3 = modDigit4 % 1000;
       digit2 = modDigit3 / 100;
       modDigit2 = modDigit3 % 100;
       digit1 = modDigit2 / 10;
       modDigit1 = modDigit2 % 10;
       digit0 = modDigit1 % 10;

       max7219Write(OP_DIGIT7, charTable[digit7]);
       max7219Write(OP_DIGIT6, charTable[digit6]);
       max7219Write(OP_DIGIT5, charTable[digit5]);
       max7219Write(OP_DIGIT4, charTable[digit4]);
       max7219Write(OP_DIGIT3, charTable[digit3]);
       max7219Write(OP_DIGIT2, charTable[digit2]);
       max7219Write(OP_DIGIT1, charTable[digit1]);
       max7219Write(OP_DIGIT0, charTable[digit0]);

       HAL_Delay(500);
     }
   }
   ```

6. Lakukan kompilasi dan download program ke mikrokontroler.

7. Ujicoba percobaan dengan cara menghubungkan mikrokontroler dengan modul 7 segment display MAX7219. 7 Segment akan menampilkan nilai variabel counter yang menghitung naik secara terus menerus.



