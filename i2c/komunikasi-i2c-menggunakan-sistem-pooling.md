## Master Kirim Data - Slave Terima Data

**Kebutuhan Komponen**

1. Sistem Minimum STM32F4Discovery 1 unit
2. USB Logic Analyzer 1 unit
3. Kabel Jumper Male-Female secukupnya

**Gambar Percobaan**

**Langkah Percobaan**

1. Buatlah project baru menggunakan STM32 CubeMX
2. dfghdfgdf

3. gjgfhjgfhj

   ```c
   /* Includes ------------------------------------------------------------------*/
   #include "main.h"
   #include "stm32f4xx_hal.h"
   #include "usb_host.h"

   /* USER CODE BEGIN Includes */

   /* USER CODE END Includes */

   /* Private variables ---------------------------------------------------------*/
   I2C_HandleTypeDef hi2c1;

   I2S_HandleTypeDef hi2s3;

   SPI_HandleTypeDef hspi1;

   /* USER CODE BEGIN PV */
   /* Private variables ---------------------------------------------------------*/

   /* USER CODE END PV */

   /* Private function prototypes -----------------------------------------------*/
   void SystemClock_Config(void);
   static void MX_GPIO_Init(void);
   static void MX_I2C1_Init(void);
   static void MX_I2S3_Init(void);
   static void MX_SPI1_Init(void);
   void MX_USB_HOST_Process(void);

   /* USER CODE BEGIN PFP */
   /* Private function prototypes -----------------------------------------------*/

   /* USER CODE END PFP */

   /* USER CODE BEGIN 0 */

   /* USER CODE END 0 */

   int main(void)
   {

     /* USER CODE BEGIN 1 */

     /* USER CODE END 1 */

     /* MCU Configuration----------------------------------------------------------*/

     /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
     HAL_Init();

     /* USER CODE BEGIN Init */

     /* USER CODE END Init */

     /* Configure the system clock */
     SystemClock_Config();

     /* USER CODE BEGIN SysInit */

     /* USER CODE END SysInit */

     /* Initialize all configured peripherals */
     MX_GPIO_Init();
     MX_I2C1_Init();
     //MX_I2S3_Init();
     //MX_SPI1_Init();
     //MX_USB_HOST_Init();

     /* USER CODE BEGIN 2 */

     /* USER CODE END 2 */

     /* Infinite loop */
     /* USER CODE BEGIN WHILE */
       uint8_t sendData[1];
       sendData[0] = 0;
     while (1)
     {
     /* USER CODE END WHILE */
       //MX_USB_HOST_Process();

           if(HAL_GPIO_ReadPin(B1_GPIO_Port,B1_Pin)==1) {
               sendData[0] = 255;
           }
           // Jika tombol user tidak ditekan variabel sendData[0] bernilai 0
           else {
               sendData[0] = 127;
           }
           HAL_I2C_Master_Transmit(&hi2c1,(0x07<<1),sendData,1,1000);

     /* USER CODE BEGIN 3 */

     }
     /* USER CODE END 3 */

   }
   ```

4. sdfgsdf

5. sdfsdf

6. halooo



