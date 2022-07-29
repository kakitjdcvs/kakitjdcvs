ARR_INPUT 		EQU	 	0x2000
ARR_POSITIVE    EQU	 	0x3000
ARR_NEGATIVE 	EQU     0x5000
LENGTH			EQU		0x0100
RES_ARR_1		EQU		0x0500
RES_ARR_2		EQU		0x0504


					AREA djb, COPY, READONLY
					ENTRY 
					EXPORT __main

__main 				MOV R0, #0        				; int i
					MOV R1, ARR_POSITIVE            
					MOV R2, ARR_NEGATIVE
					MOV R4, #0 						;LEN1
					MOV R5, #0 						;LEN2

;
__loop				LDR R3, ARR_INPUT               ; a[i]
					CMP R3, #0x0000
					BLLT __store_arr_pos
					BLGT __store_arr_neg
					ADD R3, R3, #4
					ADD R0, R0, #1
					CMP R0, LENGTH
					BNE __loop
					BEQ __sort_pos

__store_arr_pos		STR R3, [R1]
					ADD R1, R1, #4
					ADD R4, R4, #1
					MOV PC, LR			 ; Return from function

__store_arr_neg    	STR R3, [R2]
					ADD R2, R2, #4
					ADD R5, R5, #1
					MOV PC, LR			 ; Return from function

;
__sort_pos			MOV R0, #0 			 ; int i = 0
					MOV R1, #0 			 ; int j = 0
					B __sort_pos
					LDR R2, ARR_POSITIVE ;
					LDR R3, ARR_POSITIVE ;
					
__loop1 			CMP R0, R4            ;
					BEQ __save1           ; 

__loop2				ADD R1, R0, #1        ; j = i + 1
					LDR R6, [R2]		  ; a[i] = [R2]
					ADD R3, R3, #4        
					LDR R7, [R3] 		  ; a[j] = [R2 + 4]
					CMP R6,	R7            ; a[i]<>a[j]
					BGT SWAP1             ;

					CMP R1, R4            ;j <.> N
					BNE __loop2	

					ADD R2, R2, #4        ;
					ADD R0, R0, #1        ;
					B __loop1

__SWAP1				STR R6, [R3]	     ; Table[j] = R6
					STR R7, [R2]		 ; Table[i] = R7
					MOV PC, LR			 ; Return from function

__save1             LDR R8, ARR_POSITIVE
					ADD R8, R0, #8
					STR R8, RES_ARR_1
					B __sort_neg

;
__sort_neg			MOV R0, #0 			 ; int i = 0
					MOV R1, #0 			 ; int j = 0
					B __sort_pos
					LDR R2, ARR_NEGATIVE ;
					LDR R3, ARR_NEGATIVE ;
					
__loop3 			CMP R0, R5            ;
					BEQ __save1           ;

__loop4				ADD R1, R0, #1        ; j = i + 1
					LDR R6, [R2]		  ; a[i] = [R2]
					ADD R3, R3, #4        
					LDR R7, [R3] 		  ; a[j] = [R2 + 4]
					CMP R6,	R7            ;
					BLT SWAP2             ;

					CMP R1, R5            ;
					BNE __loop4	

					ADD R2, R2, #4        ;
					ADD R0, R0, #1        ;
					B __loop3

__SWAP2				STR R6, [R3]	     ; Table[j] = R6
					STR R7, [R2]		 ; Table[i] = R7
					MOV PC, LR			 ; Return from function

__save2             LDR R9, ARR_POSITIVE
					ADD R9, R0, #8
					STR R9, RES_ARR_2

					END
