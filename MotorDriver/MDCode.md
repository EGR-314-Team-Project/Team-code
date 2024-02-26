#include "mcc_generated_files/mcc.h"
#include <stdio.h>

#define DELAY_MS 1000

// Function prototypes
void initSPI(void);

int main(void)
{
    SYSTEM_Initialize();
    SPI1_Initialize();
    SPI1_Open(SPI1_DEFAULT);
    EUSART2_Initialize();

    while (1)
    {
        // Check if a command is received over EUSART
        if (EUSART2_is_rx_ready())
        {
            char command = EUSART2_Read();

            // Check if the received command is '1'
            if (command == '1')
            {
                // Determine the data based on the command
                uint8_t data = 0b11101111;
                
                // Set chip select low
                CS_RD4_SetLow();
                // Exchange data
                uint8_t receivedData = SPI1_ExchangeByte(data);
                // Set chip select high
                CS_RD4_SetHigh();

                // Check if TX buffer is ready before sending data over EUSART
                if (EUSART2_is_tx_ready())
                {
                    // Print received data over EUSART
                    printf("Received data: %x \r\n", receivedData);
                }
            }
            // Check if the received command is '2'
            else if (command == '2')
            {
                // Determine the data based on the command
                uint8_t data = 0b11101101;
                
                // Set chip select low
                CS_RD4_SetLow();
                // Exchange data
                uint8_t receivedData = SPI1_ExchangeByte(data);
                // Set chip select high
                CS_RD4_SetHigh();

                // Check if TX buffer is ready before sending data over EUSART
                if (EUSART2_is_tx_ready())
                {
                    // Print received data over EUSART
                    printf("Received data: %x \r\n", receivedData);
                }
            }
            else
            {
                // Invalid command
                printf("Invalid command received: %c \r\n", command);
            }
        }

        // Delay before the next iteration
        __delay_ms(DELAY_MS);
    }

    return 0;
}
