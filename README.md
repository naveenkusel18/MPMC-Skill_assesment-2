# Assembly language program in 8051 to generate a 250 ms delay using Timer 1 (Mode 1) and blink an LED connected to Port 0.5 continuously.
# Aim:
To write and execute an assembly language program to generate a 250 ms delay using Timer 1 in Mode 1 and use it to continuously blink an LED connected to Port pin P0.5 of the 8051 microcontroller.
# Apparatus / Software Required:
+Keil µVision IDE
# Algorithm:
+Start the program and initialize Timer 1 in Mode 1 (16-bit timer mode) using the TMOD register.

+Configure Port 0.5 as output pin for the LED.

+Turn LED ON by setting P0.5 = 1 (active low connection assumed).

+Call the delay subroutine to generate a 250 ms delay.

+Turn LED OFF by clearing P0.5 = 0.

+Call the delay again for the same duration.

+Repeat the ON–OFF cycle continuously to make the LED blink.

+The delay subroutine uses Timer 1 with values TH1 = 3CH and TL1 = 0B0H, generating about 50 ms delay per overflow.

+The delay is repeated 5 times (5 × 50 ms = 250 ms).
# Program
```asm
ORG 0000H                    ; Program start address
MAIN:
    MOV TMOD, #10H           ; Set Timer 1 to Mode 1 (16-bit timer)
	LOOP:
        SETB P0.5              ; Initialize Port 0 as output (LED off initially, assuming active low)
        ACALL DELAY          ; Call 250ms delay subroutine
        CLR P0.5		         ; Toggle P0.5 to blink LED
		ACALL DELAY          ; Call 250ms delay subroutine
        SJMP LOOP            ; Repeat indefinitely
; Subroutine for 250ms delay using Timer 1
DELAY:
    MOV R7, #5               ; Loop counter for 5 x 50ms = 250ms
DELAY_LOOP:
    MOV TH1, #03CH           ; Load TH1 for ~50ms delay (at 11.0592 MHz)
    MOV TL1, #0B0H           ; Load TL1
    SETB TR1                 ; Start Timer 1
WAIT:
    JNB TF1, WAIT            ; Wait for Timer 1 overflow flag
    CLR TF1                  ; Clear overflow flag
    CLR TR1                  ; Stop Timer 1
    DJNZ R7, DELAY_LOOP      ; Decrement counter and repeat if not zero
    RET                      ; Return from subroutine
END
```
# Output

![WhatsApp Image 2025-11-22 at 08 00 18_cb3636a8](https://github.com/user-attachments/assets/8871de49-26e6-4b72-be4e-3efff6df93fd)


# Result
The LED connected to Port 0.5 blinks continuously with an ON time of 250 ms and OFF time of 250 ms, successfully demonstrating delay generation using Timer 1 in Mode 1.
