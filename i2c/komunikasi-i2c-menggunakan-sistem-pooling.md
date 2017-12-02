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

6. fsdgsdg

   ```
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
   uint8_t receiveData[1];

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
	
     while (1)
     {
     /* USER CODE END WHILE */
       //MX_USB_HOST_Process();
   		HAL_I2C_Slave_Receive(&hi2c1,receiveData,1,1000);
   		if (receiveData[0] == 255) {
   			HAL_GPIO_WritePin(LD3_GPIO_Port,LD3_Pin,1);
   		}
   		else if(receiveData[0] == 127) {
   			HAL_GPIO_WritePin(LD3_GPIO_Port,LD3_Pin,0);
   		}
		

     /* USER CODE BEGIN 3 */

     }
     /* USER CODE END 3 */

   }
   ```

7. sdfsdf

8. \`\`\`c  
   /_ Includes ------------------------------------------------------------------_/

   # include "main.h"

   # include "stm32f4xx\_hal.h"

   # include "usb\_host.h"

/_ USER CODE BEGIN Includes _/

/_ USER CODE END Includes _/

/_ Private variables ---------------------------------------------------------_/  
I2C\_HandleTypeDef hi2c1;

I2S\_HandleTypeDef hi2s3;

SPI\_HandleTypeDef hspi1;

/_ USER CODE BEGIN PV _/  
/_ Private variables ---------------------------------------------------------_/

/_ USER CODE END PV _/

/_ Private function prototypes -----------------------------------------------_/  
void SystemClock\_Config\(void\);  
static void MX\_GPIO\_Init\(void\);  
static void MX\_I2C1\_Init\(void\);  
static void MX\_I2S3\_Init\(void\);  
static void MX\_SPI1\_Init\(void\);  
void MX\_USB\_HOST\_Process\(void\);

/_ USER CODE BEGIN PFP _/  
/_ Private function prototypes -----------------------------------------------_/

/_ USER CODE END PFP _/

/_ USER CODE BEGIN 0 _/

/_ USER CODE END 0 _/  
uint8\_t receiveData\[1\];

int main\(void\)  
{

/_ USER CODE BEGIN 1 _/

/_ USER CODE END 1 _/

/_ MCU Configuration----------------------------------------------------------_/

/_ Reset of all peripherals, Initializes the Flash interface and the Systick. _/  
  HAL\_Init\(\);

/_ USER CODE BEGIN Init _/

/_ USER CODE END Init _/

/_ Configure the system clock _/  
  SystemClock\_Config\(\);

/_ USER CODE BEGIN SysInit _/

/_ USER CODE END SysInit _/

/_ Initialize all configured peripherals _/  
  MX\_GPIO\_Init\(\);  
  MX\_I2C1\_Init\(\);  
  //MX\_I2S3\_Init\(\);  
  //MX\_SPI1\_Init\(\);  
  //MX\_USB\_HOST\_Init\(\);

/_ USER CODE BEGIN 2 _/

/_ USER CODE END 2 _/

/_ Infinite loop _/  
  /_ USER CODE BEGIN WHILE _/

while \(1\)  
  {  
  /_ USER CODE END WHILE _/  
    //MX\_USB\_HOST\_Process\(\);  
        HAL\_I2C\_Slave\_Receive\(&hi2c1,receiveData,1,1000\);  
        if \(receiveData\[0\] == 255\) {  
            HAL\_GPIO\_WritePin\(LD3\_GPIO\_Port,LD3\_Pin,1\);  
        }  
        else if\(receiveData\[0\] == 127\) {  
            HAL\_GPIO\_WritePin\(LD3\_GPIO\_Port,LD3\_Pin,0\);  
        }

/_ USER CODE BEGIN 3 _/

}  
  /_ USER CODE END 3 _/

}  
\`\`\`

