


.text– qui stocke les instructions qui consistent en le programme lui-même. Il est marqué comme exécutable et en lecture seule (r-x).
.data – qui est utilisé pour stocker des variables statiques et globales (les variables non statiques sont stockées sur la pile). Il est marqué comme lecture-écriture et non exécutable (rw-).
.bss – qui stocke des variables non initialisées. Il est marqué comme lecture-écriture et non exécutable (rw-).
.rodata – qui stocke des données constantes. Il faut s’attendre à ce que des strings et d’autres valeurs constantes  soit stocké dans rodata. c'est pour une lecture seule
db = define bytes (8 bits)
dw = define word (16 bits) 
dd = define double word (32 bits)


**3. Instructions de base :**

- jmp : jump vers une étiquette call : appelle les instructions d'une étiquette
- movzx/movsx: movzx permet la conversion des nombres naturels en plus grand format alors que movsx le fais pour des nombres entiers. 
- mov 'destination', 'source' (valeur dans regsitre)
- sub : soustraction 
- add : addition 
- div : division 
- xor: XOR ce base sur sa table de vérité: 1 et 1 = 0 / 0 et 1 = 1 
- rol/ror: rotation de bits l=left et r=right 
- mul/imul: imul effectue une multiplication signée alors que mul peux uniquement sur du non-signé soit entièrement Positive
- pop : dépile dans la stack 
- push : empile en haut de la stack
- syscall: appel système 
- ret : return 
- inc : incrémentation de 1 
- loop équivalent à while, c'est une boucle. 
- lea : Cette instruction permet d'incrémenter un registre ou un emplacement mémoire.


**4. Hello World ! : **

```asm
BITS 64

global start ; commence à start

section .rodata
    hello db "Hello World !", 0xa, 0x0 ; le str "hello world" avec 0xa pour le saut de ligne
    hello_lengh equ $-hello ; calcul de la taille
    
section .text ; démarrage du code

start: ; label start
      mov rax, 0x1 ; mettre 1 dans rax ( sorti standard )
      mov rdi, 0x1 ; mettre 1 dans rdi
      mov rsi, hello ; mettre le str hello dans rsi
      mov rdx, hello_lengh ; mettre la taille (lengh) de hello dans rdx
      syscall ; appel systeme pour charger le bloc d'instruction avec la sorti standard
      jmp exit ; jump vers exit
      
exit:
      mov rax, 0x3c ; appel du syscall 60, le mettre dans rax
      xor rdi, rdi ; mettre rdi à 0 via xor
      syscall ; appel systeme
```

4.1 Compilation :
```
nasm -f elf64 -o hello.o hello.asm && ld -o hello hello.o
```

**5. Sauts Conditionnels**

- JE : saut si égal
- JZ : saut si résultat est zéro
- JG : saut si plus grand
- JEG : saut si plus grand ou égal (equal or above)
- JL : saut si plus petit (less)
- JC : saut si retenue (carry)
- JNO : saut si pas déborbement (not overflow)
- ...

**6. boucles**

si on considère cette boucle `for (cx=0; cx<5; cx++){ ax = ax + cx }`, en assembleur sa ressemblerais à ça :

```asm
xor rax, rax 
xor rcx, rcx

boucle:
    cmp rcx, 5 ; compare CX à 5
    jge done ; si CX >= 5, on sort de la boucle
    add ax, cx ; fait le calcul
    inc cx ; CX est incrémenté de 1
    jmp boucle ; on reprend au début de la boucle
done:
     mov rax, 0x3c
     xor rdi, rdi
     syscall
```
 on commence par initaliser les registres rax, et rcx (counter) puis on compare rcx à 5 avec JGE (plus grand ou égal) si c'est le cas on quitte, sinon on commence le calcul
 et on incrémente de 1 avec inc le counter et on fais un jmp pour répeter la bloc d'instruction.
