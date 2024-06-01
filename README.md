# Counter-Strike: 2 - One-Byte-Wallhack

Extremely simple wall-hack achieved by patching a single byte.  
**LINUX ONLY: THIS WILL NOT WORK ON ANYTHING BUT LINUX.**

## How it works

Patching the test instruction, in the IsOtherEnemy function, results in returning false for every call.  
The game treats every player as a friendly. This means the game shows the overhead indicator and spectator glow on enemies.  
IsOtherEnemy (at least this one) is not used by any other relevant functions so the game does not break because of this.

```
bool IsOtherEnemy(C_BaseEntity* player1, C_BaseEntity* player2)
    31 c0           XOR        EAX,EAX  ; clears return value
    48 85 f6        TEST       RSI,RSI  ; check if player2 is null
    0f 84 85        JZ         LABEL_EXIT_EARLY ; return cleared return value => return false;
    00 00 00
    55              PUSH       RBP      ; Normal function prologue
    48 89 e5        MOV        RBP,RSP
    41 54           PUSH       R12
    49 89 fc        MOV        R12,RDI
    53              PUSH       RBX
    ; and so on...
    
    LABEL_EXIT_EARLY:
    c3              RET
```