# 资料

## 题目集

```
https://www.cnblogs.com/henry-1202/
```

# 汇编

## 大小写互换

```
data segment
s db 100 dup(0)
t db 100 dup(0)
data ends
code segment
assume cs:code, ds:data
main:
  mov ax, data
  mov ds, ax ;初始化
  mov si, -1
  mov di, 0 ;寄存器赋初值
next:
  add si, 1 ;循环计时器加1
  mov ah, 01h
  int 21h ;调用int 21h的01h功能
  cmp al, 0dh
  je done ;若等于回车则跳到输出函数
  mov s[si], al ;将输入值赋给s数组
  cmp al, 20h
  je next ;若等于空格则直接执行下一循环
  cmp al, 'a'
  jb again
  cmp al, 'z'
  ja again ;若不是小写字母则跳到数组赋值函数
  sub s[si], 20h ;若是小写字母则将小写改为大写
again:
  mov bl, s[si]
  mov t[di], bl ;将s中的元素赋给t
  add di, 1
  jmp next ;回到读数循环
done:
  mov si, -1
  mov ah, 02h
  mov dl, 0dh
  int 21h
  mov ah, 02h
  mov dl, 0ah
  int 21h ;输出回车换行
output:
  add si, 1
  mov dl, t[si]
  mov ah, 02h
  int 21h ;输出t中的元素
  cmp si, di
  jne output ;判断是否到达t的末尾
exit:
  mov ah, 02h
  mov dl, 0dh
  int 21h
  mov ah, 02h
  mov dl, 0ah
  int 21h ;输出回车换行
  mov ah, 4Ch
  int 21h ;退出程序
code ends
end main
```

## 画图案

```
data segment
asc db 000h,000h,000h,000h,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,000h,000h,000h
    db 000h,000h,07Eh,081h,0A5h,081h,081h,0BDh
    db 099h,081h,081h,07Eh,000h,000h,000h,000h
    db 000h,000h,07Ch,0FEh,0FEh,0D6h,0FEh,0FEh
    db 0BAh,0C6h,0FEh,07Ch,000h,000h,000h,000h
    db 000h,000h,000h,06Ch,0EEh,0FEh,0FEh,0FEh
    db 0FEh,07Ch,038h,010h,000h,000h,000h,000h
    db 000h,000h,000h,010h,038h,07Ch,0FEh,07Ch
    db 038h,010h,000h,000h,000h,000h,000h,000h
    db 000h,000h,000h,010h,038h,038h,010h,06Ch
    db 0EEh,06Ch,010h,038h,000h,000h,000h,000h
    db 000h,000h,010h,038h,07Ch,07Ch,0FEh,0FEh
    db 0FEh,06Ch,010h,038h,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,018h,03Ch,03Ch
    db 03Ch,018h,000h,000h,000h,000h,000h,000h
    db 0FFh,0FFh,0FFh,0FFh,0FFh,0E7h,0C3h,0C3h
    db 0C3h,0E7h,0FFh,0FFh,0FFh,0FFh,0FFh,0FFh
    db 000h,000h,000h,000h,018h,03Ch,066h,066h
    db 066h,03Ch,018h,000h,000h,000h,000h,000h
    db 0FFh,0FFh,0FFh,0FFh,0E7h,0C3h,099h,099h
    db 099h,0C3h,0E7h,0FFh,0FFh,0FFh,0FFh,0FFh
    db 000h,000h,01Eh,00Eh,01Eh,036h,078h,0CCh
    db 0CCh,0CCh,0CCh,078h,000h,000h,000h,000h
    db 000h,000h,03Ch,066h,066h,066h,03Ch,018h
    db 07Eh,018h,018h,018h,000h,000h,000h,000h
    db 000h,000h,01Eh,01Ah,01Eh,018h,018h,018h
    db 018h,078h,0F8h,070h,000h,000h,000h,000h
    db 000h,000h,03Eh,036h,03Eh,036h,036h,076h
    db 0F6h,066h,00Eh,01Eh,00Ch,000h,000h,000h
    db 000h,000h,018h,0DBh,07Eh,03Ch,066h,066h
    db 03Ch,07Eh,0DBh,018h,000h,000h,000h,000h
    db 000h,000h,000h,080h,0E0h,0F0h,0FCh,0FEh
    db 0FCh,0F0h,0E0h,080h,000h,000h,000h,000h
    db 000h,000h,000h,002h,00Eh,03Eh,07Eh,0FEh
    db 07Eh,03Eh,00Eh,002h,000h,000h,000h,000h
    db 000h,000h,018h,03Ch,07Eh,018h,018h,018h
    db 018h,07Eh,03Ch,018h,000h,000h,000h,000h
    db 000h,000h,066h,066h,066h,066h,066h,066h
    db 066h,000h,066h,066h,000h,000h,000h,000h
    db 000h,000h,07Fh,0DBh,0DBh,0DBh,0DBh,07Bh
    db 01Bh,01Bh,01Bh,01Bh,000h,000h,000h,000h
    db 000h,000h,07Ch,0C6h,0C6h,060h,07Ch,0F6h
    db 0DEh,07Ch,00Ch,0C6h,0C6h,07Ch,000h,000h
    db 000h,000h,000h,000h,000h,000h,000h,000h
    db 0FEh,0FEh,0FEh,0FEh,000h,000h,000h,000h
    db 000h,000h,018h,03Ch,07Eh,018h,018h,018h
    db 07Eh,03Ch,018h,07Eh,000h,000h,000h,000h
    db 000h,000h,018h,03Ch,07Eh,018h,018h,018h
    db 018h,018h,018h,018h,000h,000h,000h,000h
    db 000h,000h,018h,018h,018h,018h,018h,018h
    db 018h,07Eh,03Ch,018h,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,00Ch,00Eh,0FFh
    db 00Eh,00Ch,000h,000h,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,030h,070h,0FEh
    db 070h,030h,000h,000h,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,000h,0C0h,0C0h
    db 0C0h,0FEh,000h,000h,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,024h,066h,0FFh
    db 066h,024h,000h,000h,000h,000h,000h,000h
    db 000h,000h,000h,010h,038h,038h,038h,07Ch
    db 07Ch,0FEh,0FEh,000h,000h,000h,000h,000h
    db 000h,000h,000h,0FEh,0FEh,07Ch,07Ch,07Ch
    db 038h,038h,010h,000h,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,000h,000h,000h
    db 000h,000h,018h,03Ch,03Ch,03Ch,03Ch,018h
    db 018h,000h,018h,018h,000h,000h,000h,000h
    db 000h,036h,036h,036h,036h,014h,000h,000h
    db 000h,000h,000h,000h,000h,000h,000h,000h
    db 000h,000h,06Ch,06Ch,06Ch,0FEh,06Ch,06Ch
    db 0FEh,06Ch,06Ch,06Ch,000h,000h,000h,000h
    db 000h,000h,018h,018h,07Ch,0C6h,0C0h,078h
    db 03Ch,006h,0C6h,07Ch,018h,018h,000h,000h
    db 000h,000h,000h,000h,000h,062h,066h,00Ch
    db 018h,030h,066h,0C6h,000h,000h,000h,000h
    db 000h,000h,038h,06Ch,038h,030h,076h,07Eh
    db 0CCh,0CCh,0CCh,076h,000h,000h,000h,000h
    db 000h,00Ch,00Ch,00Ch,018h,000h,000h,000h
    db 000h,000h,000h,000h,000h,000h,000h,000h
    db 000h,000h,00Ch,018h,030h,030h,030h,030h
    db 030h,030h,018h,00Ch,000h,000h,000h,000h
    db 000h,000h,030h,018h,00Ch,00Ch,00Ch,00Ch
    db 00Ch,00Ch,018h,030h,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,06Ch,038h,0FEh
    db 038h,06Ch,000h,000h,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,018h,018h,07Eh
    db 018h,018h,000h,000h,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,000h,000h,000h
    db 000h,00Ch,00Ch,00Ch,018h,000h,000h,000h
    db 000h,000h,000h,000h,000h,000h,000h,0FEh
    db 000h,000h,000h,000h,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,000h,000h,000h
    db 000h,000h,018h,018h,000h,000h,000h,000h
    db 000h,000h,000h,000h,002h,006h,00Ch,018h
    db 030h,060h,0C0h,080h,000h,000h,000h,000h
    db 000h,000h,07Ch,0C6h,0C6h,0CEh,0DEh,0F6h
    db 0E6h,0C6h,0C6h,07Ch,000h,000h,000h,000h
    db 000h,000h,018h,078h,018h,018h,018h,018h
    db 018h,018h,018h,07Eh,000h,000h,000h,000h
    db 000h,000h,07Ch,0C6h,0C6h,006h,00Ch,018h
    db 030h,060h,0C6h,0FEh,000h,000h,000h,000h
    db 000h,000h,07Ch,0C6h,006h,006h,03Ch,006h
    db 006h,006h,0C6h,07Ch,000h,000h,000h,000h
    db 000h,000h,00Ch,01Ch,03Ch,06Ch,0CCh,0CCh
    db 0FEh,00Ch,00Ch,01Eh,000h,000h,000h,000h
    db 000h,000h,0FEh,0C0h,0C0h,0C0h,0FCh,006h
    db 006h,006h,0C6h,07Ch,000h,000h,000h,000h
    db 000h,000h,07Ch,0C6h,0C0h,0C0h,0FCh,0C6h
    db 0C6h,0C6h,0C6h,07Ch,000h,000h,000h,000h
    db 000h,000h,0FEh,0C6h,006h,00Ch,018h,030h
    db 030h,030h,030h,030h,000h,000h,000h,000h
    db 000h,000h,07Ch,0C6h,0C6h,0C6h,07Ch,0C6h
    db 0C6h,0C6h,0C6h,07Ch,000h,000h,000h,000h
    db 000h,000h,07Ch,0C6h,0C6h,0C6h,0C6h,07Eh
    db 006h,006h,0C6h,07Ch,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,00Ch,00Ch,000h
    db 000h,00Ch,00Ch,000h,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,00Ch,00Ch,000h
    db 000h,00Ch,00Ch,00Ch,018h,000h,000h,000h
    db 000h,000h,000h,00Ch,018h,030h,060h,0C0h
    db 060h,030h,018h,00Ch,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,000h,0FEh,000h
    db 0FEh,000h,000h,000h,000h,000h,000h,000h
    db 000h,000h,000h,060h,030h,018h,00Ch,006h
    db 00Ch,018h,030h,060h,000h,000h,000h,000h
    db 000h,000h,07Ch,0C6h,0C6h,00Ch,018h,018h
    db 018h,000h,018h,018h,000h,000h,000h,000h
    db 000h,000h,07Ch,0C6h,0C6h,0C6h,0DEh,0DEh
    db 0DEh,0DCh,0C0h,07Eh,000h,000h,000h,000h
    db 000h,000h,038h,06Ch,0C6h,0C6h,0C6h,0FEh
    db 0C6h,0C6h,0C6h,0C6h,000h,000h,000h,000h
    db 000h,000h,0FCh,066h,066h,066h,07Ch,066h
    db 066h,066h,066h,0FCh,000h,000h,000h,000h
    db 000h,000h,03Ch,066h,0C2h,0C0h,0C0h,0C0h
    db 0C0h,0C2h,066h,03Ch,000h,000h,000h,000h
    db 000h,000h,0F8h,06Ch,066h,066h,066h,066h
    db 066h,066h,06Ch,0F8h,000h,000h,000h,000h
    db 000h,000h,0FEh,066h,060h,064h,07Ch,064h
    db 060h,060h,066h,0FEh,000h,000h,000h,000h
    db 000h,000h,0FEh,066h,060h,064h,07Ch,064h
    db 060h,060h,060h,0F0h,000h,000h,000h,000h
    db 000h,000h,07Ch,0C6h,0C6h,0C0h,0C0h,0C0h
    db 0CEh,0C6h,0C6h,07Ch,000h,000h,000h,000h
    db 000h,000h,0C6h,0C6h,0C6h,0C6h,0FEh,0C6h
    db 0C6h,0C6h,0C6h,0C6h,000h,000h,000h,000h
    db 000h,000h,03Ch,018h,018h,018h,018h,018h
    db 018h,018h,018h,03Ch,000h,000h,000h,000h
    db 000h,000h,03Ch,018h,018h,018h,018h,018h
    db 018h,0D8h,0D8h,070h,000h,000h,000h,000h
    db 000h,000h,0C6h,0C6h,0CCh,0D8h,0F0h,0F0h
    db 0D8h,0CCh,0C6h,0C6h,000h,000h,000h,000h
    db 000h,000h,0F0h,060h,060h,060h,060h,060h
    db 060h,062h,066h,0FEh,000h,000h,000h,000h
    db 000h,000h,0C6h,0C6h,0EEh,0EEh,0FEh,0D6h
    db 0D6h,0D6h,0C6h,0C6h,000h,000h,000h,000h
    db 000h,000h,0C6h,0C6h,0E6h,0E6h,0F6h,0DEh
    db 0CEh,0CEh,0C6h,0C6h,000h,000h,000h,000h
    db 000h,000h,07Ch,0C6h,0C6h,0C6h,0C6h,0C6h
    db 0C6h,0C6h,0C6h,07Ch,000h,000h,000h,000h
    db 000h,000h,0FCh,066h,066h,066h,066h,07Ch
    db 060h,060h,060h,0F0h,000h,000h,000h,000h
    db 000h,000h,07Ch,0C6h,0C6h,0C6h,0C6h,0C6h
    db 0C6h,0D6h,0D6h,07Ch,006h,000h,000h,000h
    db 000h,000h,0FCh,066h,066h,066h,07Ch,078h
    db 06Ch,066h,066h,0E6h,000h,000h,000h,000h
    db 000h,000h,07Ch,0C6h,0C0h,0C0h,070h,01Ch
    db 006h,006h,0C6h,07Ch,000h,000h,000h,000h
    db 000h,000h,07Eh,05Ah,018h,018h,018h,018h
    db 018h,018h,018h,03Ch,000h,000h,000h,000h
    db 000h,000h,0C6h,0C6h,0C6h,0C6h,0C6h,0C6h
    db 0C6h,0C6h,0C6h,07Ch,000h,000h,000h,000h
    db 000h,000h,0C6h,0C6h,0C6h,0C6h,0C6h,0C6h
    db 0C6h,06Ch,038h,010h,000h,000h,000h,000h
    db 000h,000h,0C6h,0C6h,0C6h,0D6h,0D6h,0D6h
    db 0FEh,0EEh,0C6h,0C6h,000h,000h,000h,000h
    db 000h,000h,0C6h,0C6h,0C6h,06Ch,038h,038h
    db 06Ch,0C6h,0C6h,0C6h,000h,000h,000h,000h
    db 000h,000h,066h,066h,066h,066h,066h,03Ch
    db 018h,018h,018h,03Ch,000h,000h,000h,000h
    db 000h,000h,0FEh,0C6h,086h,00Ch,018h,030h
    db 060h,0C2h,0C6h,0FEh,000h,000h,000h,000h
    db 000h,000h,07Ch,060h,060h,060h,060h,060h
    db 060h,060h,060h,07Ch,000h,000h,000h,000h
    db 000h,000h,000h,000h,080h,0C0h,060h,030h
    db 018h,00Ch,006h,002h,000h,000h,000h,000h
    db 000h,000h,07Ch,00Ch,00Ch,00Ch,00Ch,00Ch
    db 00Ch,00Ch,00Ch,07Ch,000h,000h,000h,000h
    db 000h,010h,038h,06Ch,0C6h,000h,000h,000h
    db 000h,000h,000h,000h,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,0FFh,000h,000h
    db 000h,018h,018h,018h,00Ch,000h,000h,000h
    db 000h,000h,000h,000h,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,078h,00Ch,07Ch
    db 0CCh,0CCh,0DCh,076h,000h,000h,000h,000h
    db 000h,000h,0E0h,060h,060h,07Ch,066h,066h
    db 066h,066h,066h,0FCh,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,07Ch,0C6h,0C0h
    db 0C0h,0C0h,0C6h,07Ch,000h,000h,000h,000h
    db 000h,000h,01Ch,00Ch,00Ch,07Ch,0CCh,0CCh
    db 0CCh,0CCh,0CCh,07Eh,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,07Ch,0C6h,0C6h
    db 0FEh,0C0h,0C6h,07Ch,000h,000h,000h,000h
    db 000h,000h,01Ch,036h,030h,030h,0FCh,030h
    db 030h,030h,030h,078h,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,076h,0CEh,0C6h
    db 0C6h,0CEh,076h,006h,0C6h,07Ch,000h,000h
    db 000h,000h,0E0h,060h,060h,07Ch,066h,066h
    db 066h,066h,066h,0E6h,000h,000h,000h,000h
    db 000h,000h,018h,018h,000h,038h,018h,018h
    db 018h,018h,018h,03Ch,000h,000h,000h,000h
    db 000h,000h,00Ch,00Ch,000h,01Ch,00Ch,00Ch
    db 00Ch,00Ch,00Ch,0CCh,0CCh,078h,000h,000h
    db 000h,000h,0E0h,060h,060h,066h,066h,06Ch
    db 078h,06Ch,066h,0E6h,000h,000h,000h,000h
    db 000h,000h,018h,018h,018h,018h,018h,018h
    db 018h,018h,018h,01Ch,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,06Ch,0FEh,0D6h
    db 0D6h,0C6h,0C6h,0C6h,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,0DCh,066h,066h
    db 066h,066h,066h,066h,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,07Ch,0C6h,0C6h
    db 0C6h,0C6h,0C6h,07Ch,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,0DCh,066h,066h
    db 066h,066h,07Ch,060h,060h,0F0h,000h,000h
    db 000h,000h,000h,000h,000h,076h,0CCh,0CCh
    db 0CCh,0CCh,07Ch,00Ch,00Ch,01Eh,000h,000h
    db 000h,000h,000h,000h,000h,0DCh,066h,060h
    db 060h,060h,060h,0F0h,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,07Ch,0C6h,0C0h
    db 07Ch,006h,0C6h,07Ch,000h,000h,000h,000h
    db 000h,000h,030h,030h,030h,0FCh,030h,030h
    db 030h,030h,036h,01Ch,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,0CCh,0CCh,0CCh
    db 0CCh,0CCh,0CCh,076h,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,0C6h,0C6h,0C6h
    db 0C6h,06Ch,038h,010h,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,0C6h,0C6h,0D6h
    db 0D6h,0D6h,0FEh,06Ch,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,0C6h,0C6h,06Ch
    db 038h,06Ch,0C6h,0C6h,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,0C6h,0C6h,0C6h
    db 0C6h,0CEh,076h,006h,0C6h,07Ch,000h,000h
    db 000h,000h,000h,000h,000h,0FEh,086h,00Ch
    db 018h,030h,062h,0FEh,000h,000h,000h,000h
    db 000h,000h,00Eh,018h,018h,018h,070h,018h
    db 018h,018h,018h,00Eh,000h,000h,000h,000h
    db 000h,000h,018h,018h,018h,018h,000h,018h
    db 018h,018h,018h,018h,000h,000h,000h,000h
    db 000h,000h,070h,018h,018h,018h,00Eh,018h
    db 018h,018h,018h,070h,000h,000h,000h,000h
    db 000h,000h,076h,0DCh,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,010h,038h,038h
    db 06Ch,06Ch,0FEh,000h,000h,000h,000h,000h
    db 000h,000h,03Ch,066h,0C0h,0C0h,0C0h,0C6h
    db 066h,03Ch,018h,00Ch,0CCh,038h,000h,000h
    db 000h,000h,0C6h,000h,000h,0C6h,0C6h,0C6h
    db 0C6h,0C6h,0CEh,076h,000h,000h,000h,000h
    db 000h,00Ch,018h,030h,000h,07Ch,0C6h,0C6h
    db 0FEh,0C0h,0C6h,07Ch,000h,000h,000h,000h
    db 000h,030h,078h,0CCh,000h,078h,00Ch,07Ch
    db 0CCh,0CCh,0DCh,076h,000h,000h,000h,000h
    db 000h,000h,0CCh,000h,000h,078h,00Ch,07Ch
    db 0CCh,0CCh,0DCh,076h,000h,000h,000h,000h
    db 000h,060h,030h,018h,000h,078h,00Ch,07Ch
    db 0CCh,0CCh,0DCh,076h,000h,000h,000h,000h
    db 000h,038h,06Ch,038h,000h,078h,00Ch,07Ch
    db 0CCh,0CCh,0DCh,076h,000h,000h,000h,000h
    db 000h,000h,000h,000h,07Ch,0C6h,0C0h,0C0h
    db 0C6h,07Ch,018h,00Ch,06Ch,038h,000h,000h
    db 000h,030h,078h,0CCh,000h,07Ch,0C6h,0C6h
    db 0FEh,0C0h,0C6h,07Ch,000h,000h,000h,000h
    db 000h,000h,0CCh,000h,000h,07Ch,0C6h,0C6h
    db 0FEh,0C0h,0C6h,07Ch,000h,000h,000h,000h
    db 000h,030h,018h,00Ch,000h,07Ch,0C6h,0C6h
    db 0FEh,0C0h,0C6h,07Ch,000h,000h,000h,000h
    db 000h,000h,066h,000h,000h,038h,018h,018h
    db 018h,018h,018h,03Ch,000h,000h,000h,000h
    db 000h,018h,03Ch,066h,000h,038h,018h,018h
    db 018h,018h,018h,03Ch,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,038h,018h,018h
    db 018h,018h,018h,03Ch,000h,000h,000h,000h
    db 000h,0C6h,000h,038h,06Ch,0C6h,0C6h,0C6h
    db 0FEh,0C6h,0C6h,0C6h,000h,000h,000h,000h
    db 038h,06Ch,038h,000h,038h,06Ch,0C6h,0C6h
    db 0FEh,0C6h,0C6h,0C6h,000h,000h,000h,000h
    db 00Ch,018h,030h,000h,0FEh,060h,060h,07Ch
    db 060h,060h,060h,0FEh,000h,000h,000h,000h
    db 000h,000h,000h,000h,066h,0DBh,01Bh,07Fh
    db 0D8h,0D8h,0DFh,076h,000h,000h,000h,000h
    db 000h,000h,07Eh,0D8h,0D8h,0D8h,0D8h,0FEh
    db 0D8h,0D8h,0D8h,0DEh,000h,000h,000h,000h
    db 000h,030h,078h,0CCh,000h,07Ch,0C6h,0C6h
    db 0C6h,0C6h,0C6h,07Ch,000h,000h,000h,000h
    db 000h,000h,0C6h,000h,000h,07Ch,0C6h,0C6h
    db 0C6h,0C6h,0C6h,07Ch,000h,000h,000h,000h
    db 000h,030h,018h,00Ch,000h,07Ch,0C6h,0C6h
    db 0C6h,0C6h,0C6h,07Ch,000h,000h,000h,000h
    db 000h,030h,078h,0CCh,000h,0C6h,0C6h,0C6h
    db 0C6h,0C6h,0CEh,076h,000h,000h,000h,000h
    db 000h,060h,030h,018h,000h,0C6h,0C6h,0C6h
    db 0C6h,0C6h,0CEh,076h,000h,000h,000h,000h
    db 000h,018h,000h,03Ch,018h,018h,018h,018h
    db 018h,018h,018h,03Ch,000h,000h,000h,000h
    db 000h,0C6h,000h,07Ch,0C6h,0C6h,0C6h,0C6h
    db 0C6h,0C6h,0C6h,07Ch,000h,000h,000h,000h
    db 000h,0C6h,000h,0C6h,0C6h,0C6h,0C6h,0C6h
    db 0C6h,0C6h,0C6h,07Ch,000h,000h,000h,000h
    db 000h,000h,018h,018h,07Ch,0C6h,0C0h,0C0h
    db 0C6h,07Ch,018h,018h,000h,000h,000h,000h
    db 000h,038h,06Ch,060h,060h,0F0h,060h,060h
    db 060h,066h,0F6h,06Ch,000h,000h,000h,000h
    db 000h,066h,066h,066h,066h,03Ch,018h,07Eh
    db 018h,03Ch,018h,018h,000h,000h,000h,000h
    db 000h,000h,03Eh,063h,063h,030h,01Ch,006h
    db 063h,063h,03Eh,000h,01Ch,000h,000h,000h
    db 000h,000h,000h,000h,000h,03Eh,063h,038h
    db 00Eh,063h,03Eh,000h,01Ch,000h,000h,000h
    db 000h,00Ch,018h,030h,000h,078h,00Ch,07Ch
    db 0CCh,0CCh,0DCh,076h,000h,000h,000h,000h
    db 000h,00Ch,018h,030h,000h,038h,018h,018h
    db 018h,018h,018h,03Ch,000h,000h,000h,000h
    db 000h,00Ch,018h,030h,000h,07Ch,0C6h,0C6h
    db 0C6h,0C6h,0C6h,07Ch,000h,000h,000h,000h
    db 000h,018h,030h,060h,000h,0CCh,0CCh,0CCh
    db 0CCh,0CCh,0DCh,076h,000h,000h,000h,000h
    db 000h,000h,076h,0DCh,000h,0BCh,066h,066h
    db 066h,066h,066h,0E6h,000h,000h,000h,000h
    db 000h,076h,0DCh,000h,0C6h,0C6h,0E6h,0F6h
    db 0DEh,0CEh,0C6h,0C6h,000h,000h,000h,000h
    db 000h,021h,01Eh,000h,01Eh,033h,060h,060h
    db 067h,063h,033h,01Dh,000h,000h,000h,000h
    db 000h,042h,03Ch,000h,03Bh,066h,066h,066h
    db 03Eh,006h,066h,03Ch,000h,000h,000h,000h
    db 000h,000h,030h,030h,000h,030h,030h,030h
    db 060h,0C6h,0C6h,07Ch,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,000h,000h,07Eh
    db 060h,060h,060h,000h,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,000h,000h,07Eh
    db 006h,006h,006h,000h,000h,000h,000h,000h
    db 000h,060h,060h,062h,066h,06Ch,018h,030h
    db 060h,0DCh,036h,00Ch,018h,03Eh,000h,000h
    db 000h,060h,060h,062h,066h,06Ch,018h,036h
    db 06Eh,0DEh,036h,07Eh,006h,006h,000h,000h
    db 000h,000h,018h,018h,000h,018h,018h,03Ch
    db 03Ch,03Ch,03Ch,018h,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,036h,06Ch,0D8h
    db 06Ch,036h,000h,000h,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,0D8h,06Ch,036h
    db 06Ch,0D8h,000h,000h,000h,000h,000h,000h
    db 011h,044h,011h,044h,011h,044h,011h,044h
    db 011h,044h,011h,044h,011h,044h,011h,044h
    db 0AAh,055h,0AAh,055h,0AAh,055h,0AAh,055h
    db 0AAh,055h,0AAh,055h,0AAh,055h,0AAh,055h
    db 0DDh,077h,0DDh,077h,0DDh,077h,0DDh,077h
    db 0DDh,077h,0DDh,077h,0DDh,077h,0DDh,077h
    db 018h,018h,018h,018h,018h,018h,018h,018h
    db 018h,018h,018h,018h,018h,018h,018h,018h
    db 018h,018h,018h,018h,018h,018h,018h,0F8h
    db 018h,018h,018h,018h,018h,018h,018h,018h
    db 018h,018h,018h,018h,018h,0F8h,018h,0F8h
    db 018h,018h,018h,018h,018h,018h,018h,018h
    db 036h,036h,036h,036h,036h,036h,036h,0F6h
    db 036h,036h,036h,036h,036h,036h,036h,036h
    db 000h,000h,000h,000h,000h,000h,000h,0FEh
    db 036h,036h,036h,036h,036h,036h,036h,036h
    db 000h,000h,000h,000h,000h,0F8h,018h,0F8h
    db 018h,018h,018h,018h,018h,018h,018h,018h
    db 036h,036h,036h,036h,036h,0F6h,006h,0F6h
    db 036h,036h,036h,036h,036h,036h,036h,036h
    db 036h,036h,036h,036h,036h,036h,036h,036h
    db 036h,036h,036h,036h,036h,036h,036h,036h
    db 000h,000h,000h,000h,000h,0FEh,006h,0F6h
    db 036h,036h,036h,036h,036h,036h,036h,036h
    db 036h,036h,036h,036h,036h,0F6h,006h,0FEh
    db 000h,000h,000h,000h,000h,000h,000h,000h
    db 036h,036h,036h,036h,036h,036h,036h,0FEh
    db 000h,000h,000h,000h,000h,000h,000h,000h
    db 018h,018h,018h,018h,018h,0F8h,018h,0F8h
    db 000h,000h,000h,000h,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,000h,000h,0F8h
    db 018h,018h,018h,018h,018h,018h,018h,018h
    db 018h,018h,018h,018h,018h,018h,018h,01Fh
    db 000h,000h,000h,000h,000h,000h,000h,000h
    db 018h,018h,018h,018h,018h,018h,018h,0FFh
    db 000h,000h,000h,000h,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,000h,000h,0FFh
    db 018h,018h,018h,018h,018h,018h,018h,018h
    db 018h,018h,018h,018h,018h,018h,018h,01Fh
    db 018h,018h,018h,018h,018h,018h,018h,018h
    db 000h,000h,000h,000h,000h,000h,000h,0FFh
    db 000h,000h,000h,000h,000h,000h,000h,000h
    db 018h,018h,018h,018h,018h,018h,018h,0FFh
    db 018h,018h,018h,018h,018h,018h,018h,018h
    db 018h,018h,018h,018h,018h,01Fh,018h,01Fh
    db 018h,018h,018h,018h,018h,018h,018h,018h
    db 036h,036h,036h,036h,036h,036h,036h,037h
    db 036h,036h,036h,036h,036h,036h,036h,036h
    db 036h,036h,036h,036h,036h,037h,030h,03Fh
    db 000h,000h,000h,000h,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,03Fh,030h,037h
    db 036h,036h,036h,036h,036h,036h,036h,036h
    db 036h,036h,036h,036h,036h,0F7h,000h,0FFh
    db 000h,000h,000h,000h,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,0FFh,000h,0F7h
    db 036h,036h,036h,036h,036h,036h,036h,036h
    db 036h,036h,036h,036h,036h,037h,030h,037h
    db 036h,036h,036h,036h,036h,036h,036h,036h
    db 000h,000h,000h,000h,000h,0FFh,000h,0FFh
    db 000h,000h,000h,000h,000h,000h,000h,000h
    db 036h,036h,036h,036h,036h,0F7h,000h,0F7h
    db 036h,036h,036h,036h,036h,036h,036h,036h
    db 018h,018h,018h,018h,018h,0FFh,000h,0FFh
    db 000h,000h,000h,000h,000h,000h,000h,000h
    db 036h,036h,036h,036h,036h,036h,036h,0FFh
    db 000h,000h,000h,000h,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,0FFh,000h,0FFh
    db 018h,018h,018h,018h,018h,018h,018h,018h
    db 000h,000h,000h,000h,000h,000h,000h,0FFh
    db 036h,036h,036h,036h,036h,036h,036h,036h
    db 036h,036h,036h,036h,036h,036h,036h,03Fh
    db 000h,000h,000h,000h,000h,000h,000h,000h
    db 018h,018h,018h,018h,018h,01Fh,018h,01Fh
    db 000h,000h,000h,000h,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,01Fh,018h,01Fh
    db 018h,018h,018h,018h,018h,018h,018h,018h
    db 000h,000h,000h,000h,000h,000h,000h,03Fh
    db 036h,036h,036h,036h,036h,036h,036h,036h
    db 036h,036h,036h,036h,036h,036h,036h,0FFh
    db 036h,036h,036h,036h,036h,036h,036h,036h
    db 018h,018h,018h,018h,018h,0FFh,018h,0FFh
    db 018h,018h,018h,018h,018h,018h,018h,018h
    db 018h,018h,018h,018h,018h,018h,018h,0F8h
    db 000h,000h,000h,000h,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,000h,000h,01Fh
    db 018h,018h,018h,018h,018h,018h,018h,018h
    db 0FFh,0FFh,0FFh,0FFh,0FFh,0FFh,0FFh,0FFh
    db 0FFh,0FFh,0FFh,0FFh,0FFh,0FFh,0FFh,0FFh
    db 000h,000h,000h,000h,000h,000h,000h,000h
    db 0FFh,0FFh,0FFh,0FFh,0FFh,0FFh,0FFh,0FFh
    db 0F0h,0F0h,0F0h,0F0h,0F0h,0F0h,0F0h,0F0h
    db 0F0h,0F0h,0F0h,0F0h,0F0h,0F0h,0F0h,0F0h
    db 00Fh,00Fh,00Fh,00Fh,00Fh,00Fh,00Fh,00Fh
    db 00Fh,00Fh,00Fh,00Fh,00Fh,00Fh,00Fh,00Fh
    db 0FFh,0FFh,0FFh,0FFh,0FFh,0FFh,0FFh,0FFh
    db 000h,000h,000h,000h,000h,000h,000h,000h
    db 000h,000h,000h,000h,076h,0DCh,0D8h,0D8h
    db 0D8h,0D8h,0DCh,076h,000h,000h,000h,000h
    db 000h,000h,078h,0CCh,0CCh,0D8h,0FCh,0C6h
    db 0C6h,0C6h,0C6h,0CCh,000h,000h,000h,000h
    db 000h,000h,0FEh,066h,062h,060h,060h,060h
    db 060h,060h,060h,060h,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,0FEh,06Ch,06Ch
    db 06Ch,06Ch,06Ch,06Ch,000h,000h,000h,000h
    db 000h,000h,0FEh,0C6h,062h,030h,018h,018h
    db 030h,062h,0C6h,0FEh,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,07Eh,0D8h,0CCh
    db 0CCh,0CCh,0D8h,070h,000h,000h,000h,000h
    db 000h,000h,000h,000h,066h,066h,066h,066h
    db 066h,07Ch,060h,0C0h,080h,000h,000h,000h
    db 000h,000h,000h,000h,000h,076h,0DCh,018h
    db 018h,018h,018h,018h,000h,000h,000h,000h
    db 000h,000h,0FEh,038h,038h,06Ch,0C6h,0C6h
    db 06Ch,038h,038h,0FEh,000h,000h,000h,000h
    db 000h,000h,000h,038h,06Ch,0C6h,0C6h,0FEh
    db 0C6h,0C6h,06Ch,038h,000h,000h,000h,000h
    db 000h,000h,038h,06Ch,0C6h,0C6h,0C6h,0C6h
    db 06Ch,06Ch,06Ch,0EEh,000h,000h,000h,000h
    db 000h,000h,03Eh,060h,060h,03Ch,066h,0C6h
    db 0C6h,0C6h,0CCh,078h,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,07Eh,0DBh,0DBh
    db 0DBh,07Eh,000h,000h,000h,000h,000h,000h
    db 000h,000h,002h,006h,07Ch,0CEh,0DEh,0F6h
    db 0F6h,07Ch,060h,0C0h,000h,000h,000h,000h
    db 000h,000h,000h,01Ch,030h,060h,060h,07Ch
    db 060h,060h,030h,01Ch,000h,000h,000h,000h
    db 000h,000h,07Ch,0C6h,0C6h,0C6h,0C6h,0C6h
    db 0C6h,0C6h,0C6h,0C6h,000h,000h,000h,000h
    db 000h,000h,000h,000h,0FEh,000h,000h,0FEh
    db 000h,000h,0FEh,000h,000h,000h,000h,000h
    db 000h,000h,000h,000h,018h,018h,07Eh,018h
    db 018h,000h,000h,07Eh,000h,000h,000h,000h
    db 000h,000h,030h,018h,00Ch,006h,00Ch,018h
    db 030h,000h,000h,07Eh,000h,000h,000h,000h
    db 000h,000h,00Ch,018h,030h,060h,030h,018h
    db 00Ch,000h,000h,07Eh,000h,000h,000h,000h
    db 000h,000h,000h,000h,00Ch,01Eh,01Ah,018h
    db 018h,018h,018h,018h,018h,018h,018h,018h
    db 018h,018h,018h,018h,018h,018h,018h,018h
    db 018h,018h,058h,078h,030h,000h,000h,000h
    db 000h,000h,000h,000h,018h,018h,000h,07Eh
    db 000h,018h,018h,000h,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,000h,076h,0DCh
    db 000h,076h,0DCh,000h,000h,000h,000h,000h
    db 000h,000h,078h,0CCh,0CCh,078h,000h,000h
    db 000h,000h,000h,000h,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,000h,000h,018h
    db 018h,000h,000h,000h,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,000h,000h,000h
    db 018h,000h,000h,000h,000h,000h,000h,000h
    db 000h,000h,01Fh,018h,018h,018h,018h,018h
    db 0D8h,0D8h,078h,038h,018h,000h,000h,000h
    db 000h,000h,0D8h,06Ch,06Ch,06Ch,06Ch,06Ch
    db 000h,000h,000h,000h,000h,000h,000h,000h
    db 000h,000h,070h,0D8h,018h,030h,060h,0F8h
    db 000h,000h,000h,000h,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,07Eh,07Eh,07Eh
    db 07Eh,07Eh,07Eh,000h,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,000h,000h,000h
    db 000h,000h,000h,000h,000h,000h,000h,000h
input db 100 dup(0)
t db "0123456789ABCDEF", 0Dh, 0Ah, '$'
data ends

code segment
assume cs:code, ds:data
main:
   mov ax, data
   mov ds, ax; 代码初始化
   mov si, -1
next:
   add si, 1
   mov ah, 1
   int 21h
   mov input[si], al; 循环读入字符
   cmp al, 0dh
   jne next; 遇回车则停止读入
   mov si, -1
change:
   add si, 1
   cmp input[si], 'a'
   jb again
   cmp input[si], 'f'
   ja again
   sub input[si], 20h; 将小写字母转换为大写
again:
   cmp si, 1
   jne change; 重复两次，将两个字符都进行转换
   mov si, -1
   mov al, input[0]
getnum1: 
   cmp si, 16
   je afterget1
   add si, 1
   cmp t[si], al
   jne getnum1; 处理第一个字符，寻找对应数字（无对应则为16）
   mov cl, 4
   shl si, cl; 完成移位操作
afterget1:
   mov di, -1
   mov al, input[1]
getnum2:
   cmp di, 16
   je afterget2
   add di, 1
   cmp t[di], al
   jne getnum2; 处理第二个字符，寻找对应数字（无对应则为16）
afterget2:
   or si, di; 进行逻辑或运算
   mov cl, 4
   shl si, cl; 结果需乘以16,等同于左移4位
   mov ax, 0013h
   int 10h; 进入图形编程模式
   mov ax, 0A000h
   mov es, ax; 定义初始段地址
   mov di, 0
   mov dx, 16; dx表示行数
next_row:
   mov ah, asc[si]
   add si, 1
   mov cx, 8; cx表示列数
check_next_dot:
   shl ah, 1
   jnc no_dot; 左移一位得到最高位，不为1则不是点
is_dot:
   mov byte ptr es:[di], 0Ch; 以亮红色画点
   jmp after_dot
no_dot:
   mov byte ptr es:[di], 1; 以蓝色画背景
after_dot:
   add di, 1; 偏移地址加一
   sub cx, 1; 一行中剩余填充位置减一
   jnz check_next_dot
   sub di, 8
   add di, 320; 偏移地址改到下一行的开始
   sub dx, 1
   jnz next_row
   mov ah, 0
   int 16h; bios键盘输入，类似int 21h的01h功能
   mov ax, 0003h
   int 10h; 切换到80*25文本模式
   mov ah, 4Ch
   int 21h; 退出程序
code ends
end main
```

## 计算器

```
.386
data segment use16
buf1 db 20 dup('0')
buf2 db 20 dup('0')
ans1 db 20 dup('$')
ans2 db 8 dup('0')
     db 1 dup('h')
     db 1 dup('$')
ans3 db 4 dup('0')
     db 1 dup(' ')
     db 4 dup('0')
     db 1 dup(' ')
     db 4 dup('0')
     db 1 dup(' ')
     db 4 dup('0')
     db 1 dup(' ')
     db 4 dup('0')
     db 1 dup(' ')
     db 4 dup('0')
     db 1 dup(' ')
     db 4 dup('0')
     db 1 dup(' ')
     db 4 dup('0')
     db 1 dup('B')
     db 1 dup('$') ;提前定义空格
crlf db 0Ah, 0Dh, '$'
abc dd 0
data ends
code segment use16
assume cs:code, ds:data
main:
  mov ax, data
  mov ds, ax ;初始化
  lea dx, buf1
  mov ah, 0Ah
  int 21h ;调用21h的0Ah功能输入第一个数字，存到buf1中
  lea dx, crlf
  mov ah, 09h
  int 21h ;调用21h的09h功能输出回车换行
  lea dx, buf2
  mov ah, 0Ah
  int 21h ;调用21h的0Ah功能输入第二个数字，存到buf2中
  lea dx, crlf
  mov ah, 09h
  int 21h ;调用21h的09h功能输出回车换行
  mov al, buf1+1
  add al, 2
  mov ah, 0
  mov si, ax ;数组的第一个字节存放数组实际存放字符的个数
  mov buf1[si], '$' ;定位到数组末尾并添加结束符号$
  lea dx, buf1+2 ;定位到数组的第二个字节（数据开始存储的位置）
  mov ah, 09h
  int 21h ;调用21h的09h功能输出第一个数字所对应的字符串
  mov dx, '*'
  mov ah, 02h
  int 21h ;调用21h的02h功能输出乘号（单个字符）
  mov al, buf2+1
  add al, 2
  mov ah, 0
  mov si, ax
  mov buf2[si], '$'
  lea dx, buf2+2
  mov ah, 09h
  int 21h ;调用21h的09h功能输出第二个数字所对应的字符串
  mov dx, '='
  mov ah, 02h
  int 21h ;调用21h的02h功能输出等号（单个字符）
  lea dx, crlf
  mov ah, 09h
  int 21h ;调用21h的09h功能输出回车换行
  mov eax, 0
  mov si, 2
again1:
  cmp buf1[si], '$' ;判断是否到达数组末尾
  je next1
  mov ebx, 10
  mul ebx ;eax乘以10，即十进制下左移一位
  mov edx, 0 ;每次均需清空eax以放进新的数字
  mov dl, buf1[si]
  sub dl, '0' ;把下一位化为数字存到dl中
  add eax, edx ;把数字存入eax中
  inc si ;位数加一
  jmp again1
next1:
  mov ecx, eax ;把所得第一个数字放入ecx中
  mov eax, 0
  mov si, 2
again2:
  cmp buf2[si], '$' ;以下代码与第一段同理
  je next2
  mov ebx, 10
  mul ebx
  mov edx, 0
  mov dl, buf2[si]
  sub dl, '0'
  add eax, edx
  inc si
  jmp again2
next2:
  mul ecx ;进行32位乘法，edx:eax为结果，只需取eax即可
  push eax ;将eax压入堆栈以保护
  mov di, 0
  mov cx, 0 ;记录堆栈次数
change1:
  mov edx, 0
  mov ebx, 10
  div ebx ;将eax除以10，取余数
  add dl, '0' ;将数字化为字符
  push dx ;将所得字符压入堆栈
  inc cx ;堆栈次数加一
  cmp eax, 0 ;若eax为0，则开始压出堆栈
  jne change1
pop1:
  pop dx ;将数字从高位至低位压出堆栈
  mov ans1[di], dx ;将数字存入ans1数组中
  inc di ;数组下标加一
  dec cx ;压出堆栈次数减一
  jnz pop1
print1:
  lea dx, ans1 ;让dx指向ans1的首地址
  mov ah, 09h
  int 21h ;调用21h的09h功能输出字符串
  lea dx, crlf
  mov ah, 09h
  int 21h ;调用21h的09h功能输出回车
  pop eax
  push eax ;将eax的原值压出堆栈并再次进行保护
  mov si, 7 ;从低位都高位填充ans2数组，从而使高位为0
change2:
  mov edx, 0
  mov ebx, 16
  div ebx ;将eax除以16，取余数
  cmp edx, 9 ;将所得值与9进行对比，若大于9则转变为字母，否则转变为数字字符
  ja after_change2
  add dl, '0' ;将数字转换为数字字符
  jmp goon2
after_change2:
  add dl, 55 ;将数字转换为字母
goon2:
  mov ans2[si], dl ;从低位开始放置
  dec si ;位置向前移一位
  cmp eax, 0
  jne change2 ;若eax为0则开始输出
print2:
  lea dx, ans2
  mov ah, 09h
  int 21h ;调用21h的09h功能输出字符串
  lea dx, crlf
  mov ah, 09h
  int 21h ;调用21h的09h功能输出回车
  pop eax
  push eax ;将eax的原值压出堆栈并再次进行保护
  mov si, 0
change3:
  cmp si, 39 ;包含空格共39个字符
  je print3
  cmp ans3[si], ' '
  jz goon3 ;若数组对应位置为空格，则跳过写入此位置
  shl eax, 1
  jnc goon3 ;左移一位，最高位存入CF标志位，通过jnc判断
  mov ans3[si], '1' ;若CF=1,则令数组对应位置为字符1
goon3:
  inc si ;数组下标加一
  jmp change3
print3:
  lea dx, ans3
  mov ah, 09h
  int 21h ;调用21h的09h功能输出字符串
  mov ah, 4Ch
  int 21h ;退出程序
code ends
end main
```

## 在文件中寻找二进制串

```
.386
data segment use16
word1 db "Input file name:", '$'
word2 db "Input a hex string, e.g. 41 42 43 0D 0A", '$'
word3 db "found at ", '$'
word4 db "Not found!", '$'
filename db 16 dup('$')
s db 100 dup('$')
q dw 0
p dw 0
n dd 0
hex_len dw 0
buf_len dw 0
read_len dw 400h dup(0)
processed_len dw 0
remained_len dw 0
return_num dd 0
longoffset dw 0
hex db 100 dup(0)
buf db 4000h dup(0)
crlf db 0Ah, 0Dh, '$'
fp dw 0
printout db 8 dup('0')
         db 1 dup('$')
data ends

code segment use16
assume cs:code, ds:data
find:
   mov longoffset, 0
   mov ax, si
   sub ax, 1
   mov bh, 3
   div bh
   mov hex_len, al ;求要寻找的字符串的字节数并将结果存入hex_len变量中
   mov di, offset s[2] ;原数组包含的字符数
   mov si, offset hex
change_char:
   mov bl, [di]
   call judge_char ;判断是数字还是字符并进行转换
   shl bl, 4 ;左移四位，相当于乘以16，在十六进制下左移一位
   mov [si], bl
   mov bl, [di+1]
   call judge_char ;第二个字符处理方法一致（一个字节由两个字符组成）
   add [si], bl ;相加得到一个字节
   add di, 3
   add si, 1
   mov bl, [di]
   cmp bl, '$' ;判断是否已到末尾
   jne change_char ;字符串转化为字节值
   mov bl, '$'
   mov [si], bl ;在更改完成的字符串后面加上结束标志
after_change_char:
   mov read_len, 4000h
   mov processed_len, 0
   mov remained_len, 0 ;数值初始化
search:
   mov ax, word ptr n[0]
   cmp ax, 0
   je after_search ;n为0则不进入循环
   mov si, processed_len
   add longoffset, si ;processed_len是上次已搜索过的字节数
   mov si, read_len
   cmp ax, si
   ja after_read_len
   mov read_len, ax ;给read_len赋值（read_len是本次要读的字节数）
after_read_len:
   mov ah, 3Fh
   mov bx, [fp]
   mov cx, read_len
   mov dx, offset buf
   add dx, remained_len
   int 21h ;读取read_len字节到buf+remained_len中
   mov si, read_len
   sub word ptr n, si ;n为剩余未读的字节数
   mov si, word ptr remained_len
   add si, word ptr read_len
   mov word ptr buf_len, si
   cmp si, word ptr hex_len
   jb after_search ;若buf中的字节数不足，则n一定为0，结束循环
   mov si, offset buf
   mov q, si ;设定搜索起点
search_point:
   add si, word ptr buf_len
   sub si, word ptr hex_len
   cmp q, si
   ja after_search_point ;若q超过搜寻范围则不进入循环
   sub si, q
   add si, 1 ;此时si为distance值
   push es ;将es压入堆栈以保护
   mov ax, data
   mov es, ax ;使es:di指向ds:di
   mov di, q
   mov cx, si
   mov al, hex[0]
   cld
   repne scasb ;字符串扫描，在[q, q+distance-1]范围内寻找hex[0]，相当于memchr
   pop es  ;将es压出堆栈
   jne after_search_point ;根据ZF标志位判断是否找到，若没有找到，则放弃当前buf中的内容
after_judge_p:
   inc cx
   sub si, cx
   add si, q
   mov p, si ;将找到的结果放到p变量中
   push es ;将es压入堆栈以保护
   mov ax, data
   mov es, ax ;使es:di指向ds:di
   mov di, offset hex
   mov cx, hex_len
   repe cmpsb
   pop es ;字符串比较，比较p和hex指向的hex_len字节是否相同，相当于memcmp
   jne if_not_same ;根据ZF标志位判断是否找到
if_same:
   mov si, longoffset
   add si, p
   sub si, offset buf
   mov return_num, si
   jmp find_done ;若找到，则返回偏移量return_num
if_not_same:
   mov si, p
   add si, 1
   mov q, si ;若不相同, 则下个搜索点=p+1
   jmp search_point
after_search_point:
   mov si, buf_len
   sub si, hex_len
   add si, 1
   mov processed_len, si ;processed_len = buf_len - hex_len + 1
   add si, offset buf
   mov q, si ;q = buf + processed_len
   mov si, hex_len
   sub si, 1
   mov remained_len, si ;remained_len = hex_len - 1
   push es ;将es压入堆栈以保护
   mov ax, data
   mov es, ax ;使es:di指向ds:di
   mov si, q
   mov di, offset buf
   mov cx, remained_len
   cld
   rep movsb
   pop es ;字符串复制，将q中的内容复制到buf中，相当于memcmp
   mov read_len, 4000h
   mov remained_len, si
   sub read_len, si ;计算下次要读的最大字节数
   jmp search
after_search:
   mov return_num, -1 ;没找到则返回-1
find_done:
   ret
judge_char:
   cmp bl, '0'
   jb is_letter
   cmp bl, '9'
   ja is_letter ;判断是数字还是字符
   sub bl, '0' ;为数字则减'0'
   jmp judge_char_done
is_letter:
   sub bl, 37h ;为字母减37h
judge_char_done:
   ret

main:
   mov ax, data
   mov ds, ax ;初始化
   lea dx, word1
   mov ah, 09h
   int 21h
   lea dx, crlf
   mov ah, 09h
   int 21h ;输出第一个提示语
   lea dx, filename
   mov ah, 0Ah
   int 21h ;将文件名存入filename变量
   mov al, filename+1
   add al, 2
   mov ah, 0
   mov si, ax
   mov filename[si], 0 ;末尾添加结束标记
   lea dx, crlf
   mov ah, 09h
   int 21h
   lea dx, word2
   mov ah, 09h
   int 21h
   lea dx, crlf
   mov ah, 09h
   int 21h ;输出第二个提示语
   lea dx, s
   mov ah, 0Ah
   int 21h ;将要查找的内容存入s变量
   mov al, s+1
   add al, 2
   mov ah, 0
   mov si, ax
   mov s[si], '$' ;末尾添加结束标记
   mov ah, 3Dh
   mov al, 0
   mov dx, offset filename[2]
   int 21h
   jc error ;打开文件
   mov [fp], ax
   mov ah, 42h
   mov al, 2
   mov bx, [fp]
   xor cx, cx
   xor dx, dx
   int 21h ;移动文件指针到EOF
   mov word ptr n[0], ax
   mov word ptr n[2], dx ;保存文件长度到n变量
   mov ah, 42h
   mov al, 0
   mov bx, [fp]
   xor cx, cx
   xor dx, dx
   int 21h ;移动文件指针到开端
   call find ;调用find函数
   cmp return_num, -1 ;判断是否找到
   je if_not_found
if_found:
   lea dx, crlf
   mov ah, 09h
   int 21h
   lea dx, word3
   mov ah, 09h ;输出找到的提示语
   int 21h
   mov eax, return_num
   xor esi, esi ;清空寄存器
   mov si, 7
change:
   mov edx, 0
   mov ebx, 16
   div ebx ;除以16以分离出最低位
   cmp edx, 9 ;判断是数字还是字母
   ja after_change
   add dl, '0' ;数字则加上'0'变为字符
   jmp goon
after_change:
   add dl, 55 ;数字则加上55变为字符
goon:
   mov printout[si], dl ;将改好的字符存入printout中
   dec si
   cmp eax, 0
   jne change ;字符不为0时重复上述过程
print_ansewr:
   lea dx, printout
   mov ah, 09h
   int 21h ;输出找到的结果
   jmp error
if_not_found:
   lea dx, crlf
   mov ah, 09h
   int 21h
   lea dx, word4
   mov ah, 09h
   int 21h ;输出找不到的提示语
error:
   mov ah, 4Ch
   int 21h ;退出程序
code ends
end main
```

# C++

## 体育俱乐部I（构造函数）

### 题目概述

一个俱乐部需要保存它的简要信息，包括四项：名称（字符串），成立年份（整数），教练姓名（字符串）和教练胜率（0－100之间的整数）。用键盘输入这些信息后，把它们分两行输出：第一行输出名称和成立年份，第二行输出教练姓名和胜率。

### 裁判测试程序样例

```
#include <iostream>
#include <string>
using namespace std;
class Coach{
    string name;
    int winRate;
public:
    Coach(string n, int wr){
        name=n; winRate=wr;
    }
    void show();
};
class Club{
    string name;
    Coach c;
    int year;
public:
    Club(string n1, int y, string n2, int wr);
    void show();
};
int main(){
    string n1, n2;
    int year, winRate;
    cin>>n1>>year>>n2>>winRate;
    Club c(n1,year, n2, winRate);
    c.show();
    return 0;
}

/* 请在这里填写答案 */
```

### 输入样例

```
Guanzhou 2006 Tom 92
```

### 输出样例

```
Guanzhou 2006
Tom 92%
```

### 代码

```
void Club::show() {
    cout<<name<<" "<<year<<endl;
    c.show();
}

Club::Club(string n1, int y, string n2, int wr):name(n1),year(y),c(n2,wr) {}

void Coach::show() {
    cout<<name<<" "<<winRate<<"%"<<endl;
}
```

## 学生排名表（析构函数）

### 题目概述

现在输入一批学生（人数大于0且不超过100）的名次和他们的姓名。要求按名次输出每个人的排名。

输入格式：每行为一个学生的信息，共两项，第一项为排名（为正整数，且任意两名学生的排名均不同），第二项为学生姓名。当输入－1时，表示输入结束。

输出格式：按名次输出学生姓名，每行一个。

### 裁判测试程序样例

```
#include <iostream>
#include <string>
using namespace std;
class Student{
    int rank;
    string name;
    public:
        int getRank(){return rank;  }
        Student(string name, int rank):name(name), rank(rank){  }
        ~Student(){ cout<<name<<endl;}
};
int main(){
    int rank, count=0;
    const int SIZE=100;
    Student *pS[SIZE];
    string name;
    cin>>rank;
    while(count<SIZE && rank>0){
        cin>>name;
        pS[count++]= new Student(name, rank);
        cin>>rank;
    }

/* 请在这里填写答案 */

    return 0;
}
```

### 输入样例

```
1 Jack
5 Peter
2 Alice
6 Kate
52 Mike
-1
```

### 输出样例

```
Jack
Alice
Peter
Kate
Mike
```

### 代码

```
    Student *tmp;
    for(int i=0;i<count;i++){
        for(int j=i+1;j<count;j++)
        if(pS[i]->getRank()>pS[j]->getRank()){
            tmp=pS[i];
            pS[i]=pS[j];
            pS[j]=tmp;
        };
    };

    for(int i=0;i<count;i++){
        delete pS[i];
    };
```

## 学生成绩的快速录入（构造函数）

### 题目概述

现在需要录入一批学生的成绩（学号，成绩）。其中学号是正整数，并且录入时，后录入学生的学号会比前面的学号大；成绩分两等，通过(Pass,录入时用1代表),不通过(Fail,录入时用0代表）。

由于很多学号都是相邻的，并且学号相邻的学生成绩常常相同。所以在录入时，适当地加了速。如果当前学生的学号比前面的学号大1，且成绩与前面的成绩相同，则只输入0即可。

### 裁判测试程序样例

```
#include<iostream>
using namespace std;

/* 请在这里填写答案 */

int main(){
    const int size=100;
    int i, N, no, score;
    Student *st[size];
    cin>>N;
    for(i=0; i<N; i++){
        cin>>no;
        if(no>0){
            cin>>score;
            st[i]=new Student(no, score);
        }
        else
            st[i]=new Student(*st[i-1]);
    }
    cout<<Student::count<<" Students"<<endl;
    for(i=0;i<N;i++) st[i]->display();
    for(i=0;i<N;i++) delete st[i];
    return 0;
}
```

### 输入样例

```
5
3 0
0
7 1
0
12 1
```

### 输出样例

```
5 Students
3 Fail
4 Fail
7 Pass
8 Pass
12 Pass
```

### 代码

```
class Student{
private:
    int no;
    int score;
public:
    static int count;
    Student(int no1,int score1):no(no1),score(score1){count++;};
    Student(const Student& p){
        no=p.no+1;
        score=p.score;
        count++;
    };
    void display(){
        cout<<no;
        if(score==1) cout<<" Pass"<<endl;
        else if(score==0) cout<<" Fail"<<endl;
    };
    ~Student(){count--;}
};
int Student::count=0;
```

## Tree类的构造函数和成员函数

### 题目概述

定义一个Tree（树）类，有成员ages（树龄），不带参数的构造函数对ages初始化为1，成员函数grow(int years)对ages加上years，age()显示tree对象的ages的值。

### 裁判测试程序样例

```
#include <iostream>
using namespace std;

class Tree {
public:
    Tree();//构造函数
    void grow(int years);//对数龄ages加上years
    void age();//显示数龄ages的值
private:
    int ages;//树龄
};

int main()
{
    Tree tree;
    int years;
    cin >> years;
    tree.grow(years);
    tree.age();

    return 0;
}

/* 你的代码将被嵌在这里 */
```

### 输入样例

```
30
```

### 输出样例

```
31
```

### 代码

```
Tree::Tree(){ages=1;}

void Tree::grow(int years) {ages=years+ages;}

void Tree::age() {cout<<ages<<endl;}
```

## 整数的素因子分解

### 题目概述

将正整数n分解为其素因子的乘积，其中n>=2并且在int范围内。Solution类的数据成员n代表需要分解的正整数，构造函数完成对数据成员n的初始化，声明了成员函数solve()实现对n的分解。请根据样例输出实现成员函数。注意输出时每行最后一个数字后面没有空格。

### 裁判测试程序样例

```
#include <iostream>
#include <cmath>
using namespace std;

class Solution {
public:
    Solution(int num) {
        n = num;
    }
    void solve();//将n分解为素因子的乘积
private:
    int n;
};

int main()
{
    int n;
    while (cin >> n) {
        Solution obj(n);
        obj.solve();
    }
    return 0;
}
//你的代码将被嵌在这里
```

### 输入样例

```
2
3
13
24
36
1024
65535
```

### 输出样例

```
2=2
3=3
13=13
24=2*2*2*3
36=2*2*3*3
1024=2*2*2*2*2*2*2*2*2*2
65535=3*5*17*257
```

### 代码

```
bool prime( int a )
{
    if(a<=1) return false;
    for(int i=2; i<=sqrt(a); i++)
        if(a%i==0) return false;
    return true;
}


void Solution::solve()
{
    int i=0,k=1;
    cout<<n<<"=";
    if(prime(n)) cout<<n;
    else
    {
        for(i=2; ; )
            if(prime(i) && n%i==0 )
            {
                n/=i;
                if(k){
                    cout<<i;
                    k--;
                }
                else cout<<"*"<<i;

                if(n==1) break;
            }
            else i++;
    }
    cout<<endl;
}
```

## 创建CPU

### 题目概述

定义一个CPU类，包含等级（Rank）、频率（frequency）、电压(voltage)等属性。其中，rank为枚举类型CPU__Rank,定义为enum CPU_Rank{P1=1,P2,P3,P4,P5,P6,P7},frequency为单位是MHz的整型数，voltage为浮点型的电压值。

### 裁判测试程序样例

```

/* 请在这里填写答案 */


int main()
{
    CPU a(P6,3,300); 
    
    cout<<"cpu a's parameter"<<endl;
    a.showinfo(); //显示性能参数

    CPU b; 
    cout<<"cpu b's parameter"<<endl;
    b.showinfo(); //显示性能参数

    CPU c(a); 
    cout<<"cpu c's parameter"<<endl;
    c.showinfo(); //显示性能参数
}
```

### 输出样例

```
create a CPU!
cpu a's parameter
rank:6
frequency:3
voltage:300
create a CPU!
cpu b's parameter
rank:1
frequency:2
voltage:100
copy create a CPU!
cpu c's parameter
rank:6
frequency:3
voltage:300
destruct a CPU!
destruct a CPU!
destruct a CPU!
```

### 代码

```
#include<iostream>
using namespace std;

enum CPU_Rank{P1=1,P2,P3,P4,P5,P6,P7};
class CPU{
private:
    CPU_Rank rank;
    int frequency;
    float voltage;
public:
    CPU(CPU_Rank ran=P1,int fre=2,float vol=100):rank(ran),frequency(fre),voltage(vol){
        cout<<"create a CPU!"<<endl;
    };
    CPU(const CPU& p){
        rank=p.rank;
        frequency=p.frequency;
        voltage=p.voltage;
        cout<<"copy create a CPU!"<<endl;
    }
    void showinfo(){
        cout<<"rank:"<<rank<<endl;
        cout<<"frequency:"<<frequency<<endl;
        cout<<"voltage:"<<voltage<<endl;
    };
    ~CPU(){cout<<"destruct a CPU!"<<endl;}
};
```

## 创建计算机

### 题目概述

定义一个简单的Computer类，有数据成员芯片(cpu)、内存(ram)、光驱(cdrom)等等，有两个公有成员函数run、stop。cpu为CPU类的一个对象，ram为RAM类的一个对象，cdrom为CDROM类的一个对象，定义并实现这个类，为以上的类编写构造和析构函数，注意使用类组合的思想解决该问题，使得给出的主函数代码可以正确运行并得到给出的输出结果。

### 裁判测试程序样例

```
/* 请在这里填写答案 */

在这里给出函数被调用进行测试的例子。例如：

int main()
{
    COMPUTER cpt1(6,4.0,200,60,32);  //测试带参数构造
    cout<<"cpt1's parameter:"<<endl;
    cpt1.showinfo();
    cout<<"------------------------------"<<endl;
    COMPUTER cpt2; //测试不带参数构造
    cout<<"cpt2's parameter:"<<endl;
    cpt2.showinfo();
    cout<<"------------------------------"<<endl;
    COMPUTER cpt3(cpt1); //测试复制构造
    cout<<"cpt3's parameter:"<<endl;
    cpt3.showinfo();
    cout<<"------------------------------"<<endl;   
}
```

### 输出样例

```
create a CPU!
create a RAM!
create a CDROM!
create a COMPUTER with para!
cpt1's parameter:
cpu parameter:
rank:6
frequency:4
voltage:200
ram parameter:
volumn:60 GB
cdrom parameter:
speed:32
------------------------------
create a CPU!
create a RAM!
create a CDROM!
no para to create a COMPUTER!
cpt2's parameter:
cpu parameter:
rank:1
frequency:2
voltage:100
ram parameter:
volumn:1 GB
cdrom parameter:
speed:16
------------------------------
create a CPU by copy!
create a RAM by copy!
create a CDROM by copy!
create a COMPUTER by copy!
cpt3's parameter:
cpu parameter:
rank:6
frequency:4
voltage:200
ram parameter:
volumn:60 GB
cdrom parameter:
speed:32
------------------------------
destruct a COMPUTER!
destruct a CDROM!
desturct a RAM!
desturct a CPU!
destruct a COMPUTER!
destruct a CDROM!
desturct a RAM!
desturct a CPU!
destruct a COMPUTER!
destruct a CDROM!
desturct a RAM!
desturct a CPU!
```

### 代码（答案错误）

```
#include<iostream>
using namespace std;

class CPU{
private:
    int rank;
    int frequency;
    int voltage;

public:
    CPU(int rank1=1,int frequency1=2,int voltage1=100):rank(rank1),frequency(frequency1),voltage(voltage1){
        cout<<"create a CPU!"<<endl;
    }
    CPU(const CPU& p){
        rank=p.rank;
        frequency=p.frequency;
        voltage=p.voltage;
        cout<<"create a CPU by copy!"<<endl;
    }
    ~CPU(){cout<<"destruct a CPU!"<<endl;}

    void showinfo(){
        cout<<"cpu parameter:"<<endl;
        cout<<"rank:"<<rank<<endl;
        cout<<"frequency:"<<frequency<<endl;
        cout<<"voltage:"<<voltage<<endl;
    }

    void run(){
        cout<<"CPU is running!"<<endl;
    }

    void stop(){
        cout<<"CPU stopped!"<<endl;
    }

};

class RAM{
private:
    int volumn;
public:
    RAM(int volumn1=1):volumn(volumn1){
        cout<<"create a RAM!"<<endl;
    }
    RAM(const RAM& p){
        volumn=p.volumn;
        cout<<"create a RAM by copy!"<<endl;
    }
    ~RAM(){cout<<"destruct a RAM!"<<endl;}

    void showinfo(){
        cout<<"ram parameter:"<<endl;
        cout<<"volumn:"<<volumn<<" GB"<<endl;
    }

    void run(){
        cout<<"RAM is running!"<<endl;
    }

    void stop(){
        cout<<"RAM stopped!"<<endl;
    }
};

class CDROM{
private:
    int speed;
public:
    CDROM(int speed1=16):speed(speed1){
        cout<<"create a CDROM!"<<endl;
    }
    CDROM(const CDROM& p){
        speed=p.speed;
        cout<<"create a CDROM by copy!"<<endl;
    }
    ~CDROM(){cout<<"destruct a CDROM!"<<endl;}

    void showinfo(){
        cout<<"cdrom parameter:"<<endl;
        cout<<"speed:"<<speed<<endl;
    }

    void run(){
        cout<<"CDROM is running!"<<endl;
    }

    void stop(){
        cout<<"CDROM stopped!"<<endl;
    }
};

class COMPUTER{
private:
    CPU cpuer;
    RAM ramer;
    CDROM cdromer;
public:
    COMPUTER(int rank,int frequency,int voltage,int volumn,int speed):cpuer(rank,frequency,voltage),ramer(volumn),cdromer(speed){
        cout<<"create a COMPUTER with para!"<<endl;
    }
    COMPUTER(){
        cout<<"no para to create a COMPUTER!"<<endl;
    }
    COMPUTER(const COMPUTER& p):cpuer(p.cpuer),ramer(p.ramer),cdromer(p.cdromer){
        cout<<"create a COMPUTER by copy!"<<endl;
    }
    ~COMPUTER(){cout<<"destruct a COMPUTER!"<<endl;}

    void showinfo(){
        cpuer.showinfo();
        ramer.showinfo();
        cdromer.showinfo();
    }

    void run(){
        cout<<"COMPUTER is running!"<<endl;
    }

    void stop(){
        cout<<"COMPUTER stopped!"<<endl;
    }
};
```

## 二维向量相加（C++ 运算符重载）

### 题目概述

裁判测试程序样例中展示的是一段二维向量类TDVector的定义以及二维向量求和的代码，其中缺失了部分代码，请补充完整，以保证测试程序正常运行。

### 裁判测试程序样例

```
#include <iostream>
#include <iomanip>
using namespace std;
class TDVector{
private:
    double x;
    double y;
public:
    TDVector(){
        x = y = 0;
    }
/**    你提交的代码将被嵌在这里（替换本行内容）  **/
};
int main(){
    TDVector a;
    double x, y;
    cin >> x >> y;
    TDVector b(x, y);
    cin >> x >> y;
    TDVector c;
    c.setX(x);
    c.setY(y);
    TDVector d;
    d = a + b + c;
    cout << fixed << setprecision(2) << d.getX() << ' ' << d.getY();
    return 0;
}
```

### 输入样例

```
1.1 2.2
3.3 4.4
```

### 输出样例

```
4.40 6.60
```

### 代码

```
    TDVector(double x1,double y1):x(x1),y(y1){}

    void setX(double x2){x=x2;}
    void setY(double y2){y=y2;}
    double getX(){return x;}
    double getY(){return y;}

    TDVector operator+(const TDVector& p){
        TDVector td;
        td.x=x+p.x;
        td.y=y+p.y;
        return td;
    }
```

## 实现数组类（C++ 拷贝构造函数、拷贝函数）

### 题目概述

裁判测试程序样例中展示的是一段实现“数组类”的代码，其中缺失了部分代码，请补充完整，以保证测试程序正常运行。

### 裁判测试程序样例

```
#include <iostream>
using namespace std;
class ArrayIndexOutOfBoundsException{  // 异常类
public:
    int index;
    ArrayIndexOutOfBoundsException(int k){
        index = k;
    }
};
class Array{
private:
    int *data;
    int size;
    static const int dSize = 10;   // 数组默认大小
public:
    Array( ){  // 无参构造
        size = dSize;
        data = new int[size]( );
    }
        
/** 你提交的代码将被嵌在这里（替换本行内容） **/        
        
    int& operator [] (int k){     // 运算符 [ ] 重载，以方便数组的使用
        if(k<0 || k>=size) throw ArrayIndexOutOfBoundsException(k);
        return data[k];
    }
    friend ostream& operator << (ostream& o, const Array& a);   // 运算符 << 重载，以方便输出
};
ostream& operator << (ostream& o, const Array& a){
    o << '[' ;
    for(int i=0; i<a.size-1; i++)
        o << a.data[i] << ',' ;
    o << a.data[a.size-1] << ']';
    return o;
}
// 注意：实际测试程序中，在此处之前的代码与样例中相同
// 注意：实际测试程序中，在此处之后的代码（即main函数）可能与样例中不同
int main(){
    int n, k;
    cin >> n >> k;
    Array a(n);  // 构造数组，大小为 n
    for(int i=0; i<n; i++) a[i] = i;
    Array b = a;  // 拷贝构造数组
    b[n/2] = k;
    cout << a << endl;
    cout << b << endl;
    Array c;  // 构造数组，默认大小
    c = a; // 拷贝数组
    c[n/2] = k;
    cout << a << endl;
    cout << c << endl;
    a = a;
    a[n/2] = 2223;
    cout << a << endl;
    return 0;
}#include <iostream>
using namespace std;
class ArrayIndexOutOfBoundsException{  // 异常类
public:
    int index;
    ArrayIndexOutOfBoundsException(int k){
        index = k;
    }
};
class Array{
private:
    int *data;
    int size;
    static const int dSize = 10;   // 数组默认大小
public:
    Array( ){  // 无参构造
        size = dSize;
        data = new int[size]( );
    }
        
/** 你提交的代码将被嵌在这里（替换本行内容） **/        
        
    int& operator [] (int k){     // 运算符 [ ] 重载，以方便数组的使用
        if(k<0 || k>=size) throw ArrayIndexOutOfBoundsException(k);
        return data[k];
    }
    friend ostream& operator << (ostream& o, const Array& a);   // 运算符 << 重载，以方便输出
};
ostream& operator << (ostream& o, const Array& a){
    o << '[' ;
    for(int i=0; i<a.size-1; i++)
        o << a.data[i] << ',' ;
    o << a.data[a.size-1] << ']';
    return o;
}
// 注意：实际测试程序中，在此处之前的代码与样例中相同
// 注意：实际测试程序中，在此处之后的代码（即main函数）可能与样例中不同
int main(){
    int n, k;
    cin >> n >> k;
    Array a(n);  // 构造数组，大小为 n
    for(int i=0; i<n; i++) a[i] = i;
    Array b = a;  // 拷贝构造数组
    b[n/2] = k;
    cout << a << endl;
    cout << b << endl;
    Array c;  // 构造数组，默认大小
    c = a; // 拷贝数组
    c[n/2] = k;
    cout << a << endl;
    cout << c << endl;
    a = a;
    a[n/2] = 2223;
    cout << a << endl;
    return 0;
}
```

### 输入样例

```
15 666
```

### 输出样例

```
[0,1,2,3,4,5,6,7,8,9,10,11,12,13,14]
[0,1,2,3,4,5,6,666,8,9,10,11,12,13,14]
[0,1,2,3,4,5,6,7,8,9,10,11,12,13,14]
[0,1,2,3,4,5,6,666,8,9,10,11,12,13,14]
[0,1,2,3,4,5,6,2223,8,9,10,11,12,13,14]
```

### 代码

```
    Array(int n1){  // 带参构造
        size = n1;
        data = new int[size]( );
    }

    Array(const Array& p){  // 拷贝构造函数
        size = p.size;
        data = new int[size];
        for(int i=0;i<size;i++)
            data[i]=p.data[i];
    }

    ~Array(){
        delete[] data;
    }

    Array & operator=(const Array & p) {
        if (p.size != size) {
            delete[] data;
            size = p.size;
            data = new int[size];
        }
        for (int i = 0; i < size; i++) {
            data[i] = p.data[i];
        }
        return *this;
    }

```

## 写出派生类构造方法（C++）

### 题目概述

裁判测试程序样例中展示的是一段定义基类People、派生类Student以及测试两个类的相关C++代码，其中缺失了部分代码，请补充完整，以保证测试程序正常运行。

### 裁判测试程序样例

```
#include <iostream>
using namespace std;
class People{
private:
    string id;
    string name;
public:
    People(string id, string name){
        this->id = id;
        this->name = name;
    }
    string getId(){
        return this->id;
    }
    string getName(){
        return name;
    }
};
class Student : public People{
private:
    string sid;
    int score;
public:
    Student(string id, string name, string sid, int score)
        
        /** 你提交的代码将被嵌在这里（替换此行） **/
        
    }
    friend ostream& operator <<(ostream &o, Student &s);
};
ostream& operator <<(ostream &o, Student &s){
    o << "(Name:" << s.getName() << "; id:"
      << s.getId() << "; sid:" << s.sid
      << "; score:" << s.score << ")";
    return o;
}
int main(){
    Student zs("370202X", "Zhang San", "1052102", 96);
    cout << zs  << endl;
    return 0;
}
```

### 输出样例

```
(Name:Zhang San; id:370202X; sid:1052102; score:96)
```

### 代码

```
    :People(id, name){
        this->sid=sid;
        this->score=score;
```

## 定义Date类

### 题目概述

本题要求实现一个日期类定义，根据所定义的类可以完成相关的类测试。

根据Date被使用的情况，进行Date类定义，要求通过构造函数进行日期初始化，并通过display（）函数进行日期格式显示，显示格式为"月/日/年"

### 裁判测试程序样例

```
int main()
{
 Date d1(3,25,2019);
 Date d2(3,30);
 Date d3(10);
 Date d4;
 d1.display();
 d2.display();
 d3.display();
 d4.display();
 return 0;
 }

/* 请在这里填写答案 */
```

### 输出样例

```
3/25/2019
3/30/2019
10/1/2019
1/1/2019
```

### 代码

```
#include<iostream>
using namespace std;

class Date{
private:
    int year;
    int month;
    int day;
public:
    Date(int month1=1,int day1=1,int year1=2019):year(year1),month(month1),day(day1){};
    void display(){cout<<month<<"/"<<day<<"/"<<year<<endl;}
};
```

## 学生平均分计算

### 题目概述

定义一学生类，已有若干个学生数据，包括学号、姓名、成绩，要求输出这些学生数据并计算平均分。

### 裁判测试程序样例

```
利用学生类进行对象定义并输出结果的例子如下：
/* 请在这里填写答案 */

int Stud::sum=0;
int Stud::num=0;
int main()
{
    Stud  s1(1,"Li",89)，s2(2,"Chert",78)，s3(3,"zheng",94);
    s1.disp();
    s2.disp();
    s3.disp()；
    cout<<"avg="<<Stud::avg()<<endl;
    return 0；
}
```

### 输出样例

```
1，Li，89
2，Chert，78
3，zheng，94
avg=87
```

### 代码

```
#include<iostream>
using namespace std;

class Stud{
private:
    int no;
    string name;
    int score;
public:
    static int sum;
    static int num;
    Stud(int no1,string name1,int score1):no(no1),name(name1),score(score1){
        sum+=score;
        num++;
    };
    void disp(){
        cout<<no<<","<<name<<","<<score<<endl;
    }
    static int avg(){return sum/num;}
};
```

## 学生类的构造与析构

### 题目概述

定义一个学生类Student，使得main()函数能够得到指定的输出结果。

### 裁判测试程序样例

```
/* 请在这里填写答案 */
int main()
  {Student stud1(10010,"Wang_li",'f');
   stud1.display();
   Student stud2(10011,"Zhang_fun",'m');
   stud2.display();
   return 0;
}
```

### 输出样例

```
Constructor called.
num:10010
name:Wang_li
sex:f

Constructor called.
num:10011
name:Zhang_fun
sex:m

Destructor called.
Destructor called.
```

### 代码

```
#include<iostream>
using namespace std;

class Student{
private:
    int num;
    string name;
    char sex;
public:
    Student(int num1,string name1,char sex1):num(num1),name(name1),sex(sex1){
        cout<<"Constructor called."<<endl;
    }
    void display(){
        cout<<"num:"<<num<<endl;
        cout<<"name:"<<name<<endl;
        cout<<"sex:"<<sex<<endl<<endl;
    }
    ~Student(){cout<<"Destructor called."<<endl;}
};
```

## 定义点类

### 题目概述

根据main（）中的调用形式，定义一个点类（完成数据成员与成员函数的定义），将点的坐标显示在屏幕上。

### 裁判测试程序样例

```
在这里给出函数被调用进行测试的例子。例如：
int main()
{
    Point p1,p2;
    p1.setXY(1,2);
    p2.setXY(3,4);
    cout<<"x of Point p1："<<p1.getX()<<endl;
    cout<<"y of Point p2："<<p1.getY()<<endl;
    cout<<"Point P2:("<<p2.getX()<<","<<p2.getY()<<")"<<endl;
    return 0;
}
/* 请在这里填写答案 */
```

### 输出样例

```
x of Point p1：1
y of Point p2：2
Point P2:(3,4)
```

### 代码

```
#include<iostream>
using namespace std;

class Point{
private:
    int x;
    int y;
public:
    void setXY(int x1,int y1){x=x1;y=y1;}
    int getX(){return x;}
    int getY(){return y;}
};
```

## 自定义的学生类

### 题目概述

本题要求定义一个简单的学生类，数据成员仅需要定义学号和姓名，函数成员的原型见给出的代码，请给出函数成员的类外完整实现。

### 函数接口定义

```
class Student
{
private:
    int m_id;
    char m_name[10];
public:
    Student(int id=0,char *name="");
    ~Student();
    void print();

};
```

其中m_id和m_name分别表示学生的学号和姓名，类型已经定义好。类内声明了3个成员函数，分别表示构造函数、析构函数和用于输出学生信息的函数。 构造函数要完成两个数据成员的初始赋值，并做一行输出，形如：“Hi!学号 姓名”。 析构函数要求输出一行，形如：“Bye!学号 姓名”。 print函数要求在一行中输出学生信息，形如：“学号 姓名”。 注：学号和姓名之间用一个空格分开。

### 裁判测试程序样例

```
#include <iostream>
#include <cstring>
using namespace std;

int main()
{
    Student stu_array[3]={Student(1,"Zhang"),Student(2,"Wang")};
    return 0;
}

/* 请在这里填写答案 */
```

### 代码

```
Student::Student(int id,char *name){
    m_id=id;
    strcpy(m_name,name);
    cout<<"Hi!"<<m_id<<" "<<m_name<<endl;
}

Student::~Student(){cout<<"Bye!"<<m_id<<" "<<m_name<<endl;}

void Student::print() {cout<<m_id<<" "<<m_name<<endl;}
```

## 类的定义和使用

### 题目概述

定义一个日期类Date，内有数据成员年、月、日，另有成员函数：构造函数用于初始化数据成员，输出，闰年的判断。编写主函数：创建日期对象，计算并输出该日是该年的第几天。

### 裁判测试程序样例

```
输入:
每组测试数据仅包含一个测试用例，每个测试用例占一行包括三个数，分别表示年、月、日。

输出:
该日是该年的第几天。
```

### 输入样例

```
2006 3 5
```

### 输出样例

```
64   (2006年3月5日是该年的第64天)
```

### 代码

```
#include <iostream>
using namespace std;
class Date{
private:
    int year;
    int month;
    int day;
    int result=0;
    int a[12]={31,28,31,30,31,30,31,31,30,31,30,31};
public:
    Date(int x,int y,int z){
        year=x;
        month=y;
        day=z;
    };
    void getresult(){
        for(int i=1;i<month;i++){
            result=result+a[i-1];
        }
        result=result+day;
        if (month>2&&(year%100==0&&year%400==0)||(year%100!=0&&year%4==0))
            result=result+1;
        cout<<result;
    }
};

int main()
{
    int m;
    int n;
    int p;
    cin>>m>>n>>p;
    Date obj(m,n,p);
    obj.getresult();
    return 0;
}
```

## 类的定义和使用

### 题目概述

请定义一个Point类，有两个数据成员：x和y, 分别代表x坐标和y坐标，并有若干构造函数和一个移动的成员函数，可输出移动后新的坐标值。

### 裁判测试程序样例

```
输入:
第一行的两个数 分别表示 点的x坐标和y坐标。 第二行的两个数 分别表示 x和y方向移动的距离。

输出:
移动后的点的x坐标和y坐标。
```

### 输入样例

```
1  5
2  5
```

### 输出样例

```
3 10
```

### 代码

```
#include <iostream>
using namespace std;
class Point{
private:
    int x;
    int y;
    int movx;
    int movy;
    int newx;
    int newy;
public:
    Point(int m,int n,int p,int q){
        x=m;
        y=n;
        movx=p;
        movy=q;
    };
    void getmove(){
        newx=x+movx;
        newy=y+movy;
        cout << newx << " " << newy;
    }
};

int  main( ){
    int a;
    int b;
    int c;
    int d;
    cin>>a>>b>>c>>d;
    Point obj(a,b,c,d);
    obj.getmove();
    return 0;
}
```

## 个位数统计 (15)

### 题目概述

给定一个k位整数N = dk-110k-1 + ... + d1101 + d0 (0<=di<=9, i=0,...,k-1, dk-1>0)，请编写程序统计每种不同的个位数字出现的次数。例如：给定N = 100311，则有2个0，3个1，和1个3。

### 裁判测试程序样例

```
输入格式：
每个输入包含1个测试用例，即一个不超过1000位的正整数N。

输出格式：
对N中每一种不同的个位数字，以D:M的格式在一行中输出该位数字D及其在N中出现的次数M。要求按D的升序输出。
```

### 输入样例

```
100311
```

### 输出样例

```
0:2
1:3
3:1
```

### 代码

```
#include <iostream>
#include <string>
using namespace std;
class Countnum{
private:
    string str;
    int i=0;
    int a[10]={0};
public:
    void givecount(){
        cin>>str;
        while(i<=str.length()){
            a[str[i]-'0']++;
            i++;
        }
        for(int i=0;i<10;i++)
            if (a[i]>0)
                cout<<i<<":"<<a[i]<<endl;
    };
};

int main(){
    Countnum obj;
    obj.givecount();
    return 0;
}
```

## 乒乓球俱乐部的年度考核

### 题目概述

一个乒乓球俱乐部有A、B两组。其中A组为主力队员，承担对外比赛任务；B组为后备队，日常以训练为主。

下面给出基类的框架：

```
class Group
{
protected:
string name;//姓名
public:
virtual void display()=0;//显示考核成绩
}
```

以Group为基类，构建GroupA和GroupB三个类。

生成上述类并编写主函数，要求主函数中有一个基类Group指针数组，数组元素不超过20个。

Group pg[20];

主函数根据输入的信息，相应建立GroupA, GroupB类对象。

### 裁判测试程序样例

```
输入格式：每个测试用例占一行，第一项为类型，A为GroupA，B为GroupB, 第二项是运动员姓名，对于GroupA来说，第3项是总胜利场数，第4项是总失败场数。对于GroupB来说，接下来为各场测试赛的成绩（不超过5场）。输入0表示输入结束。
要求输出运动员姓名，组别和评分。对于GroupA来说，总分为: 2胜利场数-失败场数。对于GroupB来说，总分为: 胜利的局数-失败的局数。
```

### 输入样例

```
A Tom 20 3
B Jack 4:2 3:4 4:0 1:4
A Jim 15 9
A John 10 11
B Tony 4:1 0:4 4:3 4:3 4:2
B Li 3:2
0
```

### 输出样例

```
Tom A 37
Jack B 2
Jim A 21
John A 9
Tony B 3
Li B 1
```

### 代码（答案错误）

```
#include <iostream>
using namespace std;
class Group
{
protected:
    string name;//姓名
public:
    virtual void display()=0;//显示考核成绩
    Group(string name1):name(name1){}
};


class GroupA:public Group{
private:
    int score;
public:
    GroupA(string name1,int scoreA):Group(name1),score(scoreA){}
    void display(){cout<<name<<" A "<<score<<endl;}
};

class GroupB:public Group{
private:
    int score;
public:
    GroupB(string name1,int scoreB):Group(name1),score(scoreB){}
    void display(){cout<<name<<" B "<<score<<endl;}
};



int main(){
    Group *pg[20];

    char type;
    string name;

    int win;
    int lose;
    int total;

    int num1;
    int num2;
    int i;
    int count=0;
    char c;

    cin>>type;
    while(type!='0'){
        cin>>name;
        if(type=='A'){
            cin>>win>>lose;
            total=2*win-lose;
            pg[count++]=new GroupA(name,total);

        }
        else if(type=='B'){
            win=0;
            lose=0;
            c=cin.get();
            while(c!='\n'){
                cin>>num1;
                cin.get();
                cin>>num2;
                if(num1>num2) win++;
                else if (num1<num2) lose++;
                c=cin.get();
            }
            total=win-lose;
            pg[count++]=new GroupB(name,total);

        }
        cin>>type;
    }
    for(i=0;i<count;i++){
        pg[i]->display();
    }

    return 0;
}
```

## 字符统计问题

### 题目概述

实验室有个胖子叫小明，他第一天进来，老师就叫他练打字，开始练的是打英文字母，小明打了一小时就累啦，老师看他累就叫他不用再练啦，但前提是必须知道他打的英文字母串里连续相同的字符最多到底有几个？亲爱的同学们，如果你们同情胖子小明，那么你就帮他写个程序，计算一下在一个字符串里面连续相同的字符最多到底有多少个？这里我们假定字符串都是由英文小写字母、大写字母和数字组成的；字符串的长度不超过100000；我们规定“相同”的定义为：

```
1：不区分大小写；
2：“0”=“a”=“A”；“1”=“b”=“B”；……；“9”=“j”=“J”；
```

### 裁判测试程序样例

```
输入是一行字符串，输出有两行，第一行的格式是：“From=XX,To=XX”，第二行的格式是：“MaxLen=XX”。本问题有多组测试数据。如果有多组符合要求的解，那么输出起始位置最小的解。
```

### 输入样例

```
iaaaaa000AAAAwh
```

### 输出样例

```
From=2,To=13
MaxLen=12
```

### 代码（答案错误）

```
#include <iostream>
#include <string>
using namespace std;

class Charc{
public:
    int start=-1;
    int end=-1;
    char ch='\0';
    int lowup='a'-'A',lownum='a'-'0';
    char comp[3][2]={'a','z','A','Z','0','9'};
    void handle(){
        int flag;
        for(flag=0;flag<3;flag++){
            if(ch>=comp[flag][0]&&ch<=comp[flag][1])
                break;
        };
        switch(flag){
            case 0:
                break;
            case 1:
                ch += lowup;
                break;
            case 2:
                ch += lownum;
                break;
        }
    };
};

int main(){
    int i=1;
    Charc result1,result2,tmp,noch;
    result1.start=0;
    result1.end=0;

    string st;
    cin>>st;

    result1.ch=st[0];
    result1.handle();

    while(st[i]!='\0'){
        tmp.ch=st[i];
        tmp.start=i;
        tmp.end=i;
        tmp.handle();

        if(result2.ch=='\0'){
            if(result1.ch==tmp.ch){
                result1.end++;
            }
            else{
                result2=tmp;
            };
            tmp=noch;
        }
        else{
            if(tmp.ch==result2.ch) {
                result2.end++;
                if (result1.end - result1.start < result2.end - result2.start) {
                    result1 = result2;
                    result2=noch;
                }
            }
            else{
                result2=tmp;
            };
            tmp=noch;
        }
        i++;
    };

    cout<<"From="<<result1.start+1<<",To="<<result1.end+1<<endl;
    cout<<"MaxLen="<<result1.end-result1.start+1<<endl;
    return 0;
}
```

## 该日是该年的第几天

### 题目概述

定义一个日期类Date，内有数据成员年、月、日，另有成员函数：构造函数用于初始化数据成员，输出，闰年的判断。 编写主函数：创建日期对象，计算并输出该日是该年的第几天。 输入格式： 测试输入包含若干测试用例，每个测试用例占一行。当读入0 0 0时输入结束，相应的结果不要输出。

### 输入样例

```
2006 3 5
2000 3 5
0 0 0
```

### 输出样例

```
64 (2006年3月5日是该年的第64天)
65 (2000年3月5日是该年的第65天)
```

### 代码

```
#include <iostream>
using namespace std;
class Date{
private:
    int year;
    int month;
    int day;
    int result=0;
    int a[12]={31,28,31,30,31,30,31,31,30,31,30,31};
public:
    Date(int x,int y,int z){
        year=x;
        month=y;
        day=z;
    };
    void getresult(){
        for(int i=1;i<month;i++){
            result=result+a[i-1];
        }
        result=result+day;
        if (month>2&&(year%100==0&&year%400==0)||(year%100!=0&&year%4==0))
            result=result+1;
        cout<<result<<endl;
    }
};

int main()
{
    const int SIZE=100;
    int m,n,p;
    int i;
    int N=0;
    Date *da[SIZE];
    cin>>m>>n>>p;
    while(m!=0&&n!=0&&p!=0){
        da[N++]=new Date(m,n,p);
        cin>>m>>n>>p;
    };
    for(i=0;i<N;i++) da[i]->getresult();


    return 0;
}
```

## 类的定义和使用

### 题目概述

定义一个日期类Date，内有数据成员year、month和day，分别代表年、月、日，并若干有成员函数：构造函数用于初始化数据成员，isLeap函数用于闰年的判断。编写主函数：创建日期对象，判断该年是否是闰年。

### 裁判测试程序样例

```
输入格式:
每组测试数据仅包含一个测试用例，每个测试用例占一行包括三个数，分别表示年、月、日。

输出格式:
如果是闰年则输出yes，不是则输出no。
```

### 输入样例

```
2006 3 5
```

### 输出样例

```
no
```

### 代码

```
#include <iostream>
using namespace std;
class Date{
private:
    int year;
    int month;
    int day;
public:
    Date(int a,int b,int c){
        year=a;
        month=b;
        day=c;
    }
    void isLeap(){
        if (month>2&&(year%100==0&&year%400==0)||(year%100!=0&&year%4==0))
            cout<<"yes";
        else
            cout<<"no";
    }
};

int  main( ){
    int a;
    int b;
    int c;
    cin>>a>>b>>c;
    Date obj(a,b,c);
    obj.isLeap();
    return 0;
}
```

## 类的定义final

### 题目概述

定义一个类DDate，内有数据成员year、month和day，分别代表年、月、日，并若干有成员函数：构造函数用于初始化数据成员，isLeap函数用于闰年的判断。编写主函数：创建日期对象，判断该年是否是闰年。

以上类名和函数名，均须按照题目要求，不得修改。

### 裁判测试程序样例

```
输入格式:
每组测试数据仅包含一个测试用例，每个测试用例占一行包括三个数，分别表示年、月、日。

输出格式:
如果该年是闰年则输出yes，不是则输出no。
```

### 输入样例

```
2006 3 5
```

### 输出样例

```
no
```

### 代码

```
#include <iostream>
using namespace std;
class DDate{
private:
    int year;
    int month;
    int day;
public:
    DDate(int a,int b,int c){
        year=a;
        month=b;
        day=c;
    }
    void isLeap(){
        if (month>2&&(year%100==0&&year%400==0)||(year%100!=0&&year%4==0))
            cout<<"yes";
        else
            cout<<"no";
    }
};

int  main( ){
    int a;
    int b;
    int c;
    cin>>a>>b>>c;
    DDate obj(a,b,c);
    obj.isLeap();
    return 0;
}
```

## 设计一个矩形类Rectangle并创建测试程序（C++）

### 题目概述

设计一个名为Rectangle的矩形类，这个类包括：两个名为width和height的double数据域，它们分别表示矩形的宽和高。width和height的默认值都为1.该类包括矩形类的无参构造函数（默认构造函数）；一个width和height为指定值的矩形构造函数；一个名为getArea( )的函数返回矩形的面积；一个名为getPerimeter( )的函数返回矩形的周长。请实现这个类。编写一个测试程序，创建一个Rectangle对象，从键盘输入矩形的宽和高，然后输出矩形的面积和周长。

### 裁判测试程序样例

```
输入格式:
3.5 35.9（第一个数表示矩形的宽，第二个数表示矩形的高，中间是空间分隔。）

输出格式:
125.65 （第一行输出矩形的面积） 78.8 （第二行输出矩形的周长）
```

### 输入样例

```
3.5 35.9
```

### 输出样例

```
125.65
78.8
```

### 代码

```
#include<iostream>
using namespace std;

class Rectangle{
public:
    double width=1;
    double height=1;
    double getArea(){return width*height;};
    double getPerimeter(){return 2*(width+height);};
};

int main(){
    double a,b;
    cin>>a>>b;
    Rectangle rect;
    rect.width=a;
    rect.height=b;
    cout<<rect.getArea()<<endl;
    cout<<rect.getPerimeter()<<endl;
    return 0;
}
```

## 队列操作

### 题目概述

请实现一个MyQueue类，实现出队，入队，求队列长度.

实现入队函数 void push(int x); 实现出队函数 int pop(); 实现求队列长度函数 int size();

### 裁判测试程序样例

```
输入格式:
每个输入包含1个测试用例。每个测试用例第一行给出一个正整数 n (n <= 10^6) ，接下去n行每行一个数字，表示一种操作： 1 x ： 表示从队尾插入x，0<=x<=2^31-1。 2 ： 表示队首元素出队。 3 ： 表示求队列长度。

输出格式:
对于操作2,若队列为空，则输出 “Invalid”,否则请输出队首元素。 对于操作3，请输出队列长度。 每个输出项最后换行。
```

### 输入样例

```
5
3
2
1 100
3
2
```

### 输出样例

```
0
Invalid
1
100
```

### 代码

```
#include<iostream>
using namespace std;

typedef struct quene{
    int data;
    struct quene *next;
} Quenedat;

class MyQuene{
private:
    Quenedat *head,*tail,*node;
    static int i;
public:
    MyQuene(){
        head=NULL;
        tail=NULL;
        node=NULL;
        head=new Quenedat;
        tail=head;
    }

    void push(int x){
        node=new Quenedat;
        node->data=x;
        tail->next=node;
        node=tail;
        i++;
    }

    void pop(){
        if(i==0) cout<<"Invalid"<<endl;
        else{
            cout<<head->next->data<<endl;
            head->next=head->next->next;
            delete[] head->next;
            i--;
        }
    }

    int size(){
        return i;
    }
};

int MyQuene::i=0;

int main(){
    MyQuene obj;
    int step, num;
    int repeat;
    cin>>repeat;
    for(int ri=0;ri<repeat;ri++) {
        cin >> step;
        if (step == 1) {
            cin >> num;
            obj.push(num);
        }
        else if (step == 2) obj.pop();
        else if (step == 3) cout << obj.size() << endl;
    }
    return 0;
}
```

## 2017final游泳池过道造价

### 题目概述

有一个圆形游泳池，现在需要在其周围建一过道，并在其四周围上矩形栅栏，如图所示。若过道造价为20元/平方米。要求计算并输出过道的造价。请定义一个Circle类，内有私有数据成员radius表示半径，并有若干成员函数；定义一个Rectangle类，内有私有数据成员length、width表示长和宽，并有若干成员函数。（设圆周率PI = 3.14159，所有数据均为double类型）

![](https://tva1.sinaimg.cn/large/0081Kckwly1gk9j8t79d0j308806tmxb.jpg)

### 裁判测试程序样例

```
输入格式:
输入一行数据a b c,分别表示游泳池半径、栅栏的长和宽。其中a>0,b>2*a,c>2*a。

输出格式:
对每一行的输入数据，输出过道的造价。
```

### 输入样例

```
3 7 8
```

### 输出样例

```
554.514
```

### 代码

```
#include <iostream>
#include <cmath>
using namespace std;
#define pi 3.14159

class Circle{
private:
    double radius;
public:
    Circle(double a):radius(a){};
    double calc=pi*pow(radius,2);
};

class Rectangle{
private:
    double length;
    double width;
public:
    Rectangle(double b, double c):length(b),width(c){};
    double calc=length*width;
};


int main(){
    double a,b,c;
    double total;
    cin>>a>>b>>c;
    Circle cir(a);
    Rectangle rec(b,c);
    total=20*(rec.calc-cir.calc);
    cout<<total<<endl;
    return 0;
}
```

## 游泳池改造

### 题目概述

一圆形游泳池如图所示，现需要在其周围建一圆形过道，并在其四周围上栅栏。栅栏价格为35元/米，过道造价为20元/平方米。游泳池半径和过道宽度由键盘输入。要求计算并输出过道和栅栏的造价。一圆形游泳池如图所示，现需要在其周围建一圆形过道，并在其四周围上栅栏。栅栏价格为35元/米，过道造价为20元/平方米。游泳池半径和过道宽度由键盘输入。要求计算并输出过道和栅栏的造价。

![](https://tva1.sinaimg.cn/large/0081Kckwly1gk9j9tdogej305o05oq2r.jpg)

### 裁判测试程序样例

```
输入格式:
分别输入A B。A代表泳池半径，B代表过道宽度。

输出格式:
对每一组输入，在第一行中输出栅栏的造价。在第二行输出过道的造价。
```

### 输入样例

```
10  3
```

### 输出样例

```
2858.85
4335.4
```

### 代码

```
#include<iostream>
#include<cmath>
using namespace std;
#define pi 3.14159265358979323846

class Pool{
private:
    double radius;
    double width;
public:
    Pool(double a,double b):radius(a),width(b){};
    void cala(){
        double m=35*2*pi*(radius+width);
        double n=20*pi*(pow(radius+width,2)-pow(radius,2));
        cout<<m<<endl;
        cout<<n<<endl;
    };
};


int main(){
    double a,b;
    cin>>a>>b;
    Pool swim(a,b);
    swim.cala();
    return 0;
}
```

## 立方体类

### 题目概述

定义立方体类Box，数据成员有长宽高且都是整数，构造函数初始化数据成员，成员函数计算体积，主函数中输入长宽高，输出立方体体积。

### 裁判测试程序样例

```
输入格式:
输入立方体的长宽高，中间用空格分隔。

输出格式:
输出体积并换行。
```

### 输入样例

```
1 2 3
```

### 输出样例

```
6
```

### 代码

```
#include<iostream>
using namespace std;

class Box{
private:
    int length;
    int width;
    int height;
    int volume;
public:
    void calv(){
        volume=length*width*height;
        cout<<volume<<endl;
    };
    Box(int a,int b,int c):length(a),width(b),height(c){};
};

int main(){
    int a,b,c;
    cin>>a>>b>>c;
    Box cal(a,b,c);
    cal.calv();
    return 0;
}
```

## 程序改错题（知识点：浅拷贝深拷贝）

### 题目概述

```
class String
{
public:
String(const char pStr = "")
{
if(NULL == pStr)
{
pstr = new char[1]; pstr = '\0';
}
else
{
pstr = new char[strlen(pStr)+1]; //加1,某位是'\0'
strcpy(pstr,pStr); //用拷贝字符串的函数
}
}

String(const String &s) : pstr(s.pstr) //浅拷贝，指向同一块空间
{ }

String & operator=(const String&s)
{
if(this != &s)
{
delete[] pstr; //将原来所指向的空间释放
pstr = s.pstr; //让pstr重新指向s的pstr所指向的空间
}
return *this;
}

~String()
{
if(NULL != pstr)
{
delete[] pstr; //释放指针所指向的内容
pstr = NULL; //将指针置为空
}
}
friend ostream&operator<<(ostream & _cout,const String &s)
{
_cout<<s.pstr;
return _cout;
}
private:
char *pstr;
}

int main()
{
String s1("sss");
String s2(s1);
String s3(NULL);
s3 = s1;
cout<<s1<<endl;
cout<<s2<<endl;
cout<<s3<<endl;
return 0;
}
```

以上程序中我们有三个String类的对象，s1调用【构造函数】存入字符"sss"，s2调用【拷贝构造函数】来利用s1进行初始化，s3则用【赋值运算符】来进行初始化，最终输出了三个“sss”。 然而，程序运行出现错误。试分析出现错误原因并改正。

### 代码

```
//错误原因
//1、pStr需定义为字符指针形式（const char* pStr）
//2、拷贝构造函数应该使用深拷贝
//3、赋值运算符重载时应当指向一块新的空间

//以下为修改后的代码

//#include <iostream>
//#include <cstring>
//using namespace std;


class String
{
public:
    String(const char *pStr = "")
    {
        if(NULL == pStr)
        {
            pstr = new char[1]; *pstr = '\0';
        }
        else
        {
            pstr = new char[strlen(pStr)+1]; //加1,某位是'\0'
            strcpy(pstr,pStr); //用拷贝字符串的函数
        }
    }

    String(const String &s){
        pstr = new char[strlen(s.pstr)+1]; //加1,某位是'\0'
        strcpy(pstr,s.pstr); //用拷贝字符串的函数
    }

    String & operator=(const String&s)
    {
        if(this != &s)
        {
            char *tmp = new char[strlen(s.pstr)+1];
            delete[] pstr; //将原来所指向的空间释放
            strcpy(pstr,s.pstr); //让pstr重新指向s的pstr所指向的空间
            pstr = tmp;
        }
        return *this;
    }

    ~String()
    {
        if(NULL != pstr)
        {
            delete[] pstr; //释放指针所指向的内容
            pstr = NULL; //将指针置为空
        }
    }
    friend ostream&operator<<(ostream & _cout,const String &s)
    {
        _cout<<s.pstr;
        return _cout;
    }
private:
    char *pstr;
};

int main()
{
    String s1("sss");
    String s2(s1);
    String s3(NULL);
    s3 = s1;
    cout<<s1<<endl;
    cout<<s2<<endl;
    cout<<s3<<endl;
    return 0;
}
```

## 类的声明和成员函数的实现--this指针

### 题目概述

本题要求声明和实现一个Car类，包括实现其成员函数。要求如下：

类和### 函数接口定义

```
1. 声明一个Car类;
2. 三个public成员函数:
(1) disp_welcomemsg()，无返回类型;
(2) get_wheels()，返回一个Car类的数据成员m_nWheels的值；
(3) set_wheels(int)，用指定的形参初始化数据成员m_nWheels的值；
3. 一个私有数据成员m_nWheels，整数类型，代表汽车的车轮数量。
```

其中，成员函数disp_welcomemsg()显示一条欢迎信息“Welcome to the car world!”。 成员函数get_wheels()返回Car类的私有数据成员m_nWheels。 成员函数set_wheels(int)用指定的形参初始化数据成员m_nWheels。

### 裁判测试程序样例

```
#include <iostream>
using namespace std;

/* 请在这里填写答案 */

int main()
{
    int n;
    cin >> n;
    Car myfstcar, myseccar;    //定义类对象
    myfstcar.disp_welcomemsg();//访问成员函数，显示欢迎信息
    myfstcar.set_wheels(n);    //访问成员函数，设置车轮数量
    myseccar.set_wheels(n+4);  //访问成员函数，设置车轮数量
    //访问成员函数，显示车轮数量
    cout << "my first car wheels num = " << myfstcar.get_wheels() << endl;
    cout << "my second car wheels num = " << myseccar.get_wheels() << endl;

    return 0;
}
```

### 输入样例

```
4
```

### 输出样例

```
Welcome to the car world!
my first car wheels num = 4
my second car wheels num = 8
```

### 代码

```
#include <iostream>
using namespace std;

class Car  //定义类Car
{
    //成员函数
public:
    void disp_welcomemsg(); //显示欢迎信息
    int get_wheels();       //返回汽车的车轮数量
    void set_wheels(int);   //设置汽车的车轮数量
    //数据成员
private:
    int m_nWheels;    //显示汽车的车轮数量
};

void Car::disp_welcomemsg() {
    cout<<"Welcome to the car world!"<<endl;
}

int Car::get_wheels() { return m_nWheels; }

void Car::set_wheels(int n) { m_nWheels=n; }
```

## 对象指针与对象数组（拉丁舞）

### 题目概述

怡山小学毕业文艺晚会上，拉丁舞是最受欢迎的节目。不过，每年为了排练这个节目，舞蹈组都会出现一些纠纷。有些同学特别受欢迎，有些却少人问津，因此安排舞伴成为舞蹈组陈老师最头疼的问题。

为了解决这一问题，今年陈老师决定让按先男生后女生，先低号后高号的顺序，每个人先报上自己期待的舞伴，每人报两位，先报最期待的舞伴。接下来按先男生后女生，先低号后高号的顺序，依次按以下规则匹配舞伴：

（１）每个人均按志愿顺序从前到后确定舞伴。如果第一志愿匹配不成功，则考虑第二志愿。

（２）如果Ａ的当前志愿为Ｂ，则如果Ｂ未匹配舞伴，且有以下情形之一者，Ａ和Ｂ匹配成功：

2a) B的期待名单中Ａ。

2b) Ｂ的期待名单中没有Ａ，但Ｂ期待的两位舞伴均已匹配成功，所以Ｂ只能与Ａ凑合。

输入时先输入男生数m, 接下来m行，第一项为学生的姓名，后两项为期待舞伴的编号，编号从０开始，最大为女生数减１。接下来输入女生数f，接下来f行，第一项为学生的姓名，后两项为期待舞伴的编号，编号从0开始，最大为男生数减１。

输出时按男生的编号顺序输出　　姓名:舞伴姓名

注意两个姓名间有冒号隔开

### 函数接口定义

```
Student的两个成员函数：
void printPair();
void addPair(); 
```

### 裁判测试程序样例

```
#include <iostream>
#include <string>
using namespace std;
const int K=2;
const int N=20;
class Student{
  string name;
  Student *welcome[K];
  Student *pair;
  public:
      void init(string &name, Student *a, Student *b) {
        this->name=name; 
        welcome[0]=a;
        welcome[1]=b;
        pair=NULL;
      }
     void printPair();
     void addPair();        
};

/* 请在这里填写答案 */

int main(){
    Student male[N], female[N];
    int m, f, i, j, a, b;
    string name;
    cin>>m;
    for(i=0;i<m;i++){
      cin>>name>>a>>b;
      male[i].init(name, &female[a], &female[b]);
    }
    cin>>f;
    for(i=0;i<f;i++){
      cin>>name>>a>>b;
      female[i].init(name, &male[a], &male[b]);
    }
    for(i=0;i<m;i++) male[i].addPair();
    for(i=0;i<f;i++) female[i].addPair();
    for(i=0;i<m;i++) male[i].printPair();
    return 0;
}
```

### 输入样例

```
5
M0 3 1
M1 1 3
M2 1 4
M3 3 1
M4 0 3
5
F0 0 2
F1 2 0
F2 2 1
F3 2 4
F4 3 2
```

### 输出样例

```
M0:F1
M2:F4
M4:F0
```

说明：匹配过程如下：

（１）M0先选择F3, 但F3并未期待M0；接下来M0选择F1, F1也期待M0，故匹配成功。

（２）Ｍ１选择F1, 但F1已匹配，故,不成功；Ｍ１选择Ｆ３，但Ｆ３未期待M1，仍然不成功。

（３）Ｍ２选择Ｆ１，Ｆ１已匹配；Ｍ２选择Ｆ４，　Ｆ４未匹配且也期待Ｍ２，故匹配成功。

（４）Ｍ３选择Ｆ３，但Ｆ３未期待他，不成功；Ｍ３选择Ｆ１，Ｆ１已匹配，不成功。

（５）Ｍ４选择Ｆ０，　Ｆ０不期待Ｍ４，但是Ｆ０期待的Ｍ０和Ｍ２已分配，所以凑合，匹配成功。

（６）Ｆ０已匹配，　Ｆ１已匹配。

（７）Ｆ２选择Ｍ２，　Ｍ２已匹配，不成功；　Ｆ２选择Ｍ１，　Ｍ１未匹配，但期待表中没有Ｆ２，且Ｆ３也未分配，故不成功。

（８）Ｆ３选择Ｍ２，　Ｍ２已匹配，不成功；Ｆ３选择Ｍ４，　Ｍ４已匹配，不成功。

（９）Ｆ４已匹配。

### 代码

```
void Student::addPair() {
    if(this->pair!=NULL) return;
    int i, j;
    for(i=0;i<2;i++){
        if(this->welcome[i]->pair!=NULL) continue;
        for(j=0;j<2;j++){
            if(this->welcome[i]->welcome[j]==this){
                this->pair = this->welcome[i];
                this->welcome[i]->pair=this;
                return;
            }
        }

        for(j=0;j<2;j++){
            if(this->welcome[j]->welcome[0]->pair != NULL && this->welcome[j]->welcome[1]->pair != NULL && this->welcome[j]->pair == NULL){
                this->pair = this->welcome[j];
                this->welcome[j]->pair=this;
                return;
            }
        }
    }
}

void Student::printPair() {
    if(this->pair != NULL)
        cout<<this->name<<":"<<this->pair->name<<endl;
}
```

## 输出最大值

### 题目概述

根据给定的程序，写成相关的成员函数，完成指定功能。

### 函数接口定义

定义max函数，实现输出最高成绩对应的学号以及最高成绩值。

### 裁判测试程序样例

```
#include <iostream>
using namespace std;
class Student
 {public:
   Student(int n,float s):num(n),score(s){}
   int num;
   float score;
 };

int main()
{Student stud[5]={
  Student(101,78.5),Student(102,85.5),Student(103,98.5),
  Student(104,100.0),Student(105,95.5)};
 void max(Student* );
 Student *p=&stud[0];
 max(p);
 return 0; 
 }
/* 请在这里填写答案 */
```

### 输出样例

```
104 100
```

### 代码

```
void max(Student *p){
    int max = p[0].score;
    int i;
    int index;
    for(i=1;i<5;i++){
        if (p[i].score>max){
            max = p[i].score;
            index = i;
        }
    }
    cout<<p[index].num<<" "<<max<<endl;
}
```

## 运动成绩排名

### 题目概述

某大学开田径运动会，现有12名选手参加100米比赛，对应的运动员号及成绩如表所示，请按照成绩排名并输出，要求每一行输出名次、运动员号及成绩。

```
运动员号    成绩（秒）   运动员号    成绩（秒）
001        13.6       031        14.9
002        14.8       036        12.6
010        12.0       037        13.4
011        12.7       102        12.5
023        15.6       325        15.3
025        13.4       438        12.7
```

使用给定的类，完成数据的对象的定义，并按照成绩排序后输出（格式见输出样例）

### 裁判测试程序样例

```
#include <iostream>
#include <iomanip>
#include <string>
using namespace std; 
class Sport 
{
  public: 
       Sport() { }
    Sport(string n,double g)
    {
        num=n;
        grade=g;
    } 
    string num; 
    double grade;   
};  
```

### 输出样例

注意：使用setw（）进行宽度设置，每个输出项占6列。

```
     1   010    12
     2   102  12.5
     3   036  12.6
     4   011  12.7
     5   438  12.7
     6   025  13.4
     7   037  13.4
     8   001  13.6
     9   002  14.8
    10   031  14.9
    11   325  15.3
    12   023  15.6
```

### 代码

```
int main(){
    Sport sp[12]={
            Sport("001",13.6),Sport("002",14.8),Sport("010",12.0),Sport("011",12.7),Sport("023",15.6),Sport("025",13.4),
            Sport("031",14.9),Sport("036",12.6),Sport("037",13.4),Sport("102",12.5),Sport("325",15.3),Sport("438",12.7)
    };
    int i,j;
    Sport tmp;
    for(i=0;i<12;i++){
        for(j=i;j<12;j++){
            if(sp[i].grade>sp[j].grade){
                tmp = sp[i];
                sp[i] = sp[j];
                sp[j] = tmp;
            }
        }
    }

    for(i=0;i<12;i++)
        cout<<setw(6)<<i+1<<setw(6)<<sp[i].num<<setw(6)<<sp[i].grade<<endl;
}
```

## 对象数组初始化

### 题目概述

根据类定义，进行对象数组的定义，按照输出样例输出相应的数据。

编写main函数，定义三个对象数组，完成指定的输出。

### 裁判测试程序样例

```
#include<iostream>
using namespace std;
class A{
    int data;
public:
    A(int k=0){
        data=k;
    }
    void show(){
        cout<<"data="<<data<<endl;
    }
};

/* 请在这里填写答案 */
```

### 输出样例

```
data=0
data=0
data=0

data=1
data=0
data=0

data=1
data=2
data=3
```

### 代码

```
int main(){
    int i;
    A a[3];
    for(i=0;i<3;i++) a[i].show();
    cout<<endl;
    A b[3]={1};
    for(i=0;i<3;i++) b[i].show();
    cout<<endl;
    A c[3]={1,2,3};
    for(i=0;i<3;i++) c[i].show();
}
```

## 自定义的学生类

### 题目概述

本题要求定义一个简单的学生类，数据成员仅需要定义学号和姓名，函数成员的原型见给出的代码，请给出函数成员的类外完整实现。

### 函数接口定义

```
class Student
{
private:
    int m_id;
    char m_name[10];
public:
    Student(int id=0,char *name="");
    ~Student();
    void print();

};
```

其中m_id和m_name分别表示学生的学号和姓名，类型已经定义好。类内声明了3个成员函数，分别表示构造函数、析构函数和用于输出学生信息的函数。 构造函数要完成两个数据成员的初始赋值，并做一行输出，形如：“Hi!学号 姓名”。 析构函数要求输出一行，形如：“Bye!学号 姓名”。 print函数要求在一行中输出学生信息，形如：“学号 姓名”。 注：学号和姓名之间用一个空格分开。

### 裁判测试程序样例

```
#include <iostream>
#include <cstring>
using namespace std;

int main()
{
    Student stu_array[3]={Student(1,"Zhang"),Student(2,"Wang")};
    return 0;
}

/* 请在这里填写答案 */
```

### 代码

```
Student::Student(int id, char *name){
    m_id=id;
    strcpy(m_name,name);
    cout<<"Hi!"<<m_id<<" "<<m_name<<endl;
}

Student::~Student(){
    cout<<"Bye!"<<m_id<<" "<<m_name<<endl;
}

void Student::print() {
    cout<<m_id<<" "<<m_name<<endl;
}
```

## Tree类的构造函数和成员函数

### 题目概述

定义一个Tree（树）类，有成员ages（树龄），不带参数的构造函数对ages初始化为1，成员函数grow(int years)对ages加上years，age()显示tree对象的ages的值。

Tree类声明如下：

```
class Tree {
public:
    Tree();//构造函数
    void grow(int years);//对数龄ages加上years
    void age();//显示数龄ages的值
private:
    int ages;//树龄
};
```

请实现Tree类的构造函数和成员函数。

### 裁判测试程序样例

```
#include <iostream>
using namespace std;

class Tree {
public:
    Tree();//构造函数
    void grow(int years);//对数龄ages加上years
    void age();//显示数龄ages的值
private:
    int ages;//树龄
};

int main()
{
    Tree tree;
    int years;
    cin >> years;
    tree.grow(years);
    tree.age();

    return 0;
}

/* 你的代码将被嵌在这里 */
```

### 输入样例

```
30
```

### 输出样例

```
31
```

### 代码

```
Tree::Tree():ages(1){}
void Tree::grow(int years) { ages += years; }
void Tree::age() { cout<<ages<<endl; }
```

## Point类的声明和实现

### 题目概述

定义一个Point类，代表平面上的一个点，其横坐标和纵坐标分别用x和y表示，设计Point类的成员函数，实现并测试这个类。 主函数中输入两个点的坐标，计算并输出了两点间的距离。请根据主函数实现Point类。

### 裁判测试程序样例

```
#include <iostream>
#include <iomanip>
#include <cmath>
using namespace std;

//你的代码将被嵌在这里

int main()
{
    Point p1, p2;
    double x1, y1, x2, y2;
    cin >> x1 >> y1 >> x2 >> y2;
    p1.setX(x1);
    p1.setY(y1);
    p2.setX(x2);
    p2.setY(y2);
    double x = p1.getX() - p2.getX();
    double y = p1.getY() - p2.getY();
    double len = sqrt(x * x + y * y);
    cout << fixed << setprecision(2) << len << endl;
    return 0;
}
```

### 输入样例

```
0 0 3 3
```

### 输出样例

```
4.24
```

### 代码

```
class Point{
private:
    double x;
    double y;
public:
    void setX(int x1){ x=x1; }
    void setY(int y1){ y=y1; }
    double getX(){ return x; }
    double getY(){ return y; }
};
```

## 类的声明与成员函数的实现--Car类

### 题目概述

本题要求根据给定的Car类的声明，实现其成员函数。

类和### 函数接口定义

```
class Car  //定义类Car
{
    //成员函数
public:
    void disp_welcomemsg(); //显示欢迎信息
    int get_wheels();       //返回汽车的车轮数量
    void set_wheels(int);   //设置汽车的车轮数量
    //数据成员
private:
    int m_nWheels;    //汽车的车轮数量
};
```

其中，成员函数void disp_welcomemsg()显示一条欢迎信息“Welcome to the car world!”。 成员函数int get_wheels()返回Car类的私有数据成员m_nWheels。 成员函数int set_wheels(int)用指定的形参初始化数据成员m_nWheels。

### 裁判测试程序样例

```
#include <iostream>
using namespace std;

class Car  //定义类Car
{
    //成员函数
public:
    void disp_welcomemsg(); //显示欢迎信息
    int get_wheels();       //返回汽车的车轮数量
    void set_wheels(int);   //设置汽车的车轮数量
    //数据成员
private:
    int m_nWheels;    //显示汽车的车轮数量
};

/* 请在这里填写答案 */

int main()
{
    int n;
    cin >> n;
    Car mycar;     //定义类对象mycar
    mycar.disp_welcomemsg();  //访问成员函数，显示欢迎信息
    mycar.set_wheels(n);      //访问成员函数，设置车轮数量
    //访问成员函数，显示车轮数量
    cout << "wheels = " << mycar.get_wheels() << endl;
    return 0;
}
```

### 输入样例

```
4
```

### 输出样例

```
Welcome to the car world!
wheels = 4
```

### 代码

```
void Car::disp_welcomemsg() {
    cout<<"Welcome to the car world!"<<endl;
}

int Car::get_wheels() { return m_nWheels; }

void Car::set_wheels(int n) { m_nWheels=n; }
```

## 实现数组类（C++ 拷贝构造函数、拷贝函数）

### 题目概述

裁判测试程序样例中展示的是一段实现“数组类”的代码，其中缺失了部分代码，请补充完整，以保证测试程序正常运行。

### 函数接口定义

```
提示：要想程序正确运行，至少需要补充以下函数（可能还需要补充其他函数）：
1. 带参构造函数
2. 拷贝构造函数
3. 拷贝函数（赋值运算符重载）
```

### 裁判测试程序样例

```
#include <iostream>
using namespace std;
class ArrayIndexOutOfBoundsException{  // 异常类
public:
    int index;
    ArrayIndexOutOfBoundsException(int k){
        index = k;
    }
};
class Array{
private:
    int *data;
    int size;
    static const int dSize = 10;   // 数组默认大小
public:
    Array( ){  // 无参构造
        size = dSize;
        data = new int[size]( );
    }
        
/** 你提交的代码将被嵌在这里（替换本行内容） **/        
        
    int& operator [] (int k){     // 运算符 [ ] 重载，以方便数组的使用
        if(k<0 || k>=size) throw ArrayIndexOutOfBoundsException(k);
        return data[k];
    }
    friend ostream& operator << (ostream& o, const Array& a);   // 运算符 << 重载，以方便输出
};
ostream& operator << (ostream& o, const Array& a){
    o << '[' ;
    for(int i=0; i<a.size-1; i++)
        o << a.data[i] << ',' ;
    o << a.data[a.size-1] << ']';
    return o;
}
// 注意：实际测试程序中，在此处之前的代码与样例中相同
// 注意：实际测试程序中，在此处之后的代码（即main函数）可能与样例中不同
int main(){
    int n, k;
    cin >> n >> k;
    Array a(n);  // 构造数组，大小为 n
    for(int i=0; i<n; i++) a[i] = i;
    Array b = a;  // 拷贝构造数组
    b[n/2] = k;
    cout << a << endl;
    cout << b << endl;
    Array c;  // 构造数组，默认大小
    c = a; // 拷贝数组
    c[n/2] = k;
    cout << a << endl;
    cout << c << endl;
    a = a;
    a[n/2] = 2223;
    cout << a << endl;
    return 0;
}
```

### 输入样例

```
15 666
```

### 输出样例

```
[0,1,2,3,4,5,6,7,8,9,10,11,12,13,14]
[0,1,2,3,4,5,6,666,8,9,10,11,12,13,14]
[0,1,2,3,4,5,6,7,8,9,10,11,12,13,14]
[0,1,2,3,4,5,6,666,8,9,10,11,12,13,14]
[0,1,2,3,4,5,6,2223,8,9,10,11,12,13,14]
```

### 代码

```
    Array(int n1){  // 带参构造
        size = n1;
        data = new int[size]( );
    }

    Array(const Array& p){  // 拷贝构造函数
        size = p.size;
        data = new int[size];
        for(int i=0;i<size;i++)
            data[i]=p.data[i];
    }

    ~Array(){
        delete[] data;
    }

    Array & operator=(const Array & p) {
        if (p.size != size) {
            delete[] data;
            size = p.size;
            data = new int[size];
        }
        for (int i = 0; i < size; i++) {
            data[i] = p.data[i];
        }
        return *this;
    }
```

## 计算捐款总量

### 题目概述

这里需要设计一个捐款人类Donator及一个相关函数getMaxName( )，Donator类中包含捐款人的姓名及其捐款额，其部分代码如下：

```
class Donator{
    private:
        string name; //捐款人姓名
        float money; //捐款金额，单位：元        
    public:
        void setName(string _name);
        void setMoney(float _money);
        string getName(){return name;}
        float getMoney(){return money;}
```

请根据题意将代码补充完整，以输出一批捐款人来到前后的捐款总金额，以及本批次捐款人中捐款最高者的姓名，题目保证捐款人数不少于1人。

### 裁判测试程序样例

```
#include <iostream>
using namespace std;

class Donator{
    private:
        string name;
        float money; //单位：元     
    public:
        void setName(string _name);
        void setMoney(float _money);
        string getName(){return name;}
        float getMoney(){return money;}
        
/* 你编写的代码将被嵌入这里*/

//读取n个捐款人的姓名和捐款额 
void read(Donator dt[],int n){
    string name;
    float money;
    for(int i=0;i<n;i++){
        cin>>name>>money;
        dt[i].setName(name);
        dt[i].setMoney(money);
    }
}

int main(){
    int n;
    cin>>n; ////输入本批次将参与的捐款人数
    cin>>Donator::totalMoney; //输入目前已有的捐款总额 
    Donator::printTotal();
    Donator d[n];
    read(d,n);
    Donator::printTotal();
    cout<<getMaxName(d,n)<<endl;//输出本批次中捐款最高者姓名 
    return 0;    
} 
```

### 输入样例

第一行为捐款人数及当前的捐款总额，第二行开始每行为一个捐款人的姓名和个人捐款金额。

```
3 28.5
Xiaoyu 12
Mike 81.5
Joey  50
```

### 输出样例

输出本批次捐款人到达前后的捐款总额，及本批次中捐款最高者的姓名。

```
total:28.5
total:172
Mike
```

### 代码

```
    static float totalMoney;
    static void printTotal(){
        cout<<"total:"<<totalMoney<<endl;
    }

};

float Donator::totalMoney = 0;

void Donator::setName(string _name) {
    name = _name;
}

void Donator::setMoney(float _money) {
    money = _money;
    totalMoney += money;
}

string getMaxName(Donator dt[],int n){
    int index=0;
    for(int i=1;i<n;i++)
        if(dt[i].getMoney()>dt[index].getMoney()) index=i;
    return dt[index].getName();
}
```

## 2017final友元函数之全班同学的平均绩点

### 题目概述

一个学生类，有三个私有成员：名字name、课程学分指针score、课程成绩指针grade。定义一个友元函数，求全班同学的平均绩点。单门课程的学分绩点=学分*绩点=学分*(成绩/10-5) ; 全班同学的平均绩点是 所有同学的全部课程的学分绩点之和/所有同学学分数之和。单个同学的课程数不超过100门。全班同学人数不超过100名。

输入说明：

输入若干行。

每行一个学生的信息：第一个输入是学生的名字，第二个输入是第一门课程的学分，第三个输入是第一门课程的成绩，第四个输入是第二门课程的学分，第五个输入是第二门课程的成绩，以此类推，最后以-1表示该行输入结束。每个学生的课程数不超过100门。

最后以 no 表示输入结束。

输出一行，即该全班同学的平均绩点。

### 函数接口定义

```
这是求全部同学平均绩点的友元函数的声明：
friend double averagegrade(student *stu, int count)
```

其中 *stu 和 count 都是用户传入的参数。 *stu 是传入的学生对象数组的首指针，count是全班学生数量。

### 裁判测试程序样例

```
#include<iostream>
#include<string>
using namespace std;
class student{
   private:
      double *grade;
      double *score;
      string name;
public:
      student( )
     {
      grade=NULL;
      score=NULL;
      }
      student(string n, double *g, double *s)
      {
            name=n;
            grade=g;
            score=s;
       }
     friend double averagegrade(student *stu, int count);
};
/* 请在这里填写答案 */


int main()
{
   student stu[100];
   double s[100][100], g[100][100];
   int count=0;
   string n;
   for(int i=0;i<100;i++)
   {
         cin>>n;
         if(n=="no") break;
         count++;
         for(int j=0;j<100;j++)
        {
            cin>>s[i][j];
            if(s[i][j]==-1) break;
            cin>>g[i][j];
        }
       stu[i]=student(n, g[i], s[i]);
   }
   cout<<averagegrade(stu, count);
   return 0;
}
```

### 输入样例

```
bob 3 90 2 68.5 2.5 50 -1
andy 3 80 2 77 -1
no
```

### 输出样例

```
2.408
```

### 代码

```
double averagegrade(student *stu, int count){
    int i,j;
    double tgrade=0;
    double tscore=0;
    for(i=0;i<count;i++){
        for(j=0;stu[i].score[j]!=-1;j++){
            tscore += stu[i].score[j];
            tgrade += stu[i].score[j]*(stu[i].grade[j]/10-5);
        }
    }
    if(tgrade==0||tscore==0) return 0;
    else return tgrade/tscore;
}
```

## 车与船的重量

### 题目概述

定义一boat与car两个类，二者都有weight属性，定义二者的一个友元函数totalweight()，计算二者的重量和。

### 裁判测试程序样例

```
在这里给出函数被调用进行测试的例子。例如：
#include <iostream>
using namespace std;

/* 请在这里填写答案 */

int main()
{
  int c,b;
  cin>>c>>b;
  car c1(c);
  boat b1(b);
  cout<<totalweight(b1,c1)<<endl;
}
```

### 输入样例

```
1000 2000
```

### 输出样例

```
3000
```

### 代码

```
class boat;
class car{
private:
    double weight;
public:
    car(double w):weight(w){};
    friend double totalweight(boat &p,car &q);
};

class boat{
private:
    double weight;
public:
    boat(double w):weight(w){};
    friend double totalweight(boat &p,car &q);
};

double totalweight(boat &p,car &q){
    return p.weight+q.weight;
}
```

## 方阵的转置

### 题目概述

编写一个类用于处理3×3矩阵转置，测试转置的效果，输出转置前后的矩阵。

根据要求写出类，并可以使得主函数正确运行，得到对应的结果。

### 裁判测试程序样例

```
/* 请在这里填写答案 */

int main(){
    Matrix m;
    m.input();
    m.show();
    m.transform();
    m.show();
}
```

### 输入样例

```
1 2 3
4 5 6
7 8 9
```

### 输出样例

```
datas:
 1 2 3
 4 5 6
 7 8 9
datas:
 1 4 7
 2 5 8
 3 6 9
```

### 代码

```
#include <iostream>
using namespace std;
class Matrix{
private:
    double matr[3][3]={0,0,0,0,0,0,0,0,0};
public:
    void input(){
        for(int i=0;i<3;i++)
            for(int j=0;j<3;j++)
                cin>>matr[i][j];
    };
    void show(){
        cout<<"datas:\n";
        for(int i=0;i<3;i++) {
            for (int j = 0; j < 3; j++)
                cout << " " << matr[i][j];
            cout<<"\n";
        }
    };
    void transform(){
        double temp[3][3]={0,0,0,0,0,0,0,0,0};;
        for(int i=0;i<3;i++)
            for(int j=0;j<3;j++)
                temp[i][j]=matr[j][i];
        for(int i=0;i<3;i++)
            for(int j=0;j<3;j++)
                matr[i][j]=temp[i][j];
    }
};
```

## 二维向量相加（C++ 运算符重载）

### 题目概述

裁判测试程序样例中展示的是一段二维向量类TDVector的定义以及二维向量求和的代码，其中缺失了部分代码，请补充完整，以保证测试程序正常运行。

### 函数接口定义

```
提示：需要补充的函数有：
1. 带参构造函数
2. getX
3. getY
4. setX
5. setY
6. 运算符重载函数
```

### 裁判测试程序样例

```
#include <iostream>
#include <iomanip>
using namespace std;
class TDVector{
private:
    double x;
    double y;
public:
    TDVector(){
        x = y = 0;
    }
/**    你提交的代码将被嵌在这里（替换本行内容）  **/
};
int main(){
    TDVector a;
    double x, y;
    cin >> x >> y;
    TDVector b(x, y);
    cin >> x >> y;
    TDVector c;
    c.setX(x);
    c.setY(y);
    TDVector d;
    d = a + b + c;
    cout << fixed << setprecision(2) << d.getX() << ' ' << d.getY();
    return 0;
}
```

### 输入样例

```
1.1 2.2
3.3 4.4
```

### 输出样例

```
4.40 6.60
```

### 代码

```
    TDVector(double x1,double y1):x(x1),y(y1){}

    void setX(double x2){x=x2;}
    void setY(double y2){y=y2;}
    double getX(){return x;}
    double getY(){return y;}

    TDVector operator+(const TDVector& p){
        TDVector td;
        td.x=x+p.x;
        td.y=y+p.y;
        return td;
    }
```

## 求Box的体积（类的定义与使用）

### 题目概述

本题要求实现一个类定义，可完成长方柱体积的计算与输出。

类定义：类的名称为Box，完成类体的声明与实现。

### 裁判测试程序样例

```
在这里给出根据类进行对象定义与使用的主函数的示例：
int main()
{
 Box box2;
 box2.get_value();
 box2.display(); 
 return 0;
}

/* 请在这里填写答案 */
```

### 输入样例

```
2 3 5
```

### 输出样例

```
30
```

### 代码

```
#include <iostream>
using namespace std;

class Box{
private:
    double length;
    double width;
    double height;
public:
    void get_value(){ cin>>length>>width>>height; }
    void display(){ cout<<length*width*height<<endl; }
};
```

## 宿舍谁最高？

### 题目概述

学校选拔篮球队员，每间宿舍最多有4个人。现给出宿舍列表，请找出每个宿舍最高的同学。定义一个学生类Student,有身高height，体重weight等。

### 裁判测试程序样例

```
输入格式:
首先输入一个整型数n （1<=n<=1000000），表示n位同学。
紧跟着n行输入,每一行格式为：宿舍号，name,height,weight。
宿舍号的区间为[0,999999]， name 由字母组成，长度小于16，height，weight为正整数。

输出格式:
按宿舍号从小到大排序，输出每间宿舍身高最高的同学信息。题目保证每间宿舍只有一位身高最高的同学。输入格式:
首先输入一个整型数n （1<=n<=1000000），表示n位同学。
紧跟着n行输入,每一行格式为：宿舍号，name,height,weight。
宿舍号的区间为[0,999999]， name 由字母组成，长度小于16，height，weight为正整数。

输出格式:
按宿舍号从小到大排序，输出每间宿舍身高最高的同学信息。题目保证每间宿舍只有一位身高最高的同学。
```

### 输入样例

```
7
000000 Tom 175 120
000001 Jack 180 130
000001 Hale 160 140
000000 Marry 160 120
000000 Jerry 165 110
000003 ETAF 183 145
000001 Mickey 170 115
```

### 输出样例

```
000000 Tom 175 120
000001 Jack 180 130
000003 ETAF 183 145
```

### 代码

```
#include <iostream>
#include <cstring>
#include<iomanip>
using namespace std;

class Student{
public:
    int no;
    string name;
    int height;
    int weight;
    Student(){
        height = -1;
        weight = -1;
    };
    Student(int no1,string name1,int height1,int weight1){
        no=no1;
        name=name1;
        height=height1;
        weight=weight1;
    }
};

int main(){
    int i, n;
    int no, height, weight;
    string name;
    Student stu[100];
    cin>>n;
    for(i=0; i<n; i++){
        cin>>no>>name>>height>>weight;
        if(stu[no].height<height)
            stu[no]=Student(no,name,height,weight);
    }

    for(i=0; i<100; i++) {
        if(stu[i].height!=-1&&stu[i].weight!=-1)
            cout<<setfill('0')<<setw(6)<<stu[i].no<<" "<<stu[i].name<<" "<<stu[i].height<<" "<<stu[i].weight<<endl;
    }
}
```

## 鸿鸿哥的苹果树

### 题目概述

鸿鸿哥家的院子里有一棵桃子树，每到秋天树上就会结出10个桃子。桃子成熟的时候，鸿鸿哥就会跑去摘桃子。鸿鸿哥有个30厘米高的板凳，当他不能直接用手摘到桃子的时候，就会踩到板凳上再试试。 现在已知10个桃子到地面的高度，以及鸿鸿哥把手伸直的时候能够达到的最大高度，请帮鸿鸿哥算一下他能够摘到的桃子的数目。假设他碰到桃子，桃子就会掉下来。

### 裁判测试程序样例

```
输入格式:
第一行中给出10个桃子到地面的高度(cm)。

第二行中输入鸿鸿哥把手伸直能够达到的最大高度(cm)。

输出格式:
输出鸿鸿哥能够摘到的桃子数目n。
```

### 输入样例

```
26 95 43 77 49 31 87 19 35 65
40
```

### 输出样例

```
7
```

### 代码

```
#include <iostream>
using namespace std;

int main(){
    int peach[10];
    int length;
    int i;
    int sum=0;
    for(i=0; i<10; i++) cin>>peach[i];
    cin>>length;
    for(i=0; i<10; i++)
        if(peach[i]<=length+30) sum++;
    cout<<sum<<endl;
}
```

## 立方体类的实现

### 题目概述

立方体类Box的实现，完成计算体积、计算表面积、输出结果等功能。其中给定的主函数为：

```
int  main( ){
    float ab;
    cin>>ab;
    Box  obj;
    obj.seta( ab );
    obj.getvolume( );
    obj.getarea( );
    obj.disp( );
    return 0;
}
```

### 裁判测试程序样例

```
输入格式:
立方体的边长，可以是float类型的数据。

输出格式:
立方体的体积和表面积，中间用一个空格隔开，末尾换行。
```

### 输入样例

```
3
```

### 输出样例

```
27 54
```

### 代码

```
#include <iostream>
#include <cmath>
using namespace std;
class Box{
private:
    int length;
    int volume;
    int square;
public:
    void seta(float a){
        length=a;
    }
    void getvolume(){
        volume=pow(length,3);
    }
    void getarea(){
        square=6*pow(length,2);
    }
    void disp(){
        cout<<volume<<" "<<square;
    }
};

int  main( ){
    float ab;
    cin>>ab;
    Box  obj;
    obj.seta(ab);
    obj.getvolume( );
    obj.getarea( );
    obj.disp( );
    return 0;
}
```

## 计算全班学生C++课程的总成绩和平均成绩

### 题目概述

定义一个类Student，记录学生C++课程的成绩。要求使用静态数据成员或静态成员函数计算全班学生C++课程的总成绩和平均成绩。

### 裁判测试程序样例

```
输入格式:
输入5个不超过100的正整数，作为C++成绩。

输出格式:
在第一行中输出成绩的和，第二行输出平均成绩。
```

### 输入样例

```
90 80 70 60 50
```

### 输出样例

```
350
70
```

### 代码

```
#include <iostream>
using namespace std;

class Student{
private:
    int score;
public:
    static int total;
    Student(int score1):score(score1){
        total += score;
    }
};

int Student::total=0;

int main(){
    int num;
    for(int i=0;i<5;i++){
        cin>>num;
        Student a(num);
    }
    cout<<Student::total<<endl;
    cout<<Student::total/5<<endl;
}
```

## 友元函数

### 题目概述

C++考试正在进行。请设计一个学生类student，学号、本次考试成绩是其私有数据成员，同时有一个计算本次考试平均成绩的友元函数 double average(student *p,int count)

以上类名和友元函数的形式，均须按照题目要求，不得修改。

### 裁判测试程序样例

```
输入是 学号（[00001,99999]）和成绩，以0结束。（不超过100个学生）
输出是平均成绩。
```

### 输入样例

```
10001 90
10002 93
0
```

### 输出样例

```
91.5
```

### 代码

```
#include <iostream>
#include <cstring>
using namespace std;

class student{
private:
    int no;
    double score;
public:
    student(){
        score = 0;
        no = 0;
    };
    void setstud(int no1,double score1){
        score = score1;
        no = no1;
    }

    friend double average(student *p,int count);
};

double average(student *p,int count){
    int i;
    double total;
    for(i=0;i<count;i++)
        total += p[i].score;
    return total/count;
}

int main(){
    student stu[100];
    int no1;
    int i;
    int count=0;
    double score1;

    for(i=0;i<100;i++){
        cin>>no1;
        if(no1==0) break;
        cin>>score1;
        stu[i].setstud(no1,score1);
        count++;
    }
    cout<<average(stu,count)<<endl;
}
```

## 程序猿和产品狗

### 题目概述

在公司里面，程序猿经常有一堆todolist要做，而这些todolist是产品经理分配给他们的。但是当程序员遇到不懂技术的产品狗时，就悲剧了。产品经理经常修改他们的todolist，比如：添加，减少他们的todolist。

请设计一个类CodeMonkey ，表示程序猿，另一个类ProductDog，表示产品经理。

CodeMonkey类有私有成员 name，todolist。
构造函数初始化姓名和todolist，公有函数 int sizeof_todolist()， 来自ProductDog类的友元函数 add_todolist(CodeMonkey&,int),reduce_todolist(CodeMonkey& , int);

ProductDog类有公有函数 add_todolist(CodeMonkey&,int),reduce_todolist(CodeMonkey& , int)

### 裁判测试程序样例

```
Input Specification

每个测试文件包含一组测试用例，对于每个测试用例，第一行输入 n (1<= n <= 10^6)表示有n个程序员，接下去n行，每行为: name x 表示名为name的程序员的todolist的长度为x。 接下去一行 m (1<=m<=10^6)，表示产品经理分配任务的次数。 接下去m行，每行为 name opt x ， opt 为 0 表示名为name的程序猿的todolist增加x，opt为 1 表示减少x。 输入数据保证合法性。

Output Specification

对于每个程序员，请输出 name x，x表示最终的todolist长度。按输入顺序输出。
```

### 输入样例

```
3
Jack 1
Luck 2
Tom 3
4
Tom 0 100
Luck 0 50
Jack 0 25
Tom 1 50
```

### 输出样例

```
Jack 26
Luck 52
Tom 53
```

### 代码

```
#include <iostream>
using namespace std;

class CodeMonkey;

class ProductDog{
public:
    void add_todolist(CodeMonkey&,int);
    void reduce_todolist(CodeMonkey&, int);
};

class CodeMonkey{
private:
    string name;
    int todolist;
public:
    CodeMonkey(){}
    CodeMonkey(string n, int t):name(n),todolist(t){}
    int sizeof_todolist(){
        cout<<name<<" "<<todolist<<endl;
        return 0;
    }
    friend void ProductDog::add_todolist(CodeMonkey&,int);
    friend void ProductDog::reduce_todolist(CodeMonkey& , int);
};



void ProductDog::add_todolist(CodeMonkey& p,int x){
    p.todolist += x;
}

void ProductDog::reduce_todolist(CodeMonkey& p, int x){
    p.todolist -= x;
}


int main(){
    int i, j, n1, n;
    string name,na[100];
    int opt, todolist;
    CodeMonkey cm[100];
    ProductDog pd;
    cin>>n1;
    for(i=0;i<n1;i++){
        cin>>name>>todolist;
        na[i] = name;
        cm[i] = CodeMonkey(name,todolist);
    }
    cin>>n;
    for(i=0;i<n;i++){
        cin>>name>>opt>>todolist;
        for(j=0;j<n;j++)
            if(name == na[j]) break;
        if(opt == 0){
            pd.add_todolist(cm[j],todolist);
        }
        else if(opt == 1){
            pd.reduce_todolist(cm[j],todolist);
        }
    }

    for(i=0;i<n1;i++){
        cm[i].sizeof_todolist();
    }

}
```

## 计算高考状元

### 题目概述

高考成绩已经公布，大家正在填报志愿。设计一个学生类student，四门学科成绩是其私有成员，分别是语文、数学、英语、综合。有个计算高考状元的函数是其友元函数,其形式是 student top(const student *p, int count) 。

以上类名和友元函数的形式，均须按照题目要求，不得修改。

### 裁判测试程序样例

```
输入是姓名 和 四科成绩，以0结束。 （不超过100个学生） 输出是状元的总分。
```

### 输入样例

```
Alice 105 107 107 230
Bob 112 120 120 250
0
```

### 输出样例

```
602
```

### 代码

```
#include <iostream>
using namespace std;

class student{
private:
    string name;
    double chinese;
    double math;
    double english;
    double comp;
public:
    student(){}
    student(string na,double ch,double ma,double en,double co){
        name = na;
        chinese = ch;
        math = ma;
        english = en;
        comp = co;
    }
    friend student top(const student *p, int count);
    double gettotal(){ return chinese+math+english+comp; }
};

student top(const student *p, int count){
    int i;
    int index=0;
    double total=p[0].chinese+p[0].math+p[0].english+p[0].comp;
    double tmp;
    for(i=1;i<count;i++){
        tmp = p[i].chinese+p[i].math+p[i].english+p[i].comp;
        if(tmp > total){
            total = tmp;
            index = i;
        }
    }
    return p[index];
}

int main(){
    int i;
    int count=0;
    string name;
    double chinese, math, english, comp;
    student stu[100];
    for(i=0;i<100;i++){
        cin>>name;
        if(name=="0") break;
        cin>>chinese>>math>>english>>comp;
        stu[i]=student(name,chinese,math,english,comp);
        count++;
    }
    student toper=top(stu,count);
    cout<<toper.gettotal()<<endl;
}
```

## 友元很简单2016final

### 题目概述

C++考试正在进行。请设计一个学生类student，学号、本次考试成绩是其私有数据成员，同时有一个求本次考试成绩最高分的学生的友元函数 student* average(student *p,int count)

以上类名和友元函数的形式，均须按照题目要求，不得修改。

### 裁判测试程序样例

```
输入格式:
输入是 学号（[00001,99999]）和成绩，以0结束。（不超过100个学生）

输出格式:
输出是最高分学生的 学号 。 提示：如果是并列最高分，需要将并列最高分学生的学号都输出，以一个空格间隔。
```

### 输入样例

```
在这里给出一组输入。例如：
10001 90
10002 93
0
```

### 输出样例

```
在这里给出相应的输出。例如：
10002
```

### 代码

```
#include <iostream>
#include <iomanip>
#include <cstring>
using namespace std;

class student{
private:
    int no;
    double score;
public:
    student(){
        score = 0;
        no = 0;
    };
    void setstud(int no1,double score1){
        score = score1;
        no = no1;
    }

    int getno(){ return no; }
    friend student* average(student *p,int count);
};

student* average(student *p,int count){
    int i,j;
    int index[100]={0};
    int num=0;
    for(i=1;i<count;i++){
        if(p[i].score>p[index[0]].score){
            for(j=0;j<num;j++) index[j]=0;
            index[0]=i;
        }
        else if(p[i].score == p[index[0]].score){
            index[++num]=i;
        }
    }

    student *result;

    result = new student[100];
    for(i=0;i<=num;i++){
        result[i]=p[index[i]];
    }

    return result;

}

int main(){
    student stu[100];
    int no1;
    int i;
    int count=0;
    double score1;

    for(i=0;i<100;i++){
        cin>>no1;
        if(no1==0) break;
        cin>>score1;
        stu[i].setstud(no1,score1);
        count++;
    }

    student *result;
    result = average(stu,count);

    for(i=0;i<100;i++){
        if(result[i].getno()==0) break;
        if(i==0) cout<<setfill('0')<<setw(5)<<result[i].getno();
        else cout<<" "<<setfill('0')<<setw(5)<<result[i].getno();
    }
    cout<<endl;
}
```

## 求两点之间距离

### 题目概述

定义一个Point类，有两个数据成员：x和y, 分别代表x坐标和y坐标，并有若干成员函数。 定义一个函数Distance(), 用于求两点之间的距离。

### 裁判测试程序样例

```
输入格式:
输入有两行： 第一行是第一个点的x坐标和y坐标； 第二行是第二个点的x坐标和y坐标。

输出格式:
输出两个点之间的距离，保留两位小数。
```

### 输入样例

```
0 9 3 -4
```

### 输出样例

```
13.34
```

### 代码

```
#include <iostream>
#include <cmath>
#include <iomanip>
using namespace std;

class Point{
private:
    double x;
    double y;
public:
    Point(int x1,int y1):x(x1),y(y1){}
    double getX(){ return x; }
    double getY(){ return y; }
};

double Distance(Point m,Point n){
    return pow((pow(m.getX()-n.getX(),2))+(pow(m.getY()-n.getY(),2)),0.5);
}

int main(){
    int a, b, c, d;
    cin>>a>>b>>c>>d;
    Point p1(a,b);
    Point p2(c,d);
    cout<<fixed<<setprecision(2)<<Distance(p1,p2)<<endl;
}
```

## 友元函数的练习

### 题目概述

定义Boat与Car两个类，两者都有私有的整型weight属性，定义两者的一个友元函数getTotalWeight()，计算二者的重量和。

输入格式:
请在这里写输入格式。例如：输入在一行中给出2个整数m和n。

输出格式:
请在这里描述输出格式。例如：对每一组输入，在一行中输出:船和汽车共重M+n吨值。

### 裁判测试程序样例

```
int main() {
    int n,m; 
    cin>>n>>m; 
    Boat boat(n); 
    Car car(m); 
    cout<<"船和汽车共重"<<getTotalWeight(boat,car)<<"吨"<<endl; 
}
```

### 输入样例

```
40 30
```

### 输出样例

```
船和汽车共重70吨
```

### 代码

```
#include <iostream>
using namespace std;

class Car;

class Boat{
private:
    int weight;
public:
    Boat(int w):weight(w){};
    friend int getTotalWeight(Boat &p,Car &q);
};

class Car{
private:
    int weight;
public:
    Car(int w):weight(w){};
    friend int getTotalWeight(Boat &p,Car &q);
};

int getTotalWeight(Boat &p,Car &q){
    return p.weight+q.weight;
}

int main() {
    int n,m;
    cin>>n>>m;
    Boat boat(n);
    Car car(m);
    cout<<"船和汽车共重"<<getTotalWeight(boat,car)<<"吨"<<endl;
}
```

## 复数类的操作

### 题目概述

1、声明一个复数类Complex（类私有数据成员为double型的real和image）

2、定义构造函数，用于指定复数的实部与虚部。

3、定义取反成员函数，调用时能返回该复数的相反数（实部、虚部分别是原数的相反数）。

4、定义成员函数Print()，调用该函数时，以格式(real, image)输出当前对象。

5、编写加法友元函数，以复数对象c1，c2为参数，求两个复数对象相加之和。

6、主程序实现：

（1）读入两个实数，用于初始化对象c1。

（2）读入两个实数，用于初始化对象c2。

（3）计算c1与c2相加结果，并输出。

（4）计算c2的相反数与c1相加结果，并输出。

### 裁判测试程序样例

```
输入格式:
输入有两行：

第一行是复数c1的实部与虚部，以空格分隔；

第二行是复数c2的实部与虚部，以空格分隔。

输出格式:
输出共三行:

第一行是c1与c2之和；

第二行是c2的相反数与c1之和；

第三行是c2 。


```

### 输入样例

```
2.5 3.7
4.2 6.5
```

### 输出样例

```
(6.7, 10.2)
(-1.7, -2.8)
(4.2, 6.5)
```

### 代码

```
#include <iostream>
using namespace std;

class Complex{
private:
    double real;
    double image;
public:
    Complex(double r=0,double i=0):real(r),image(i){};
    void Print(){
        cout<<"("<<real<<", "<<image<<")"<<endl;
    }
    Complex opp(){
        Complex r;
        r.real=-real;
        r.image=-image;
        return r;
    }
    friend Complex add(Complex &p,Complex &q);
};

Complex add(Complex &p,Complex &q){
    Complex r;
    r.real=p.real+q.real;
    r.image=p.image+q.image;
    return r;
}

int main(){
    double real1,image1;
    double real2,image2;
    cin>>real1>>image1>>real2>>image2;
    Complex c1(real1,image1),c2(real2,image2);
    Complex sumc=add(c1,c2);
    Complex co2=c2.opp();
    Complex sumoc=add(c1,co2);
    sumc.Print();
    sumoc.Print();
    c2.Print();
}
```

## 计算年龄问题

### 题目概述

定义一个Birthday类，其成员变量有3个整形变量（出生的年月日）：year,month,day；提供构造方法对这3个成员变量进行初始化；提供成员变量的get、set方法；成员函数有getAge(),功能是实现计算到2017年12月25日时该Birthday对象的年龄。编写程序测试这个类。

### 裁判测试程序样例

```
输入格式:
输入出生的年、月、日（注：输入的年月日以换行隔开）

输出格式:
计算得到年龄
```

### 输入样例

```
1995
12
23
```

### 输出样例

```
22
```

### 代码

```
#include <iostream>
using namespace std;

class Birthday{
private:
    int year;
    int month;
    int day;
public:
    void setAge(int y, int m, int d){
        year=y;
        month=m;
        day=d;
    }
    int getAge(){
        if(month==12&&day>25) return 2017-year-1;
        else return 2017-year;
    }
};

int main(){
    int y,m,d;
    cin>>y>>m>>d;
    Birthday obj;
    obj.setAge(y,m,d);
    cout<<obj.getAge()<<endl;
}
```

## 工作备忘录的生成（链表）

### 题目概述

每天都要处理很多事务，为了更好地安排工作，希望在每天开始工作前，根据工作记录，生成工作备忘录。首先输入工作记录数（大于0的一个整数），再逐条输入各条工作记录，每条工作记录包括：工作名，开始时间，结束时间。假设每项工作的开始时间均小于它的结束时间，并且各项工作的开始时间互不相同。

我们的工作是需要把这些工作记录按开始时间排序并输出，在输出时，如果某项工作与若干项工作冲突（在做该项工作时，需要同时做其它工作），则在该工作名前加'*'。

### 函数接口定义

```
Node* add(Node *, Node *);
void display(Node *);
```

### 裁判测试程序样例

```
#include<iostream>
#include <string>
using namespace std;
struct Node{
    string name;
    int start;
    int end;
    Node *next;
};
Node* add(Node *, Node *);
void display(Node *);
bool check(Node *head)
{
    if(head==NULL || head->next==NULL) return true;
    Node *p=head->next;
    if(head->start > p->start) return false;
    return check(p);
}
int main()
{
    Node *head=NULL, *p;
    int i, repeat;
    cin>>repeat;
    for(i=0;i<repeat;i++){
        p = new Node;
        cin>>p->name>>p->start>>p->end;
        p->next=NULL;
        head = add(head, p);
    }
    if(!check(head)) cout<<"ERROR"<<endl;
    display(head);
    return 0;
}

/* 请在这里填写答案 */
```

### 输入样例

```
4
aaa 19 20
ccc 169 200
ddd 153 170
bbb 20 111
```

### 输出样例

```
aaa 19 20
bbb 20 111
*ddd 153 170
*ccc 169 200
```

### 代码

```
Node* add(Node *p, Node *q){
    Node *s1=p,*s2;
    if(s1==NULL){
        p=q;
        p->next=NULL;
        return p;
    }

    if(s1->start>q->start){
        q->next=s1;
        p=q;
        return p;
    }

    while(s1->next!=NULL){
        s2 = s1->next;
        if (s1->start < q->start && s2->start > q->start) break;
        s1 = s1->next;
    }

    if(s1->next==NULL){
        s1->next=q;
        q->next=NULL;
    }
    else{
        s1->next=q;
        q->next=s2;
    }

    return p;
}

void display(Node *p){
    Node *s1=p,*s2;
    while(s1!=NULL){
        s2 = s1->next;
        while(s2!=NULL) {
            if (s1->end > s2->start) {
                if (s1->name[0] != '*'){
                    s1->name.insert(0, 1, '*');
                }
                if(s2->name[0]!='*'){
                    s2->name.insert(0, 1, '*');
                }
            }
            s2 = s2->next;
        }
        s1=s1->next;
    }

    while(p!=NULL) {
        cout << p->name << " " << p->start << " " << p->end << endl;
        p=p->next;
    }
}Node* add(Node *p, Node *q){
    Node *s1=p,*s2;
    if(s1==NULL){
        p=q;
        p->next=NULL;
        return p;
    }

    if(s1->start>q->start){
        q->next=s1;
        p=q;
        return p;
    }

    while(s1->next!=NULL){
        s2 = s1->next;
        if (s1->start < q->start && s2->start > q->start) break;
        s1 = s1->next;
    }

    if(s1->next==NULL){
        s1->next=q;
        q->next=NULL;
    }
    else{
        s1->next=q;
        q->next=s2;
    }

    return p;
}

void display(Node *p){
    Node *s1=p,*s2;
    while(s1!=NULL){
        s2 = s1->next;
        while(s2!=NULL) {
            if (s1->end > s2->start) {
                if (s1->name[0] != '*'){
                    s1->name.insert(0, 1, '*');
                }
                if(s2->name[0]!='*'){
                    s2->name.insert(0, 1, '*');
                }
            }
            s2 = s2->next;
        }
        s1=s1->next;
    }

    while(p!=NULL) {
        cout << p->name << " " << p->start << " " << p->end << endl;
        p=p->next;
    }
}
```

## 单链表（流浪狗收养所）

### 题目概述

怡山小学生物组的同学在课外要收养一批流浪狗。在流浪狗进入收养基地时，课外指导老师会给每一只狗取一个唯一的编号，并且判定它的年龄，让组长输入流浪狗档案。档案以单链表存储，按年龄为序（从小到大），如果年龄相同，则后录入的记录应该放在前面。

由于组里新来了一位二年级的淘气组员小林，喜欢将已经输入的档案再次输入。他在输入时，会输入正确的编号，却可能输入错误的年龄（但他所输入的年龄不会大于真实的年龄），因此在输入档案外，组长还需要对档案进行清理，清除那些重复档案。

输入时：首先输入任务类型：1代表插入新的记录, 2代表清理档案, 0代表退出。如果需要加入新的记录，则接下来将依次输入编号和年龄。

输出时：按照从前到后的顺序输出档案中的所有的记录，每条记录单独占一行。

### 函数接口定义

```
Dog *insert(string &no, int age);　　//加入一只狗的信息
Dog *clear();  //清理档案
```

### 裁判测试程序样例

```
#include <iostream>
#include <string>
using namespace std;
struct Dog{
  string no;
  int age;
  Dog *next;    
};
Dog *head=NULL;
void *del(Dog *p){
  if(p!=NULL)   {
    del(p->next);
    delete p;
  }
}
void display(Dog *p){
    if(p!=NULL){
        cout<<p->no<<' '<<p->age<<endl;
        display(p->next);
    }
}

Dog *clear();
Dog *insert(string &no, int age);

int main()
{

    int task, age;
    string no;
    cin>>task;
    while(task>0){
        switch(task){
            case 1:cin>>no>>age; head=insert(no, age); display(head); break;
            case 2:head=clear(); display(head); break;
        }
        cin>>task;
    }
    del(head);
    return 0;   
}

/* 请在这里填写答案 */
```

### 输入样例

```
1 abcd 2
1 cd 5
1 cd 4
1 zz 4
2
0
```

### 输出样例

```
abcd 2
abcd 2
cd 5
abcd 2
cd 4
cd 5
abcd 2
zz 4
cd 4
cd 5
abcd 2
zz 4
cd 5
```

### 代码

```
Dog *clear(){
    if(head == NULL) return NULL;
    if(head->next == NULL) return head;

    Dog *prv1, *now1;
    Dog *now2;
    prv1 = head;
    now1 = head;
    now2 = now1->next;

    while(now1!=NULL) {
        while (now2 != NULL) {
            if (now2->no == now1->no) {
                if (prv1 == head && now1 == head) {
                    head = head->next;
                    prv1 = head;
                    now1 = head;
                    now2 = now1->next;
                    continue;
                } else {
                    prv1->next = now1->next;
                    now1 = now1->next;
                    now2 = now1->next;
                    continue;
                }
            }
            now2 = now2->next;
        }
        prv1 = now1;
        now1 = now1->next;
        if(now1 == NULL) break;
        else if(now1->next == NULL) break;
        else now2 = now1->next;
    }
    return head;
}


Dog *insert(string &no, int age){
    Dog *nownode=head,*nextnode;
    Dog *insertnode;
    insertnode=(Dog *)malloc(sizeof(Dog));
    insertnode->age=age;
    insertnode->no=no;
    if(head == NULL){
        insertnode->next=NULL;
        head = insertnode;
        return head;
    }

    if(head->age>age){
        insertnode->next=head;
        head=insertnode;
        return head;
    }

    while(nownode->next!=NULL){
        nextnode = nownode->next;
        if(nownode->age<age && nextnode->age>=age) break;
        nownode = nownode->next;
    }

    if(nownode->next==NULL){
        nownode->next=insertnode;
        insertnode->next=NULL;
        return head;
    }
    else{
        nownode->next=insertnode;
        insertnode->next=nextnode;
        return head;
    }
}
```

## 函数指针（理科实验班）

### 题目概述

梦山高中现需要将某普通班的最优秀学生调整入理科实验班。为此，将从两个方面考察学生，一是数学和英语两门课的总分；另一个是所有四门课的总分。分别找出两科总分和全科总分的第一名，并从中决定调整人选。

输入将首先输入学生数n, (n为不超过80的正整数);接下来依次输入各位学生的学号，数学、英语、语文、理科综合成绩。学号及四科成绩均为不超过３００的正整数。

输出时：第一行输出两科总分第一的学号，第二行输出四科总分第一的学号。　约定在两位学生成绩相同时，优先选择学号较小的学生；各位学生的学号均不相同。

### 裁判测试程序样例

```
#include <iostream>
using namespace std;
const int N=80;
struct Student{
  int num;
  int score[4];
};

/* 请在这里填写答案 */

int main()
{
  int i, j, k, n;
  bool s2(const Student &, const Student &);
  bool s4(const Student &, const Student &);
  Student st[N];
   cin>>n;
   for(i=0;i<n;i++){
      cin>>st[i].num;
      for(j=0;j<4;j++) cin>>st[i].score[j];
    }
   cout<<select(st, n, s2)<<endl;
   cout<<select(st, n, s4)<<endl;
}

#include <iostream>
using namespace std;
const int N=80;
struct Student{
  int num;
  int score[4];
};

/* 请在这里填写答案 */

int main()
{
  int i, j, k, n;
  bool s2(const Student &, const Student &);
  bool s4(const Student &, const Student &);
  Student st[N];
   cin>>n;
   for(i=0;i<n;i++){
      cin>>st[i].num;
      for(j=0;j<4;j++) cin>>st[i].score[j];
    }
   cout<<select(st, n, s2)<<endl;
   cout<<select(st, n, s4)<<endl;
}
```

### 输入样例

```
3
6 148 150 120 252
5 148 150 117 260
7 145 148 128 287
```

### 输出样例

```
5
7
```

### 代码

```
bool s2(const Student &a, const Student &b){
    if(a.score[0]+a.score[1]>b.score[0]+b.score[1])
        return true;
    else
        return false;
}

bool s4(const Student &a, const Student &b){
    if(a.score[0]+a.score[1]+a.score[2]+a.score[3]>b.score[0]+b.score[1]+b.score[2]+b.score[3])
        return true;
    else
        return false;
}

int select(Student *st, int n, bool (*s)(const Student &,const Student &)){
    int index=0;
    for(int i=1;i<n;i++)
    {
        if(s(st[index],st[i]))
            continue;
        else
            index=i;
    }
    return st[index].num;
}
```

## 二维数组（海绵城市）

### 题目概述

根据海绵城市建设指挥部要求，怡山小学将对校内道路进行改造，铺设透水砖。这样有些道路将不能通行。为了不妨碍假期少先队的校内活动安排，大队宣传委员小黄需要知道一些关键的活动地点是否可以到达。

已知校内一共有20处建筑，分别标为1号楼，2号楼，...，20号楼。有些楼之间有道路连接，道路是双向的，如果Ａ楼与Ｂ楼间有道路，那么既可以从Ａ楼到Ｂ楼，也可以从Ｂ楼到Ａ楼。

首先将输入校内的道路数n, 接下来分n行输入各条道路的信息，每行有两个整数（均在1和20之间），代表这两座楼之间有道路连接。　接下来输入查询数m, 然后分m行输入要查询的楼间连路信息，每行有两个整数（均在1和20之间）。

如果两楼之间可以通过一条路径到达（中途有可能经过其它楼），则输出两楼是连接的，否则输出两楼是断开的。

### 函数接口定义

```
完成查询两建筑是否连通的函数test
```

### 裁判测试程序样例

```
#include <iostream>
using namespace std;
const int N=21;

/* 请在这里填写答案 */

int main()
{
  int a[N][N]={0}, n, m, i, j, k;
  cin>>n;
  for(i=0;i<n;i++){
    cin>>j>>k;
    a[j][k]=a[k][j]=1;
  }
  cin>>m;
  for(i=0;i<m;i++){
    cin>>j>>k;
    cout<<j<<'-'<<k<<' ';
    if(test(a, j, k)) cout<<"connected"<<endl; else cout<<"disconnected"<<endl;
  } 
  return 0;
}
```

### 输入样例

```
2
1 2
2 3
2
1 3
1 4
```

### 输出样例

```
1-3 connected
1-4 disconnected
```

### 代码

```
bool test(int a[N][N],int j,int k){
    int stack[N+1] = {0};
    bool visited[N+1] = {false};
    int top = 0;
    stack[++top] = j;
    visited[j] = true;
    while(top>0){
        int cur = stack[top--];
        for(int i=0;i<N;i++){
            if(a[cur][i]==1 && visited[i]==false){
                stack[++top] = i;
                if(i==k) return true;
                visited[i] = true;
            }
        }
    }
    return false;
}
```

## 2017Final 乐观的中考生

### 题目概述

作为参加首届全省统一中考的学生，小林同学的压力非常大。每天老师都会布置很多批作业，每批作业具有相同的优先值（不同批的任务的优先值不一样），由若干个不同的任务组成，每个任务都有唯一的编号，小林同学会把每批作业先按照编号顺序整理好，然后按照这批作业的优先值，放入活页夹里。接下来，她将按照任务在活页夹的顺序依次把作业完成。

但近期情况有改变了：妈妈给她生了一个弟弟，所以她要时不时帮妈妈照看一下他。令她欣慰的是：可爱的弟弟非常乖，每次吵闹的时间和程度都是可以预期的。因此她可以确定当前能用于完成作业的最长时间，每项任务的可持续时间（最短时间和最长时间，其中设置最短时间是因为连续的学习时间非常宝贵，所以在预期弟弟会安静较长时间时，她要把时间放在那些工作量较大的任务上），然后按符合条件的任务在活页夹中的顺序来完成。

林同学非常乐观，当她整理完一批作业，或者结束一次工作时，都会找出该批作业的第三个任务或者活页夹中的第三个任务（因为3是她的幸运数字）看一下，如果任务数少于3个，她会愉快地说一声"OK"。

现在要输出林同学看到的任务和她说的"OK"。

输入时，每次输入一行：

（1）如果该行的第一个数字是正数N时，说明新来了一批任务，这批任务由N个任务构成。第二个数字是该批任务的优先值，为不超过10000的正整数，从第三个数字开始的2*N个数字是顺序的N个任务的信息，前面一个是任务号（不超过10000的正整数），后一个是任务的持续时间（不超过200的正整数）。（每批任务在输入时，已经按照编号排好序了，因此不需要考虑按顺序号的从小到大排序过程，只好按任务来到的顺序简单整理好，放进一个小链表就可以了。）。然后再将这批任务根据优先值插入活页夹（大链表）。

（2）如果该行的第一个数字是负数，则说明现在可以做作业了，接下来会输入三个数字，第一个数字是单项任务的最短持续时间mi，第二个数字是单项任务的最长持续时间ma,最后一个数字是本次工作的最长持续时间load.

（3）如果该行的第一个数字是0，则结束。

### 函数接口定义

```
Task* getBatch(int m);   //接收一批任务，该批任务共有m项任务
Task* addBatch(Task *head, Task *h);//将h指向的小链表插入head指向的大链表，并返回新的大链表首指针。
void display(Task *head, int m);//输出第m项任务的信息，或者输出OK
Task* study(Task*head, int mi, int ma, int load);
//mi是本次学习过程中，所学习的各个单项任务的最短持续时间约束，ma是单项任务的最长持续时间约束, load是本次学习过程的最长持续时间
```

### 裁判测试程序样例

```
#include <iostream>
using namespace std;
struct Task{
    int ID;//任务编号
    int key;//任务优先级
    int load;//任务持续时间
    Task *next;
};

/* 请在这里填写答案 */

void del(Task* head){//删除链表
    if(head==NULL) return;
    del(head->next);
    delete head;
}
const int K=3;//幸运数字
int main(){
    int count, mi, ma, load;
    Task *head=NULL, *h;
    cin>>count;//输入行的第一个数字
    while(count!=0){
        if(count>0){   //新添加count个任务
            h=getBatch(count);//构建链表，并将首指针返回给h
            display(h, K);//显示小链表的第K个任务或输出OK
            head=addBatch(head, h);//将小链表加入大链表，并将首指针返回
        }else{
            cin>>mi>>ma>>load;
            head=study(head, mi, ma, load);
            display(head,K);//显示大链表的第K个任务或输出OK
        }
        cin>>count;     
    }
    del(head);
    return 0;
}
```

### 输入样例

```
4 12 101 5 102 3 103 6 104 7
2 22 105 12 106 8
1 9 107 9
-1 5 10 25
0
```

### 输出样例

输出时先输出任务号，再输出任务的优先级，最后输出时间。如果没有可输出的任务，则输出“OK”。

```
103 12 6
OK
OK
105 22 12
```

说明：

（1）输入：4 12 101 5 102 3 103 6 104 7后，

子链（即小链表)共有四项任务101,102,103和104，其中第三项任务是103, 所以输出它的编号，优先值和时间103 12 6

将子链加入活页夹（大链表）后，活页夹中也是四项任务

（2）输入：2 22 105 12 106 8后

子链中共有两项任务105和106，找不到第三项，所以输出OK。

加入活页夹中，此时活页夹中共有六项任务：101,102,103,104,105,106

（3）输入：1 9 107 9后

子链中共有一项任务，所以输出OK。

加入活页夹中，此时活页夹中共有七项任务，顺序为：107,101,102,103,104,105,106

(4)-1 5 12 25

则顺序完成活页夹中时间在5和12之间的任务，最多持续时间为20

第一项任务107，持续时间为9，符合要求，完成该项任务后，剩余时间为16

第二项任务101, 持续时间为5, 符合要求，完成该项任务后，剩余时间为11 第三项任务102，持续时间为3, 不符合最低持续时间为5的要求，直接忽略。

第四项任务103,持续时间为6，符合要求，完成该项任务后，剩余时间为5。

接下来的任务都没有足够时间完成，所以此时活页夹中剩余四项任务，为：102,104,105,106

此时输出活页夹的第三项任务信息：105 22 12

(5)输入0，结束

### 代码

```
Task* getBatch(int m){  //接收一批任务，该批任务共有m项任务
    int key;
    int ID,load;
    cin>>key;
    Task *head = new Task;
    head->key = key;
    cin>>ID>>load;
    head->ID = ID;
    head->load = load;
    head->next = NULL;
    Task *tail = head;
    for(int i=0;i<m-1;i++){
        Task *node = new Task;
        node->key = key;
        cin>>ID>>load;
        node->ID = ID;
        node->load = load;
        node->next = NULL;
        tail->next = node;
        tail = node;
    }

    return head;
}
Task* addBatch(Task *head, Task *h){    //将h指向的小链表插入head指向的大链表，并返回新的大链表首指针。
    if(head == NULL) return h;


    if(head->key>h->key){
        Task *node = h;
        while(node->next!=NULL) node = node->next;
        node->next = head;
        return h;
    }

    Task *node = head;
    while(node->next!=NULL && node->key<=h->key){
        if(node->next->key>h->key) break;
        node = node->next;
    }

    if(node->next == NULL)
        node->next = h;
    else{
        Task *nex = node->next;
        node->next = h;
        node = h;
        while(node->next!=NULL) node = node->next;
        node->next = nex;
    }
    return head;

}

void display(Task *head, int m){    //输出第m项任务的信息，或者输出OK
    if(head==NULL){
        cout<<"OK"<<endl;
        return;
    }
    int i;
    Task *node = head;
    for(i=0;i<m-1;i++){
        if(node->next==NULL) break;
        node = node->next;
    }
    if(i<m-1) cout<<"OK"<<endl;
    else cout<<node->ID<<" "<<node->key<<" "<<node->load<<endl;
}

Task* study(Task*head, int mi, int ma, int load){   //mi是本次学习过程中，所学习的各个单项任务的最短持续时间约束，ma是单项任务的最长持续时间约束, load是本次学习过程的最长持续时间
    if(head == NULL) return NULL;
    if(head->next == NULL){
        if(head->load>=mi && head->load<=ma && head->load<=load) return NULL;
        else return head;
    }

    Task *prv,*cur,*nex;
    prv = head;
    cur = head;
    nex = head->next;
    int time = 0;
    while(nex!=NULL){
        if(cur->load>=mi && cur->load<= ma){
            if(time+cur->load>load) return head;
            else time += cur->load;
            if(prv == head && cur == head){
                nex = nex->next;
                head = head->next;
                prv = head;
                cur = head;
                continue;
            }
            else{
                prv->next = nex;
                cur = cur->next;
                nex = nex->next;
                continue;
            }
        }
        if(prv == head && cur == head){
            nex = nex->next;
            cur = cur->next;
        }
        else{
            nex = nex->next;
            cur = cur->next;
            prv = prv->next;
        }
    }
    if(cur->load>=mi && cur->load<=ma && time+cur->load<=load){
        if(prv == head && cur == head) return NULL;
        else prv->next=NULL;
    }
    return head;
}
```

## 时间换算

### 题目概述

输入一个正整数 repeat `(0<repeat<10)`，做 repeat 次下列运算：

输入一个时间数值，再输入秒数 n，输出该时间再过 n 秒后的时间值，时间的表示形式为时:分:秒，超过 24 时从 0 时重新开始计时。

### 裁判测试程序样例

```
输出格式： printf("time: %d:%d:%d\n", );

输入输出示例：括号内为说明，无需输入输出
```

### 输入样例

```
3      (repeat=3)
0:0:1
59     (秒数n=59)
11:59:40   
30     (秒数n=30)
23:59:40   
301    (秒数n=301)
```

### 输出样例

```
time: 0:1:0    (0:0:01加上59秒的新时间)   
time: 12:0:10      (11:59:40加上30秒的新时间)
time: 0:4:41       (23:59:40加上301秒的新时间)
```

### 代码

```
#include <iostream>
using namespace std;

class Time{
    int hour;
    int minute;
    int second;
public:
    Time(int hour1,int minute1,int second1,int sn1){
        hour=hour1;
        minute=minute1;
        second=second1+sn1;
        minute=minute+second/60;
        second=second%60;
        hour=hour+minute/60;
        minute=minute%60;
        hour=hour%24;
    }
    void display(){
        cout<<"time: "<<hour<<":"<<minute<<":"<<second<<endl;
    }
};

int main(){
    int i,N;
    int a,b,c,sn;
    Time *obj[10];
    cin>>N;
    for(i=0;i<N;i++){
        cin>>a;
        cin.get();
        cin>>b;
        cin.get();
        cin>>c>>sn;
        obj[i]=new Time(a,b,c,sn);
    }
    for(i=0;i<N;i++){
        obj[i]->display();
    }
}
```

## 查找单价最高和最低的书籍

### 题目概述

编写程序，从键盘输入 n (n<10)本书的名称和定价并存入结构数组中，查找并输出其中定价最高和最低的书的名称和定价。

### 裁判测试程序样例

```
输出格式语句：

printf("highest price: %.1f, %s\n", );

printf("lowest price: %.1f, %s\n",);

输入输出示例：括号内为说明，无需输入输出
```

### 输入样例

```
3   (n=3)
Programming in C
21.5
Programming in VB
18.5
Programming in Delphi
25
```

### 输出样例

```
highest price: 25.0, Programming in Delphi 
lowest price: 18.5, Programming in VB 
```

### 代码

```
#include <stdio.h>
#include <string.h>

int main(){
    int i, n;
    char title[10][100];
    double price[10];
    scanf("%d",&n);
    getchar();
    for(i=0;i<n;i++){
        gets(title[i]);
        scanf("%lf",&price[i]);
        getchar();
    };

    int j;
    int highest=0, lowest=0;
    for(j=1;j<n;j++){
            if(price[j]>price[highest]&&price[j]>0) highest=j;
            if(price[j]<price[lowest]&&price[j]>0) lowest=j;
    };
    printf("highest price: %.1f, %s\n", price[highest], title[highest]);
    printf("lowest price: %.1f, %s\n", price[lowest], title[lowest]);
}
```

## 书籍排序

### 题目概述

编写程序，从键盘输入 n (n<10)本书的名称和定价并存入结构数组中，按单价从小到大排序并输出排序后的书籍信息。

### 输入样例

```
3   (n=3)
Programming in C
21.5
Programming in VB
18.5
Programming in Delphi
20
```

### 输出样例

```
Programming in VB 18.5
Programming in Delphi 20.0
Programming in C 21.5
```

### 代码

```
#include <stdio.h>
#include <string.h>

int main(){
    int i, n;
    char title[10][100];
    double price[10];
    scanf("%d",&n);
    getchar();
    for(i=0;i<n;i++){
        gets(title[i]);
        scanf("%lf",&price[i]);
        getchar();
    };

    int j,k;
    double tprice;
    char ttitle[100];
    for(j=0;j<n;j++){
        for(k=j+1;k<n;k++){
            if(price[j]>price[k]){
                tprice=price[j];
                price[j]=price[k];
                price[k]=tprice;

                strcpy(ttitle,title[j]);
                strcpy(title[j],title[k]);
                strcpy(title[k],ttitle);
            }
        }
    };

    for(i=0;i<n;i++)
        printf("%s %.1f\n",title[i],price[i]);

}
```

## 输入学生信息

### 题目概述

编写程序，从键盘输入 n (n<10)个学生的学号（学号为4位的整数，从1000开始）、成绩并存入结构数组中，输出学生信息。

### 输入样例

```
3   (n=3)
1000 85
1001 90
1002 75
```

### 输出样例

```
1000 85
1001 90
1002 75
```

### 代码

```
#include<iostream>
using namespace std;

struct Student{
    int no;
    int score;
};

int main(){
    int i,n;
    int no,score;
    cin>>n;
    Student stu[3];
    for(i=0;i<n;i++){
        cin>>no>>score;
        stu[i].no=no;
        stu[i].score=score;
    }
    for(i=0;i<n;i++){
        cout<<stu[i].no<<" "<<stu[i].score<<endl;
    }
    return 0;
}
```

## 查找字符串

### 题目概述

编程实现： 输入一个正整数repeat `(0<repeat<10)`，做repeat次下列运算：

输入一个字符，再输入一个以回车结束的字符串（少于80个字符），在字符串中查找该字符，如果找到，输出该字符在字符串中所对应的最大下标 (下标从0开始)；否则输出"Not Found"。输出格式为"index = %d\n"

### 输入样例

```
2           (repeat=2)
m           (字符'm')
programming (字符串"programming")
a           (字符'a')
1234        (字符串"1234")
```

### 输出样例

```
index = 7       ('m'在"programming"中对应的最大下标是7)
Not Found       ("1234"中没有'a')
```

### 代码

```
#include <iostream>
#include <cstring>
using namespace std;

int main(){
    int ri, repeat;
    char ch;
    char str[80];
    int i, index;
    scanf("%d",&repeat);
    for(ri=0;ri<repeat;ri++){
        i=0;
        index=-1;
        getchar();
        scanf("%c",&ch);
        scanf("%s",&str);
        while(str[i]!='\0'){
            if(str[i]==ch) index=i;
            i++;
        }
        if(index==-1) printf("Not Found\n");
        else printf("index = %d\n",index);
    }
}
```

## 练习题

### 题目概述

输入若干行，每行输入两个绝对值不大于100的整数，求这两个整数的和、差、乘积中不超过100的最大数。

### 输入样例

```
3 9
-5 15
22 7
```

### 输出样例

```
27
20
29
```

### 代码

```
#include <iostream>
using namespace std;

int main(){
    int a,b;
    int result[3];
    int maxnum;
    int i;
    int ri;

    while(scanf("%d%d",&a,&b)!=EOF){
        result[0]=a+b;
        result[1]=(a-b)>(b-a)?(a-b):(b-a);
        result[2]=a*b;
        maxnum=1000;
        i=0;
        while(maxnum>100&&i<3){
            maxnum=result[i++];
        };

        for(ri=i;ri<3;ri++){
            if(result[ri]>maxnum && result[ri]<=100)
                maxnum=result[ri];
        }
        cout<<maxnum<<endl;
    }
}
```

## 方阵的转置

### 题目概述

编写一个类用于处理3×3矩阵转置，测试转置的效果，输出转置前后的矩阵。

根据要求写出类，并可以使得主函数正确运行，得到对应的结果。

### 裁判测试程序样例

```
int main(){
    Matrix m;
    m.input();
    m.show();
    m.transform();
    m.show();
}
```

### 输入样例

```
1 2 3
4 5 6
7 8 9
```

### 输出样例

```
datas:
 1 2 3
 4 5 6
 7 8 9
datas:
 1 4 7
 2 5 8
 3 6 9
```

### 代码

```
#include <iostream>
using namespace std;
class Matrix{
private:
    double matr[3][3]={0,0,0,0,0,0,0,0,0};
public:
    void input(){
        for(int i=0;i<3;i++)
            for(int j=0;j<3;j++)
                cin>>matr[i][j];
    };
    void show(){
        cout<<"datas:\n";
        for(int i=0;i<3;i++) {
            for (int j = 0; j < 3; j++)
                cout << " " << matr[i][j];
            cout<<"\n";
        }
    };
    void transform(){
        double temp[3][3]={0,0,0,0,0,0,0,0,0};;
        for(int i=0;i<3;i++)
            for(int j=0;j<3;j++)
                temp[i][j]=matr[j][i];
        for(int i=0;i<3;i++)
            for(int j=0;j<3;j++)
                matr[i][j]=temp[i][j];
    }
};
```

## 二维向量相加（C++ 运算符重载）

### 题目概述

裁判测试程序样例中展示的是一段二维向量类TDVector的定义以及二维向量求和的代码，其中缺失了部分代码，请补充完整，以保证测试程序正常运行。

### 函数接口定义

```
提示：需要补充的函数有：
1. 带参构造函数
2. getX
3. getY
4. setX
5. setY
6. 运算符重载函数
```

### 裁判测试程序样例

```
#include <iostream>
#include <iomanip>
using namespace std;
class TDVector{
private:
    double x;
    double y;
public:
    TDVector(){
        x = y = 0;
    }
/**    你提交的代码将被嵌在这里（替换本行内容）  **/
};
int main(){
    TDVector a;
    double x, y;
    cin >> x >> y;
    TDVector b(x, y);
    cin >> x >> y;
    TDVector c;
    c.setX(x);
    c.setY(y);
    TDVector d;
    d = a + b + c;
    cout << fixed << setprecision(2) << d.getX() << ' ' << d.getY();
    return 0;
}
```

### 输入样例

```
1.1 2.2
3.3 4.4
```

### 输出样例

```
4.40 6.60
```

### 代码

```
    TDVector(double x1,double y1):x(x1),y(y1){}

    void setX(double x2){x=x2;}
    void setY(double y2){y=y2;}
    double getX(){return x;}
    double getY(){return y;}

    TDVector operator+(const TDVector& p){
        TDVector td;
        td.x=x+p.x;
        td.y=y+p.y;
        return td;
    }
```

## 自定义的学生类

### 题目概述

本题要求定义一个简单的学生类，数据成员仅需要定义学号和姓名，函数成员的原型见给出的代码，请给出函数成员的类外完整实现。

### 函数接口定义

```
class Student
{
private:
    int m_id;
    char m_name[10];
public:
    Student(int id=0,char *name="");
    ~Student();
    void print();

};
```

其中m_id和m_name分别表示学生的学号和姓名，类型已经定义好。类内声明了3个成员函数，分别表示构造函数、析构函数和用于输出学生信息的函数。 构造函数要完成两个数据成员的初始赋值，并做一行输出，形如：“Hi!学号 姓名”。 析构函数要求输出一行，形如：“Bye!学号 姓名”。 print函数要求在一行中输出学生信息，形如：“学号 姓名”。 注：学号和姓名之间用一个空格分开。

### 裁判测试程序样例

```
#include <iostream>
#include <cstring>
using namespace std;

int main()
{
    Student stu_array[3]={Student(1,"Zhang"),Student(2,"Wang")};
    return 0;
}

/* 请在这里填写答案 */
```

### 代码

```
Student::Student(int id, char *name){
    m_id=id;
    strcpy(m_name,name);
    cout<<"Hi!"<<m_id<<" "<<m_name<<endl;
}

Student::~Student(){
    cout<<"Bye!"<<m_id<<" "<<m_name<<endl;
}

void Student::print() {
    cout<<m_id<<" "<<m_name<<endl;
}
```

## 类的声明和成员函数的实现

### 题目概述

声明了一个Dog类，包含了age，weight等属性，以及对这些属性进行操作的方法。请实现该类的成员函数。

### 裁判测试程序样例

```
#include <iostream>
using namespace std;

class Dog {
public:
    void setAge(int a);
    int getAge();
    void setWeight(int w);
    int getWeight();
private:
    int age, weight;
};

int main()
{
    Dog d;
    int a, w;
    cin >> a >> w;
    d.setAge(a);
    d.setWeight(w);
    cout << d.getAge() << endl;
    cout << d.getWeight() << endl;
    return 0;
}

/* 你的代码将被嵌在这里 */
```

### 输入样例

```
1 3
```

### 输出样例

```
1
3
```

### 代码

```
void Dog::setAge(int a){ age=a; }

int Dog::getAge() { return age; }

void Dog::setWeight(int w) { weight=w; }

int Dog::getWeight() { return weight; }
```

## 使用类计算矩形的面积

### 题目概述

定义并实现一个矩形类，有长和宽两个属性，由成员函数计算矩形的面积。

### 裁判测试程序样例

```
#include <iostream>
using namespace std;

class Rectangle {
public:
    void setLength(int l);//设置矩形的长度
    void setWidth(int w); //设置矩形的宽度
    int getArea();        //计算并返回矩形的面积
private:
    int length, width;    //矩形的长度和宽度    
};

int main()
{
    Rectangle r;
    int len, w;
    cin >> len >> w;
    r.setLength(len);
    r.setWidth(w);
    cout << r.getArea() << "\n";

    return 0;
}

/* 你的代码将嵌在这里 */
```

### 输入样例

```
10 20
```

### 输出样例

```
200
```

### 代码

```
void Rectangle::setLength(int l) { length=l; }
void Rectangle::setWidth(int w) { width=w; }
int Rectangle::getArea() { return length*width; }
```

## Tree类的构造函数和成员函数

### 题目概述

定义一个Tree（树）类，有成员ages（树龄），不带参数的构造函数对ages初始化为1，成员函数grow(int years)对ages加上years，age()显示tree对象的ages的值。

### 裁判测试程序样例

```
#include <iostream>
using namespace std;

class Tree {
public:
    Tree();//构造函数
    void grow(int years);//对数龄ages加上years
    void age();//显示数龄ages的值
private:
    int ages;//树龄
};

int main()
{
    Tree tree;
    int years;
    cin >> years;
    tree.grow(years);
    tree.age();

    return 0;
}

/* 你的代码将被嵌在这里 */
```

### 输入样例

```
30
```

### 输出样例

```
31
```

### 代码

```
Tree::Tree():ages(1){}
void Tree::grow(int years) { ages += years; }
void Tree::age() { cout<<ages<<endl; }
```

## Point类的声明和实现

### 题目概述

定义一个Point类，代表平面上的一个点，其横坐标和纵坐标分别用x和y表示，设计Point类的成员函数，实现并测试这个类。 主函数中输入两个点的坐标，计算并输出了两点间的距离。请根据主函数实现Point类。

### 裁判测试程序样例

```
#include <iostream>
#include <iomanip>
#include <cmath>
using namespace std;

//你的代码将被嵌在这里

int main()
{
    Point p1, p2;
    double x1, y1, x2, y2;
    cin >> x1 >> y1 >> x2 >> y2;
    p1.setX(x1);
    p1.setY(y1);
    p2.setX(x2);
    p2.setY(y2);
    double x = p1.getX() - p2.getX();
    double y = p1.getY() - p2.getY();
    double len = sqrt(x * x + y * y);
    cout << fixed << setprecision(2) << len << endl;
    return 0;
}
```

### 输入样例

```
0 0 3 3
```

### 输出样例

```
4.24
```

### 代码

```
class Point{
private:
    double x;
    double y;
public:
    void setX(int x1){ x=x1; }
    void setY(int y1){ y=y1; }
    double getX(){ return x; }
    double getY(){ return y; }
};
```

## 是否是回文

### 题目概述

所谓回文字符串，就是一个字符串，从左到右读和从右到左读完全相同的字符串。比如"123321" 、 "aabaa"都是回文，但"123"和"aabb"就不是回文。规定空串为回文。 Solution类中有一个函数isPalindrome，其功能是判断以'@'结尾的字符串s是否为回文，若是返回true，若不是返回false。 请给出函数bool isPalindrome(char *s)的实现。

### 裁判测试程序样例

```
#include <iostream>
using namespace std;

class Solution
{
public:
    /* 
     * 判断以'@'结尾的字符串s是否为回文。
     * 如果是回文，返回true；不是回文，返回false。
     */
    bool isPalindrome(char *s);
};

int main()
{
    Solution obj;
    char ss[100];
    cin >> ss;
    if (obj.isPalindrome(ss)) {
        cout << "YES\n";
    } else {
        cout << "NO\n";
    }
    return 0;
}

/* 你的代码将被嵌在这里 */
```

### 输入样例

```
aabaa@
aabb@
```

### 输出样例

```
YES
NO
```

### 代码

```
bool Solution::isPalindrome(char *s) {
    int len=0;
    int i,j;
    if(s[0]=='@') return true;
    else{
        while(s[len]!='@') len++;
        for(i=0,j=len-1;j>i;i++,j--){
            if(s[i]!=s[j]) return false;
        }
        return true;
    }
}
```

## 类的声明与成员函数的实现--Car类

### 题目概述

本题要求根据给定的Car类的声明，实现其成员函数。

其中，成员函数void disp_welcomemsg()显示一条欢迎信息“Welcome to the car world!”。 成员函数int get_wheels()返回Car类的私有数据成员m_nWheels。 成员函数int set_wheels(int)用指定的形参初始化数据成员m_nWheels。

### 裁判测试程序样例

```
#include <iostream>
using namespace std;

class Car  //定义类Car
{
    //成员函数
public:
    void disp_welcomemsg(); //显示欢迎信息
    int get_wheels();       //返回汽车的车轮数量
    void set_wheels(int);   //设置汽车的车轮数量
    //数据成员
private:
    int m_nWheels;    //显示汽车的车轮数量
};

/* 请在这里填写答案 */

int main()
{
    int n;
    cin >> n;
    Car mycar;     //定义类对象mycar
    mycar.disp_welcomemsg();  //访问成员函数，显示欢迎信息
    mycar.set_wheels(n);      //访问成员函数，设置车轮数量
    //访问成员函数，显示车轮数量
    cout << "wheels = " << mycar.get_wheels() << endl;
    return 0;
}
```

### 输入样例

```
4
```

### 输出样例

```
Welcome to the car world!
wheels = 4
```

### 代码

```
void Car::disp_welcomemsg() {
    cout<<"Welcome to the car world!"<<endl;
}

int Car::get_wheels() { return m_nWheels; }

void Car::set_wheels(int n) { m_nWheels=n; }
```

## 统计数字

### 题目概述

对于给定的一个字符串，统计其中数字字符出现的次数。

类和### 函数接口定义

```
设计一个类Solution，其中包含一个成员函数count_digits，其功能是统计传入的string类型参数中数字字符的个数并返回。
```

### 裁判测试程序样例

```
#include <cstdlib>
#include <cstdio>
#include <cstring>
#include <cctype>
#include <string>
#include <iostream>
using namespace std;

/* 请在这里填写答案 */

int main()
{
    int t;

    cin >> t;
    getchar();
    while (t--)
    {
        string str;
        Solution obj;

        getline(cin,str);
        int digits = obj.count_digits(str);

        cout << digits << endl;
    }

    return 0;
}
```

### 输入样例

```
2
asdfasdf123123asdfasdf
asdf111111111asdfasdfasdf
```

### 输出样例

```
6
9
```

### 代码

```
class Solution{
public:
    int count_digits(string str){
        int sum=0;
        int i=0;
        while(str[i]!='\0') {
            if (str[i] >= '0' && str[i] <= '9') sum++;
            i++;
        }
    return sum;
    }
};
```

## 实现数组类（C++ 拷贝构造函数、拷贝函数）

### 题目概述

裁判测试程序样例中展示的是一段实现“数组类”的代码，其中缺失了部分代码，请补充完整，以保证测试程序正常运行。

### 函数接口定义

```
提示：要想程序正确运行，至少需要补充以下函数（可能还需要补充其他函数）：
1. 带参构造函数
2. 拷贝构造函数
3. 拷贝函数（赋值运算符重载）
```

### 裁判测试程序样例

```
#include <iostream>
using namespace std;
class ArrayIndexOutOfBoundsException{  // 异常类
public:
    int index;
    ArrayIndexOutOfBoundsException(int k){
        index = k;
    }
};
class Array{
private:
    int *data;
    int size;
    static const int dSize = 10;   // 数组默认大小
public:
    Array( ){  // 无参构造
        size = dSize;
        data = new int[size]( );
    }
        
/** 你提交的代码将被嵌在这里（替换本行内容） **/        
        
    int& operator [] (int k){     // 运算符 [ ] 重载，以方便数组的使用
        if(k<0 || k>=size) throw ArrayIndexOutOfBoundsException(k);
        return data[k];
    }
    friend ostream& operator << (ostream& o, const Array& a);   // 运算符 << 重载，以方便输出
};
ostream& operator << (ostream& o, const Array& a){
    o << '[' ;
    for(int i=0; i<a.size-1; i++)
        o << a.data[i] << ',' ;
    o << a.data[a.size-1] << ']';
    return o;
}
// 注意：实际测试程序中，在此处之前的代码与样例中相同
// 注意：实际测试程序中，在此处之后的代码（即main函数）可能与样例中不同
int main(){
    int n, k;
    cin >> n >> k;
    Array a(n);  // 构造数组，大小为 n
    for(int i=0; i<n; i++) a[i] = i;
    Array b = a;  // 拷贝构造数组
    b[n/2] = k;
    cout << a << endl;
    cout << b << endl;
    Array c;  // 构造数组，默认大小
    c = a; // 拷贝数组
    c[n/2] = k;
    cout << a << endl;
    cout << c << endl;
    a = a;
    a[n/2] = 2223;
    cout << a << endl;
    return 0;
}
```

### 输入样例

```
15 666
```

### 输出样例

```
[0,1,2,3,4,5,6,7,8,9,10,11,12,13,14]
[0,1,2,3,4,5,6,666,8,9,10,11,12,13,14]
[0,1,2,3,4,5,6,7,8,9,10,11,12,13,14]
[0,1,2,3,4,5,6,666,8,9,10,11,12,13,14]
[0,1,2,3,4,5,6,2223,8,9,10,11,12,13,14]
```

### 代码

```
    Array(int n1){  // 带参构造
        size = n1;
        data = new int[size]( );
    }

    Array(const Array& p){  // 拷贝构造函数
        size = p.size;
        data = new int[size];
        for(int i=0;i<size;i++)
            data[i]=p.data[i];
    }

    ~Array(){
        delete[] data;
    }

    Array & operator=(const Array & p) {
        if (p.size != size) {
            delete[] data;
            size = p.size;
            data = new int[size];
        }
        for (int i = 0; i < size; i++) {
            data[i] = p.data[i];
        }
        return *this;
    }
```

## 身份证升位

### 题目概述

试定义一个类ID，将15位的旧版身份证号扩充为18位。在15位的身份证号中，第7、8两位为出生年份，例如：1980年出生的人，身份证号码的第7、8位的值是80，在18位身份证号中，将7-10四位的值改为1980，并将原身份证号码第9位开始以后所有数字依次向右平移2位，在18位身份证号码中，最后增加一位校验码，校验码的计算方法如下（只考虑20世纪出生的公民）：
（1）将已扩展出的17位身份证号按各位上的数字进行加权求和，结果为S。自左到右各位上的数字的权值依次为：{7,9,10,5,8,4,2,1,6,3,7,9,10,5,8,4,2}
（2）将S对11取余运算，结果为Y
（3）根据Y取值的大小顺序{0,1,2,3,4,5,6,7,8,9,10}取对应的校验码{1,0，X，9,8,7,6,5,4,3,2}
例如：当15位身份证号位340524800101001时，扩展成17位：34052419800101001，各位上的数字的加权和S=3*7+4*9+0*10+...=189，S对11取余，得2，则校验码为X。最终升位后的身份证号码为：34052419800101001X

请补充类的定义，实现上述功能。

### 裁判测试程序样例

```
#include<iostream>
#include <string.h>
using namespace std;

/* 请在这里填写答案 */

int main()
{
    char oldID[15];
    cin>>oldID; 
    ID myID(oldID);
    cout<<"ID No.：";
    myID.print();
    myID.convert();
    cout<<"New ID NO.：";
    myID.print();
    return 0;
}
```

### 输入样例

```
150105890811001
```

### 输出样例

```
ID No.：150105890811001
New ID NO.：150105198908110012
```

### 代码

```
class ID{
private:
    char number[19];
public:
    ID(char s[]){ strcpy(number,s); }
    void print(){ cout<<number<<endl; }
    void convert(){
        number[18]='\0';
        int i;
        for(i=16;i>=8;i--)
            number[i]=number[i-2];
        number[6]='1';
        number[7]='9';
        int weight[17]={7,9,10,5,8,4,2,1,6,3,7,9,10,5,8,4,2};
        int S=0;
        for(i=0;i<17;i++)
            S+=weight[i]*(number[i]-'0');
        int Y=S%11;
        char num[11]={'1','0','X','9','8','7','6','5','4','3','2'};
        number[17]=num[Y];
    }
};
```

## 字符平台

### 题目概述

一个字符串中的任意一个子序列，若子序列中个字符值均相等，则称为字符平台。写一算法，输入任意一字符串S，输出S中长度最大的字符平台的起始位置及所含字符。（若有多个最长平台长度相同，则输出第一个）

根据主程序写出相应的类定义。

### 裁判测试程序样例

```
#include <iostream>
#include <string.h>
using namespace std;

/* 请在这里填写答案 */

int main()
{
    char t[30];
    cin>>t;
    Str test(t);
    test.process();
    test.print();
    return 0;
}
```

### 输入样例

```
34dadee888saaaa
```

### 输出样例

```
34dadee888saaaa
aaaa
4
```

### 代码

```
class Str{
private:
    char s[30];
    int start;
    int n=0;
public:
    Str(char t[]){ strcpy(s,t); }
    void process(){
        int i=0, j=0;
        char ch1 = s[0];
        char ch2 = s[1];
        n = 0;
        int tn;
        int tstart;

        while(i<strlen(s)-1){
            while(j<strlen(s)){
                if(ch1 != ch2) break;
                ch2 = s[++j];
            }

            tn = j-i;
            if(tn > n){
                start = i;
                n = tn;
            }
            ch1 = s[++i];
            j = i+1;
            ch2 = s[j];
        }
    }

    void print(){
        cout<<s<<endl;
        for(int i=0;i<n;i++)
            cout<<s[start+i];
        cout<<endl;
        cout<<n<<endl;
    }
};
```

## 定义点类

### 题目概述

根据main（）中的调用形式，定义一个点类（完成数据成员与成员函数的定义），将点的坐标显示在屏幕上。

### 函数接口定义

```
定义点类Point：
class Point 
{
 ... 
};
```

### 裁判测试程序样例

```
在这里给出函数被调用进行测试的例子。例如：
int main()
{
    Point p1,p2;
    p1.setXY(1,2);
    p2.setXY(3,4);
    cout<<"x of Point p1："<<p1.getX()<<endl;
    cout<<"y of Point p2："<<p1.getY()<<endl;
    cout<<"Point P2:("<<p2.getX()<<","<<p2.getY()<<")"<<endl;
    return 0;
}
/* 请在这里填写答案 */
```

### 输出样例

```
x of Point p1：1
y of Point p2：2
Point P2:(3,4)
```

### 代码

```
#include <iostream>
using namespace std;
class Point{
private:
double x;
double y;
public:
    void setXY(double a,double b){
    x=a;
    y=b;
};
    double getX(){
    return x;
};
    double getY(){
        return y;
    }
};
```

## 圆类的定义

### 题目概述

根据main函数的形式，设计一个圆Circle类，能够求出圆的面积（圆周率取值为3.14159）

### 函数接口定义

```
class Circle { };
```

### 裁判测试程序样例

```
#include<iostream>
#include<cmath>
using namespace std;

/* 请在这里填写答案 */

int main()
{
      Circle c;
      double r;
      cin>>(r);
      c.setR(r);
      cout<<"Circle's r is "<<c.getR()<<endl;
      cout<<"Circle's area is "<<c.getArea()<<endl;
      return 0;
}
```

### 输入样例

```
3
```

### 输出样例

```
Circle's r is 3
Circle's area is 28.2743
```

### 代码

```
class Circle{
private:
    double r;
public:
    void setR(double a){
r=a;
};
    double getR(){
        return r;
    };
    double getArea(){
        return 3.1415926*pow(r,2);
    }
};
```

## 体育俱乐部I（构造函数）

### 题目概述

一个俱乐部需要保存它的简要信息，包括四项：名称（字符串），成立年份（整数），教练姓名（字符串）和教练胜率（0－100之间的整数）。用键盘输入这些信息后，把它们分两行输出：第一行输出名称和成立年份，第二行输出教练姓名和胜率。

### 裁判测试程序样例

```
#include <iostream>
#include <string>
using namespace std;
class Coach{
    string name;
    int winRate;
public:
    Coach(string n, int wr){
        name=n; winRate=wr;
    }
    void show();
};
class Club{
    string name;
    Coach c;
    int year;
public:
    Club(string n1, int y, string n2, int wr);
    void show();
};
int main(){
    string n1, n2;
    int year, winRate;
    cin>>n1>>year>>n2>>winRate;
    Club c(n1,year, n2, winRate);
    c.show();
    return 0;
}

/* 请在这里填写答案 */
```

### 输入样例

```
Guanzhou 2006 Tom 92
```

### 输出样例

```
Guanzhou 2006
Tom 92%
```

### 代码

```
void Coach::show() { cout<<name<<" "<<winRate<<"%"<<endl; }

Club::Club(string n1, int y, string n2, int wr):name(n1),year(y),c(n2,wr){}

void Club::show() {
    cout<<name<<" "<<year<<endl;
    c.show();
}
```

## 学生排名表（析构函数）

### 题目概述

现在输入一批学生（人数大于0且不超过100）的名次和他们的姓名。要求按名次输出每个人的排名。

### 函数接口定义

```
输入格式：每行为一个学生的信息，共两项，第一项为排名（为正整数，且任意两名学生的排名均不同），第二项为学生姓名。当输入－1时，表示输入结束。

输出格式：按名次输出学生姓名，每行一个。
```

### 裁判测试程序样例

```
#include <iostream>
#include <string>
using namespace std;
class Student{
    int rank;
    string name;
    public:
        int getRank(){return rank;  }
        Student(string name, int rank):name(name), rank(rank){  }
        ~Student(){ cout<<name<<endl;}
};
int main(){
    int rank, count=0;
    const int SIZE=100;
    Student *pS[SIZE];
    string name;
    cin>>rank;
    while(count<SIZE && rank>0){
        cin>>name;
        pS[count++]= new Student(name, rank);
        cin>>rank;
    }

/* 请在这里填写答案 */

    return 0;
}
```

### 输入样例

```
1 Jack
5 Peter
2 Alice
6 Kate
52 Mike
-1
```

### 输出样例

```
Jack
Alice
Peter
Kate
Mike
```

### 代码

```
    int i,j,k;

    for(i=0;i<count-1;i++)
        for(j=i;j<count;j++)
            if(pS[i]->getRank()>=pS[j]->getRank()){
                Student *tmp;
                tmp=pS[i];
                pS[i]=pS[j];
                pS[j]=tmp;
            }

    for(k=0;k<count;k++)
        delete(pS[k]);
```

## 学生成绩的快速录入（构造函数）

### 题目概述

现在需要录入一批学生的成绩（学号，成绩）。其中学号是正整数，并且录入时，后录入学生的学号会比前面的学号大；成绩分两等，通过(Pass,录入时用1代表),不通过(Fail,录入时用0代表）。

由于很多学号都是相邻的，并且学号相邻的学生成绩常常相同。所以在录入时，适当地加了速。如果当前学生的学号比前面的学号大1，且成绩与前面的成绩相同，则只输入0即可。

### 函数接口定义

```
完成Student类
```

### 裁判测试程序样例

```
#include<iostream>
using namespace std;

/* 请在这里填写答案 */

int main(){
    const int size=100;
    int i, N, no, score;
    Student *st[size];
    cin>>N;
    for(i=0; i<N; i++){
        cin>>no;
        if(no>0){
            cin>>score;
            st[i]=new Student(no, score);
        }
        else
            st[i]=new Student(*st[i-1]);
    }
    cout<<Student::count<<" Students"<<endl;
    for(i=0;i<N;i++) st[i]->display();
    for(i=0;i<N;i++) delete st[i];
    return 0;
}
```

### 输入样例

```
5
3 0
0
7 1
0
12 1
```

### 输出样例

```
5 Students
3 Fail
4 Fail
7 Pass
8 Pass
12 Pass
```

### 代码

```
class Student{
private:
    int no;
    int score;
public:
    static int count;
    Student(int no1,int score1):no(no1),score(score1){count++;};
    Student(const Student& p){
        no=p.no+1;
        score=p.score;
        count++;
    };
    void display(){
        cout<<no;
        if(score==1) cout<<" Pass"<<endl;
        else if(score==0) cout<<" Fail"<<endl;
    };
    ~Student(){count--;}
};
int Student::count=0;
```

## 整数的素因子分解

### 题目概述

将正整数n分解为其素因子的乘积，其中n>=2并且在int范围内。Solution类的数据成员n代表需要分解的正整数，构造函数完成对数据成员n的初始化，声明了成员函数solve()实现对n的分解。请根据样例输出实现成员函数。注意输出时每行最后一个数字后面没有空格。

### 裁判测试程序样例

```
#include <iostream>
#include <cmath>
using namespace std;

class Solution {
public:
    Solution(int num) {
        n = num;
    }
    void solve();//将n分解为素因子的乘积
private:
    int n;
};

int main()
{
    int n;
    while (cin >> n) {
        Solution obj(n);
        obj.solve();
    }
    return 0;
}
//你的代码将被嵌在这里
```

### 输入样例

```
2
3
13
24
36
1024
65535
```

### 输出样例

```
2=2
3=3
13=13
24=2*2*2*3
36=2*2*3*3
1024=2*2*2*2*2*2*2*2*2*2
65535=3*5*17*257
```

### 代码

```
bool prime( int a )
{
    if(a<=1) return false;
    for(int i=2; i<=sqrt(a); i++)
        if(a%i==0) return false;
    return true;
}


void Solution::solve()
{
    int i=0,k=1;
    cout<<n<<"=";
    if(prime(n)) cout<<n;
    else
    {
        for(i=2; ; )
            if(prime(i) && n%i==0 )
            {
                n/=i;
                if(k){
                    cout<<i;
                    k--;
                }
                else cout<<"*"<<i;

                if(n==1) break;
            }
            else i++;
    }
    cout<<endl;
}
```

## 创建CPU

### 题目概述

定义一个CPU类，包含等级（Rank）、频率（frequency）、电压(voltage)等属性。其中，rank为枚举类型CPU__Rank,定义为enum CPU_Rank{P1=1,P2,P3,P4,P5,P6,P7},frequency为单位是MHz的整型数，voltage为浮点型的电压值。

### 函数接口定义

```
根据题目要求写出类。
```

### 裁判测试程序样例

```
/* 请在这里填写答案 */


int main()
{
    CPU a(P6,3,300); 
    
    cout<<"cpu a's parameter"<<endl;
    a.showinfo(); //显示性能参数

    CPU b; 
    cout<<"cpu b's parameter"<<endl;
    b.showinfo(); //显示性能参数

    CPU c(a); 
    cout<<"cpu c's parameter"<<endl;
    c.showinfo(); //显示性能参数
}
```

### 输出样例

```
create a CPU!
cpu a's parameter
rank:6
frequency:3
voltage:300
create a CPU!
cpu b's parameter
rank:1
frequency:2
voltage:100
copy create a CPU!
cpu c's parameter
rank:6
frequency:3
voltage:300
destruct a CPU!
destruct a CPU!
destruct a CPU!
```

### 代码

```
#include<iostream>
using namespace std;

enum CPU_Rank{P1=1,P2,P3,P4,P5,P6,P7};
class CPU{
private:
    CPU_Rank rank;
    int frequency;
    float voltage;
public:
    CPU(CPU_Rank ran=P1,int fre=2,float vol=100):rank(ran),frequency(fre),voltage(vol){
        cout<<"create a CPU!"<<endl;
    };
    CPU(const CPU& p){
        rank=p.rank;
        frequency=p.frequency;
        voltage=p.voltage;
        cout<<"copy create a CPU!"<<endl;
    }
    void showinfo(){
        cout<<"rank:"<<rank<<endl;
        cout<<"frequency:"<<frequency<<endl;
        cout<<"voltage:"<<voltage<<endl;
    };
    ~CPU(){cout<<"destruct a CPU!"<<endl;}
};
```

## 创建计算机

### 题目概述

定义一个简单的Computer类，有数据成员芯片(cpu)、内存(ram)、光驱(cdrom)等等，有两个公有成员函数run、stop。cpu为CPU类的一个对象，ram为RAM类的一个对象，cdrom为CDROM类的一个对象，定义并实现这个类，为以上的类编写构造和析构函数，注意使用类组合的思想解决该问题，使得给出的主函数代码可以正确运行并得到给出的输出结果。

### 函数接口定义

```
根据要求写出类的实现代码
```

### 裁判测试程序样例

```
/* 请在这里填写答案 */

在这里给出函数被调用进行测试的例子。例如：

int main()
{
    COMPUTER cpt1(6,4.0,200,60,32);  //测试带参数构造
    cout<<"cpt1's parameter:"<<endl;
    cpt1.showinfo();
    cout<<"------------------------------"<<endl;
    COMPUTER cpt2; //测试不带参数构造
    cout<<"cpt2's parameter:"<<endl;
    cpt2.showinfo();
    cout<<"------------------------------"<<endl;
    COMPUTER cpt3(cpt1); //测试复制构造
    cout<<"cpt3's parameter:"<<endl;
    cpt3.showinfo();
    cout<<"------------------------------"<<endl;   
}
```

### 输出样例

```
create a CPU!
create a RAM!
create a CDROM!
create a COMPUTER with para!
cpt1's parameter:
cpu parameter:
rank:6
frequency:4
voltage:200
ram parameter:
volumn:60 GB
cdrom parameter:
speed:32
------------------------------
create a CPU!
create a RAM!
create a CDROM!
no para to create a COMPUTER!
cpt2's parameter:
cpu parameter:
rank:1
frequency:2
voltage:100
ram parameter:
volumn:1 GB
cdrom parameter:
speed:16
------------------------------
create a CPU by copy!
create a RAM by copy!
create a CDROM by copy!
create a COMPUTER by copy!
cpt3's parameter:
cpu parameter:
rank:6
frequency:4
voltage:200
ram parameter:
volumn:60 GB
cdrom parameter:
speed:32
------------------------------
destruct a COMPUTER!
destruct a CDROM!
desturct a RAM!
desturct a CPU!
destruct a COMPUTER!
destruct a CDROM!
desturct a RAM!
desturct a CPU!
destruct a COMPUTER!
destruct a CDROM!
desturct a RAM!
desturct a CPU!
```

### 代码

```
#include<iostream>
using namespace std;

class CPU{
private:
    int rank;
    int frequency;
    int voltage;

public:
    CPU(int rank1=1,int frequency1=2,int voltage1=100):rank(rank1),frequency(frequency1),voltage(voltage1){
        cout<<"create a CPU!"<<endl;
    }
    CPU(const CPU& p){
        rank=p.rank;
        frequency=p.frequency;
        voltage=p.voltage;
        cout<<"create a CPU by copy!"<<endl;
    }
    ~CPU(){cout<<"desturct a CPU!"<<endl;}

    void showinfo(){
        cout<<"cpu parameter:"<<endl;
        cout<<"rank:"<<rank<<endl;
        cout<<"frequency:"<<frequency<<endl;
        cout<<"voltage:"<<voltage<<endl;
    }

    void run(){
        cout<<"CPU is running!"<<endl;
    }

    void stop(){
        cout<<"CPU stopped!"<<endl;
    }

};

class RAM{
private:
    int volumn;
public:
    RAM(int volumn1=1):volumn(volumn1){
        cout<<"create a RAM!"<<endl;
    }
    RAM(const RAM& p){
        volumn=p.volumn;
        cout<<"create a RAM by copy!"<<endl;
    }
    ~RAM(){cout<<"desturct a RAM!"<<endl;}

    void showinfo(){
        cout<<"ram parameter:"<<endl;
        cout<<"volumn:"<<volumn<<" GB"<<endl;
    }

    void run(){
        cout<<"RAM is running!"<<endl;
    }

    void stop(){
        cout<<"RAM stopped!"<<endl;
    }
};

class CDROM{
private:
    int speed;
public:
    CDROM(int speed1=16):speed(speed1){
        cout<<"create a CDROM!"<<endl;
    }
    CDROM(const CDROM& p){
        speed=p.speed;
        cout<<"create a CDROM by copy!"<<endl;
    }
    ~CDROM(){cout<<"destruct a CDROM!"<<endl;}

    void showinfo(){
        cout<<"cdrom parameter:"<<endl;
        cout<<"speed:"<<speed<<endl;
    }

    void run(){
        cout<<"CDROM is running!"<<endl;
    }

    void stop(){
        cout<<"CDROM stopped!"<<endl;
    }
};

class COMPUTER{
private:
    CPU cpuer;
    RAM ramer;
    CDROM cdromer;
public:
    COMPUTER(int rank,int frequency,int voltage,int volumn,int speed):cpuer(rank,frequency,voltage),ramer(volumn),cdromer(speed){
        cout<<"create a COMPUTER with para!"<<endl;
    }
    COMPUTER(){
        cout<<"no para to create a COMPUTER!"<<endl;
    }
    COMPUTER(const COMPUTER& p):cpuer(p.cpuer),ramer(p.ramer),cdromer(p.cdromer){
        cout<<"create a COMPUTER by copy!"<<endl;
    }
    ~COMPUTER(){cout<<"destruct a COMPUTER!"<<endl;}

    void showinfo(){
        cpuer.showinfo();
        ramer.showinfo();
        cdromer.showinfo();
    }

    void run(){
        cout<<"COMPUTER is running!"<<endl;
    }

    void stop(){
        cout<<"COMPUTER stopped!"<<endl;
    }
};


```

## 定义Date类

### 题目概述

本题要求实现一个日期类定义，根据所定义的类可以完成相关的类测试。

### 函数接口定义

```
根据Date被使用的情况，进行Date类定义，要求通过构造函数进行日期初始化，并通过display（）函数进行日期格式显示，显示格式为"月/日/年"
```

### 裁判测试程序样例

```
int main()
{
 Date d1(3,25,2019);
 Date d2(3,30);
 Date d3(10);
 Date d4;
 d1.display();
 d2.display();
 d3.display();
 d4.display();
 return 0;
 }

/* 请在这里填写答案 */
```

### 输出样例

```
3/25/2019
3/30/2019
10/1/2019
1/1/2019
```

### 代码

```
class Date{
private:
    int year;
    int month;
    int day;
public:
    Date(int d=1,int m=1,int y=2019):year(y),month(m),day(d){}
    void display(){ cout<<day<<"/"<<month<<"/"<<year<<endl; }
};
```

## 学生平均分计算

### 题目概述

定义一学生类，已有若干个学生数据，包括学号、姓名、成绩，要求输出这些学生数据并计算平均分。

### 函数接口定义

```
定义一学生类，已有若干个学生数据，包括学号、姓名、成绩，要求输出这些学生数据并计算平均分
```

### 裁判测试程序样例

```
利用学生类进行对象定义并输出结果的例子如下：
/* 请在这里填写答案 */

int Stud::sum=0;
int Stud::num=0;
int main()
{
    Stud  s1(1,"Li",89)，s2(2,"Chert",78)，s3(3,"zheng",94);
    s1.disp();
    s2.disp();
    s3.disp()；
    cout<<"avg="<<Stud::avg()<<endl;
    return 0；
}
```

### 输出样例

```
1，Li，89
2，Chert，78
3，zheng，94
avg=87
```

### 代码

```
#include<iostream>
using namespace std;

class Stud{
private:
    int no;
    string name;
    int score;
public:
    static int sum;
    static int num;
    Stud(int no1,string name1,int score1):no(no1),name(name1),score(score1){
        sum+=score;
        num++;
    };
    void disp(){
        cout<<no<<","<<name<<","<<score<<endl;
    }
    static int avg(){return sum/num;}
};
```

## 学生类的构造与析构

### 题目概述

定义一个学生类Student，使得main()函数能够得到指定的输出结果。

### 裁判测试程序样例

```
/* 请在这里填写答案 */
int main()
  {Student stud1(10010,"Wang_li",'f');
   stud1.display();
   Student stud2(10011,"Zhang_fun",'m');
   stud2.display();
   return 0;
}

```

### 输出样例

```
Constructor called.
num:10010
name:Wang_li
sex:f

Constructor called.
num:10011
name:Zhang_fun
sex:m

Destructor called.
Destructor called.
```

### 代码

```
#include<iostream>
using namespace std;

class Student{
private:
    int num;
    string name;
    char sex;
public:
    Student(int num1,string name1,char sex1):num(num1),name(name1),sex(sex1){
        cout<<"Constructor called."<<endl;
    }
    void display(){
        cout<<"num:"<<num<<endl;
        cout<<"name:"<<name<<endl;
        cout<<"sex:"<<sex<<endl<<endl;
    }
    ~Student(){cout<<"Destructor called."<<endl;}
};
```

## 立方体类的实现

### 题目概述

立方体类Box的实现，完成计算体积、计算表面积、输出结果等功能。

### 函数接口定义

```
输入格式:
立方体的边长，可以是float类型的数据。

输出格式:
立方体的体积和表面积，中间用一个空格隔开，末尾换行。
```

### 裁判测试程序样例

```
int  main( ){
    float ab;
    cin>>ab;
    Box  obj;
    obj.seta( ab );
    obj.getvolume( );
    obj.getarea( );
    obj.disp( );
    return 0;
}
```

### 输入样例

```
3
```

### 输出样例

```
27 54
```

### 代码

```
#include <iostream>
#include <cmath>
using namespace std;
class Box{
    private:
        int length;
        int volume;
        int square;
    public:
        void seta(float a){
            length=a;
        }
        void getvolume(){
            volume=pow(length,3);
        }
        void getarea(){
            square=6*pow(length,2);
        }
        void disp(){
            cout<<volume<<" "<<square;
        }
};

int  main( ){
    float ab;
    cin>>ab;
    Box  obj;
    obj.seta(ab);
    obj.getvolume( );
    obj.getarea( );
    obj.disp( );
    return 0;
}
```

## 类的定义和使用

### 题目概述

定义一个日期类Date，内有数据成员年、月、日，另有成员函数：构造函数用于初始化数据成员，输出，闰年的判断。编写主函数：创建日期对象，计算并输出该日是该年的第几天。

### 函数接口定义

```
定义一个日期类Date，内有数据成员年、月、日，另有成员函数：构造函数用于初始化数据成员，输出，闰年的判断。编写主函数：创建日期对象，计算并输出该日是该年的第几天。
```

### 输入样例

```
2006 3 5
```

### 输出样例

```
64   (2006年3月5日是该年的第64天)
```

### 代码

```
#include <iostream>
using namespace std;
class Date{
private:
    int year;
    int month;
    int day;
    int result=0;
    int a[12]={31,28,31,30,31,30,31,31,30,31,30,31};
public:
    void getdata() {
        cin >> year>> month>> day;
    };
    void getresult(){
        for(int i=1;i<month;i++){
            result=result+a[i-1];
        }
        result=result+day;
        if (month>2&&(year%100==0&&year%400==0)||(year%100!=0&&year%4==0))
            result=result+1;
        cout<<result;
    }
};

int  main( ){
    Date obj;
    obj.getdata();
    obj.getresult();
    return 0;
}
```

## 类的定义和使用

### 题目概述

请定义一个Point类，有两个数据成员：x和y, 分别代表x坐标和y坐标，并有若干构造函数和一个移动的成员函数，可输出移动后新的坐标值。

### 函数接口定义

```
输入:
第一行的两个数 分别表示 点的x坐标和y坐标。 第二行的两个数 分别表示 x和y方向移动的距离。

输出:
移动后的点的x坐标和y坐标。
```

### 输入样例

```
1  5
2  5
```

### 输出样例

```
3 10
```

### 代码

```
#include <iostream>
using namespace std;
class Point{
private:
    int x;
    int y;
    int movx;
    int movy;
    int newx;
    int newy;
public:
    void getdata(){
        cin>>x>>y>>movx>>movy;
    };
    void getmove(){
        newx=x+movx;
        newy=y+movy;
    };
    void disp(){
        cout << newx << " " << newy;
    }
};

int  main( ){
    Point obj;
    obj.getdata();
    obj.getmove();
    obj.disp();
    return 0;
}
```

## 个位数统计

### 题目概述

给定一个k位整数N = dk-110k-1 + ... + d1101 + d0 (0<=di<=9, i=0,...,k-1, dk-1>0)，请编写程序统计每种不同的个位数字出现的次数。例如：给定N = 100311，则有2个0，3个1，和1个3。

### 函数接口定义

```
输入格式：

每个输入包含1个测试用例，即一个不超过1000位的正整数N。

输出格式：

对N中每一种不同的个位数字，以D:M的格式在一行中输出该位数字D及其在N中出现的次数M。要求按D的升序输出。
```

### 输入样例

```
100311
```

### 输出样例

```
0:2
1:3
3:1
```

### 代码

```
#include <iostream>
#include <string>
using namespace std;
class Countnum{
private:
    string str;
    int i=0;
    int a[10]={0};
public:
    void givecount(){
        cin>>str;
        while(i<=str.length()){
            a[str[i]-'0']++;
            i++;
        }
        for(int i=0;i<10;i++)
            if (a[i]>0)
                cout<<i<<":"<<a[i]<<endl;
    };
};

int main(){
    Countnum obj;
    obj.givecount();
    return 0;
}
```

## 乒乓球俱乐部的年度考核

### 题目概述

一个乒乓球俱乐部有A、B两组。其中A组为主力队员，承担对外比赛任务；B组为后备队，日常以训练为主。
下面给出基类的框架：

```
class Group
{
protected:
string name;//姓名
public:
virtual void display()=0;//显示考核成绩
}
```

以Group为基类，构建GroupA和GroupB三个类。

生成上述类并编写主函数，要求主函数中有一个基类Group指针数组，数组元素不超过20个。

Group pg[20];

主函数根据输入的信息，相应建立GroupA, GroupB类对象。

### 函数接口定义

```
输入格式：每个测试用例占一行，第一项为类型，A为GroupA，B为GroupB, 第二项是运动员姓名，对于GroupA来说，第3项是总胜利场数，第4项是总失败场数。对于GroupB来说，接下来为各场测试赛的成绩（不超过5场）。输入0表示输入结束。
要求输出运动员姓名，组别和评分。对于GroupA来说，总分为: 2胜利场数-失败场数。对于GroupB来说，总分为: 胜利的局数-失败的局数。
```

### 输入样例

```
A Tom 20 3
B Jack 4:2 3:4 4:0 1:4
A Jim 15 9
A John 10 11
B Tony 4:1 0:4 4:3 4:3 4:2
B Li 3:2
0
```

### 输出样例

```
Tom A 37
Jack B 2
Jim A 21
John A 9
Tony B 3
Li B 1
```

### 代码

```
#include <iostream>
using namespace std;
class Group
{
protected:
    string name;//姓名
public:
    virtual void display()=0;//显示考核成绩
    Group(string name1):name(name1){}
};


class GroupA:public Group{
private:
    int score;
public:
    GroupA(string name1,int scoreA):Group(name1),score(scoreA){}
    void display(){cout<<name<<" A "<<score<<endl;}
};

class GroupB:public Group{
private:
    int score;
public:
    GroupB(string name1,int scoreB):Group(name1),score(scoreB){}
    void display(){cout<<name<<" B "<<score<<endl;}
};



int main(){
    Group *pg[20];

    char type;
    string name;

    int win;
    int lose;
    int total;

    int num1;
    int num2;
    int i;
    int count=0;
    char c;

    cin>>type;
    while(type!='0'){
        cin>>name;
        if(type=='A'){
            cin>>win>>lose;
            total=2*win-lose;
            pg[count++]=new GroupA(name,total);

        }
        else if(type=='B'){
            win=0;
            lose=0;
            c=cin.get();
            while(c!='\n'){
                cin>>num1;
                win+=num1;
                cin.get();
                cin>>num2;
                lose+=num2;
                c=cin.get();
            }
            total=win-lose;
            pg[count++]=new GroupB(name,total);

        }
        cin>>type;
    }
    for(i=0;i<count;i++){
        pg[i]->display();
    }

    return 0;
}
```

## 字符统计问题

### 题目概述

实验室有个胖子叫小明，他第一天进来，老师就叫他练打字，开始练的是打英文字母，小明打了一小时就累啦，老师看他累就叫他不用再练啦，但前提是必须知道他打的英文字母串里连续相同的字符最多到底有几个？亲爱的同学们，如果你们同情胖子小明，那么你就帮他写个程序，计算一下在一个字符串里面连续相同的字符最多到底有多少个？这里我们假定字符串都是由英文小写字母、大写字母和数字组成的；字符串的长度不超过100000；我们规定“相同”的定义为：

```
1：不区分大小写；
2：“0”=“a”=“A”；“1”=“b”=“B”；……；“9”=“j”=“J”；
```

### 函数接口定义

```
输入是一行字符串，输出有两行，第一行的格式是：“From=XX,To=XX”，第二行的格式是：“MaxLen=XX”。本问题有多组测试数据。如果有多组符合要求的解，那么输出起始位置最小的解。
```

### 输入样例

```
iaaaaa000AAAAwh
```

### 输出样例

```
From=2,To=13
MaxLen=12
```

### 代码（答案错误）

```
#include <iostream>
#include <string>
using namespace std;

class Charc{
public:
    int start=-1;
    int end=-1;
    char ch='\0';
    int lowup='a'-'A',lownum='a'-'0';
    char comp[3][2]={'a','z','A','Z','0','9'};
    void handle(){
        int flag;
        for(flag=0;flag<3;flag++){
            if(ch>=comp[flag][0]&&ch<=comp[flag][1])
                break;
        };
        switch(flag){
            case 0:
                break;
            case 1:
                ch += lowup;
                break;
            case 2:
                ch += lownum;
                break;
        }
    };
};

int main(){
    int i=1;
    Charc result1,result2,tmp,noch;
    result1.start=0;
    result1.end=0;

    string st;
    cin>>st;

    result1.ch=st[0];
    result1.handle();

    while(st[i]!='\0'){
        tmp.ch=st[i];
        tmp.start=i;
        tmp.end=i;
        tmp.handle();

        if(result2.ch=='\0'){
            if(result1.ch==tmp.ch){
                result1.end++;
            }
            else{
                result2=tmp;
            };
            tmp=noch;
        }
        else{
            if(tmp.ch==result2.ch) {
                result2.end++;
                if (result1.end - result1.start < result2.end - result2.start) {
                    result1 = result2;
                    result2=noch;
                }
            }
            else{
                result2=tmp;
            };
            tmp=noch;
        }
        i++;
    };

    cout<<"From="<<result1.start+1<<",To="<<result1.end+1<<endl;
    cout<<"MaxLen="<<result1.end-result1.start+1<<endl;
    return 0;
}
```

## 该日是该年的第几天

### 题目概述

定义一个日期类Date，内有数据成员年、月、日，另有成员函数：构造函数用于初始化数据成员，输出，闰年的判断。 编写主函数：创建日期对象，计算并输出该日是该年的第几天。 输入格式： 测试输入包含若干测试用例，每个测试用例占一行。当读入0 0 0时输入结束，相应的结果不要输出。

### 输入样例

```
2006 3 5
2000 3 5
0 0 0
```

### 输出样例

```
64 (2006年3月5日是该年的第64天)
65 (2000年3月5日是该年的第65天)
```

### 代码

```
#include <iostream>
using namespace std;
class Date{
private:
    int year;
    int month;
    int day;
    int result=0;
    int a[12]={31,28,31,30,31,30,31,31,30,31,30,31};
public:
    Date(int x,int y,int z){
        year=x;
        month=y;
        day=z;
    };
    void getresult(){
        for(int i=1;i<month;i++){
            result=result+a[i-1];
        }
        result=result+day;
        if (month>2&&(year%100==0&&year%400==0)||(year%100!=0&&year%4==0))
            result=result+1;
        cout<<result<<endl;
    }
};

int main()
{
    const int SIZE=100;
    int m,n,p;
    int i;
    int N=0;
    Date *da[SIZE];
    cin>>m>>n>>p;
    while(m!=0&&n!=0&&p!=0){
        da[N++]=new Date(m,n,p);
        cin>>m>>n>>p;
    };
    for(i=0;i<N;i++) da[i]->getresult();


    return 0;
}
```

## 类的定义和使用

### 题目概述

定义一个日期类Date，内有数据成员year、month和day，分别代表年、月、日，并若干有成员函数：构造函数用于初始化数据成员，isLeap函数用于闰年的判断。编写主函数：创建日期对象，判断该年是否是闰年。

### 函数接口定义

```
输入格式:
每组测试数据仅包含一个测试用例，每个测试用例占一行包括三个数，分别表示年、月、日。

输出格式:
如果是闰年则输出yes，不是则输出no。
```

### 输入样例

```
2006 3 5
```

### 输出样例

```
no
```

### 代码

```
#include <iostream>
using namespace std;
class Date{
private:
    int year;
    int month;
    int day;
public:
    Date(int a,int b,int c){
        year=a;
        month=b;
        day=c;
    }
    void isLeap(){
        if (month>2&&(year%100==0&&year%400==0)||(year%100!=0&&year%4==0))
            cout<<"yes";
        else
            cout<<"no";
    }
};

int  main( ){
    int a;
    int b;
    int c;
    cin>>a>>b>>c;
    Date obj(a,b,c);
    obj.isLeap();
    return 0;
}
```

## 类的定义final

### 题目概述

定义一个类DDate，内有数据成员year、month和day，分别代表年、月、日，并若干有成员函数：构造函数用于初始化数据成员，isLeap函数用于闰年的判断。编写主函数：创建日期对象，判断该年是否是闰年。

以上类名和函数名，均须按照题目要求，不得修改

### 函数接口定义

```
输入格式:
每组测试数据仅包含一个测试用例，每个测试用例占一行包括三个数，分别表示年、月、日。

输出格式:
如果该年是闰年则输出yes，不是则输出no。
```

### 输入样例

```
2006 3 5
```

### 输出样例

```
no
```

### 代码

```
#include <iostream>
using namespace std;
class DDate{
private:
    int year;
    int month;
    int day;
public:
    int result=0;
    void getdata() {
        cin >>year>>month>>day;
    };
    void isLeap(){
        if (month>2&&(year%100==0&&year%400==0)||(year%100!=0&&year%4==0))
            result=1;
    }
};

int  main( ){
    DDate obj;
    obj.getdata();
    obj.isLeap();
    if(obj.result==1)
        cout<<"yes";
    else
        cout<<"no";
    return 0;
}
```

## 设计一个矩形类Rectangle并创建测试程序（C++）

### 题目概述

设计一个名为Rectangle的矩形类，这个类包括：两个名为width和height的double数据域，它们分别表示矩形的宽和高。width和height的默认值都为1.该类包括矩形类的无参构造函数（默认构造函数）；一个width和height为指定值的矩形构造函数；一个名为getArea( )的函数返回矩形的面积；一个名为getPerimeter( )的函数返回矩形的周长。请实现这个类。编写一个测试程序，创建一个Rectangle对象，从键盘输入矩形的宽和高，然后输出矩形的面积和周长。

### 函数接口定义

```
输入格式:
3.5 35.9（第一个数表示矩形的宽，第二个数表示矩形的高，中间是空间分隔。）

输出格式:
125.65 （第一行输出矩形的面积） 78.8 （第二行输出矩形的周长）
```

### 输入样例

```
3.5 35.9
```

### 输出样例

```
125.65
78.8
```

### 代码

```
#include <iostream>
using namespace std;
class Rectangle{
private:
    double width=1;
    double height=1;
public:
    void getdata(){
        cin>>width>>height;
    }
    double getArea(){
        return width*height;
    };
    double getPerimeter(){
        return 2*(width+height);

    };
    void disp(){
        cout<<getArea()<<"\n"<<getPerimeter();
    }
};

int main(){
    Rectangle obj;
    obj.getdata();
    obj.getArea();
    obj.getPerimeter();
    obj.disp();
    return 0;
}
```

## 队列操作

### 题目概述

请实现一个MyQueue类，实现出队，入队，求队列长度.

实现入队函数 void push(int x); 实现出队函数 int pop(); 实现求队列长度函数 int size();

### 函数接口定义

```
输入格式:
每个输入包含1个测试用例。每个测试用例第一行给出一个正整数 n (n <= 10^6) ，接下去n行每行一个数字，表示一种操作： 1 x ： 表示从队尾插入x，0<=x<=2^31-1。 2 ： 表示队首元素出队。 3 ： 表示求队列长度。

输出格式:
对于操作2,若队列为空，则输出 “Invalid”,否则请输出队首元素。 对于操作3，请输出队列长度。 每个输出项最后换行。
```

### 输入样例

```
5
3
2
1 100
3
2
```

### 输出样例

```
0
Invalid
1
100
```

### 代码

```
#include<iostream>
using namespace std;

typedef struct quene{
    int data;
    struct quene *next;
} Quenedat;

class MyQuene{
private:
    Quenedat *head,*tail,*node;
    static int i;
public:
    MyQuene(){
        head=NULL;
        tail=NULL;
        node=NULL;
        head=new Quenedat;
        tail=head;
    }

    void push(int x){
        node=new Quenedat;
        node->data=x;
        tail->next=node;
        node=tail;
        i++;
    }

    void pop(){
        if(i==0) cout<<"Invalid"<<endl;
        else{
            cout<<head->next->data<<endl;
            head->next=head->next->next;
            delete[] head->next;
            i--;
        }
    }

    int size(){
        return i;
    }
};

int MyQuene::i=0;

int main(){
    MyQuene obj;
    int step, num;
    int repeat;
    cin>>repeat;
    for(int ri=0;ri<repeat;ri++) {
        cin >> step;
        if (step == 1) {
            cin >> num;
            obj.push(num);
        }
        else if (step == 2) obj.pop();
        else if (step == 3) cout << obj.size() << endl;
    }
    return 0;
}
```

## 复数类的操作

### 题目概述

1、声明一个复数类Complex（类私有数据成员为double型的real和image）

2、定义构造函数，用于指定复数的实部与虚部。

3、定义取反成员函数，调用时能返回该复数的相反数（实部、虚部分别是原数的相反数）。

4、定义成员函数Print()，调用该函数时，以格式(real, image)输出当前对象。

5、编写加法友元函数，以复数对象c1，c2为参数，求两个复数对象相加之和。

6、主程序实现：

（1）读入两个实数，用于初始化对象c1。

（2）读入两个实数，用于初始化对象c2。

（3）计算c1与c2相加结果，并输出。

（4）计算c2的相反数与c1相加结果，并输出。

### 函数接口定义

```
输入格式:
输入有两行：

第一行是复数c1的实部与虚部，以空格分隔；

第二行是复数c2的实部与虚部，以空格分隔。

输出格式:
输出共三行:

第一行是c1与c2之和；

第二行是c2的相反数与c1之和；

第三行是c2 。
```

### 输入样例

```
2.5 3.7
4.2 6.5
```

### 输出样例

```
(6.7, 10.2)
(-1.7, -2.8)
(4.2, 6.5)
```

### 代码

```
#include <iostream>
using namespace std;

class Complex{
private:
    double real;
    double image;
public:
    Complex(double r=0,double i=0):real(r),image(i){};
    void Print(){
        cout<<"("<<real<<", "<<image<<")"<<endl;
    }
    Complex opp(){
        Complex r;
        r.real=-real;
        r.image=-image;
        return r;
    }
    friend Complex add(Complex &p,Complex &q);
};

Complex add(Complex &p,Complex &q){
    Complex r;
    r.real=p.real+q.real;
    r.image=p.image+q.image;
    return r;
}

int main(){
    double real1,image1;
    double real2,image2;
    cin>>real1>>image1>>real2>>image2;
    Complex c1(real1,image1),c2(real2,image2);
    Complex sumc=add(c1,c2);
    Complex co2=c2.opp();
    Complex sumoc=add(c1,co2);
    sumc.Print();
    sumoc.Print();
    c2.Print();
}
```

## 狗的继承

### 题目概述

完成两个类，一个类Animal，表示动物类，有一个成员表示年龄。一个类Dog，继承自Animal，有一个新的数据成员表示颜色，合理设计这两个类，使得测试程序可以运行并得到正确的结果。

### 裁判测试程序样例

```
/* 请在这里填写答案 */

int main(){
    Animal ani(5);
    cout<<"age of ani:"<<ani.getAge()<<endl;
    Dog dog(5,"black");
    cout<<"infor of dog:"<<endl;
    dog.showInfor();
}
```

### 输出样例

```
age of ani:5
infor of dog:
age:5
color:black
```

### 代码

```
#include <iostream>
using namespace std;

class Animal{
private:
    int age;
public:
    Animal(int a):age(a){}
    int getAge(){ return age; }
};

class Dog:public Animal{
private:
    string color;
public:
    Dog(int a, string c):Animal(a),color(c){}
    void showInfor(){
        cout<<"age:"<<Animal::getAge()<<endl;
        cout<<"color:"<<color<<endl;
    }
};
```

## 写出派生类构造方法（C++）

### 题目概述

裁判测试程序样例中展示的是一段定义基类People、派生类Student以及测试两个类的相关C++代码，其中缺失了部分代码，请补充完整，以保证测试程序正常运行。

### 函数接口定义

```
提示：
观察类的定义和main方法中的测试代码，补全缺失的代码。
```

### 裁判测试程序样例

```
#include <iostream>
using namespace std;
class People{
private:
    string id;
    string name;
public:
    People(string id, string name){
        this->id = id;
        this->name = name;
    }
    string getId(){
        return this->id;
    }
    string getName(){
        return name;
    }
};
class Student : public People{
private:
    string sid;
    int score;
public:
    Student(string id, string name, string sid, int score)
        
        /** 你提交的代码将被嵌在这里（替换此行） **/
        
    }
    friend ostream& operator <<(ostream &o, Student &s);
};
ostream& operator <<(ostream &o, Student &s){
    o << "(Name:" << s.getName() << "; id:"
      << s.getId() << "; sid:" << s.sid
      << "; score:" << s.score << ")";
    return o;
}
int main(){
    Student zs("370202X", "Zhang San", "1052102", 96);
    cout << zs  << endl;
    return 0;
}
```

### 输出样例

```
(Name:Zhang San; id:370202X; sid:1052102; score:96)
```

### 代码

```
    :People(id, name){
        this->sid = sid;
        this->score = score;
```

## 派生类的定义和使用

### 题目概述

按要求完成下面的程序：

1、定义一个Animal类，包含一个void类型的无参的speak方法，输出“animal language!”。

2、定义一个Cat类，公有继承自Animal类，其成员包括：

（1）私有string类型的成员m_strName;

（2）带参数的构造函数，用指定形参对私有数据成员进行初始化；

（3）公有的成员函数print_name，无形参，void类型，功能是输出成员m_strName的值，具体输出格式参见main函数和样例输出。

### 裁判测试程序样例

```
#include <iostream>
#include <string>
using namespace std;

/* 请在这里填写答案 */

int main()
{
    Cat cat("Persian"); //定义派生类对象
    cat.print_name();   //派生类对象使用本类成员函数
    cat.speak();    //派生类对象使用基类成员函数
    return 0;
}
```

### 输出样例

```
cat name: Persian
animal language!
```

### 代码

```
class Animal{
public:
    void speak(){ cout<<"animal language!"<<endl; }
};

class Cat:public Animal{
private:
    string m_strName;
public:
    Cat(string m):m_strName(m){};
    void print_name(){ cout<<"cat name: "<<m_strName<<endl; }
};
```

## 派生类使用基类的成员函数

### 题目概述

按要求完成下面的程序：

1、定义一个Animal类，成员包括：

（1）整数类型的私有数据成员m_nWeightBase，表示Animal的体重；

（2）整数类型的保护数据成员m_nAgeBase，表示Animal的年龄；

（3）公有函数成员set_weight，用指定形参初始化数据成员nWeightBase；

（4）公有成员函数get_weight，返回数据成员nWeightBase的值；

（5）公有函数成员set_age，用指定形参初始化数据成员m_nAgeBase；

2、定义一个Cat类，公有继承自Animal类，其成员包括：

（1）string类型的私有数据成员m_strName;

（2）带参数的构造函数，用指定形参对私有数据成员进行初始化；

（3）公有的成员函数print_age，功能是首先输出成员m_strName的值，然后输出“, age = ”，接着输出基类的数据成员m_nAgeBase的值。具体输出格式参见main函数和样例输出。

### 裁判测试程序样例

```
#include <iostream>
#include <string>
using namespace std;

/* 请在这里填写答案 */

int main()
{
    Cat cat("Persian");   //定义派生类对象cat
    cat.set_age(5);       //派生类对象调用从基类继承的公有成员函数
    cat.set_weight(6);    //派生类对象调用从基类继承的公有成员函数
    cat.print_age();      //派生类对象调用自己的公有函数
    cout << "cat weight = " << cat.get_weight() << endl;
    return 0;
}
```

### 输出样例

```
Persian, age = 5
cat weight = 6
```

### 代码

```
class Animal{
private:
    int m_nWeightBase;
protected:
    int m_nAgeBase;
public:
    void set_weight(int w){ m_nWeightBase = w; }
    int get_weight(){ return m_nWeightBase; }
    void set_age(int a){ m_nAgeBase = a; }
};

class Cat:public Animal{
private:
    string m_strName;
public:
    Cat(string m):m_strName(m){};
    void print_age(){ cout<<m_strName<<", age = "<<m_nAgeBase<<endl; }
};
```

## 私有继承派生类使用基类的成员函数

### 题目概述

按要求完成下面的程序：

1、定义一个Animal类，成员包括：

（1）整数类型的私有数据成员m_nWeightBase，表示Animal的体重；

（2）整数类型的保护数据成员m_nAgeBase，表示Animal的年龄；

（3）公有函数成员set_weight，用指定形参初始化数据成员m_nWeightBase；

（4）公有成员函数get_weight，返回数据成员m_nWeightBase的值；

（5）公有函数成员set_age，用指定形参初始化数据成员m_nAgeBase；

2、定义一个Cat类，私有继承自Animal类，其成员包括：

（1）string类型的私有数据成员m_strName;

（2）带参数的构造函数，用指定形参对私有数据成员进行初始化；

（3）公有的成员函数set_print_age，功能是首先调用基类的成员函数set_age设置数据成员m_nAgeBase的值为5，接着输出成员m_strName的值，然后输出“, age = ”，最后输出基类的数据成员m_nAgeBase的值。具体输出格式参见main函数和样例输出。

（4）公有的成员函数set_print_weight，功能是首先调用基类的成员函数set_weight设置数据成员nWeightBase的值为6，接着输出成员m_strName的值，然后输出“, weight = ”，最后调用基类的成员函数get_weight输出基类的数据成员m_nAgeBase的值。具体输出格式参见main函数和样例输出。

### 裁判测试程序样例

```
#include <iostream>
#include <string>
using namespace std;

/* 请在这里填写答案 */

int main()
{
    Cat cat("Persian");     //定义派生类对象cat
    cat.set_print_age();
    cat.set_print_weight(); //派生类对象调用自己的公有函数
    return 0;
}
```

### 输出样例

```
Persian, age = 5
Persian, weight = 6
```

### 代码

```
class Animal{
private:
    int m_nWeightBase;
protected:
    int m_nAgeBase;
public:
    void set_weight(int w){ m_nWeightBase = w; }
    int get_weight(){ return m_nWeightBase; }
    void set_age(int a){ m_nAgeBase = a; }
};

class Cat:private Animal{
private:
    string m_strName;
public:
    Cat(string m):m_strName(m){};
    void set_print_age(){
        set_age(5);
        cout<<m_strName<<", age = "<<m_nAgeBase<<endl;
    }
    void set_print_weight(){
        set_weight(6);
        cout<<m_strName<<", weight = "<<get_weight()<<endl;
    }
};
```

## 学生信息输入输出

### 题目概述

根据给出的基类，定义派生类，完成学生信息的输入输出（参见输入输出样例）

### 函数接口定义

```
#include <iostream>
#include <string>
using namespace std;
class Student
{public:
  void get_value()
   {cin>>num>>name>>sex;}
  void display( )
    {cout<<"num: "<<num<<endl;
     cout<<"name: "<<name<<endl;
     cout<<"sex: "<<sex<<endl;}
 private :
   int num;
   string name;
   char sex;
};   

/*在这里添加派生类的定义*/
```

### 裁判测试程序样例

```
int main()
 {Student1 stud1;
  stud1.get_value();
  stud1.get_value_1();
  stud1.display();
  stud1.display_1();
  return 0;
} 
```

### 输入样例

```
1002 Lisi F
20 beijing
```

### 输出样例

```
num: 1002
name: Lisi
sex: F
age: 20
address: beijing
```

### 代码

```
class Student1:public Student{
private:
    int age;
    string address;
public:
    void get_value_1(){ cin>>age>>address; }
    void display_1(){
        cout<<"age: "<<age<<endl;
        cout<<"address: "<<address<<endl;
    }
};
```

## 学生派生类

### 题目概述

根据所给的类Student定义其派生类，并利用构造函数进行数据初始化，使程序能按照"样例"的格式进行输出

### 函数接口定义

```
#include <iostream>
#include<string>
using namespace std;

class Student                             
 {public:                                
   Student(int n,string nam,char s )      
    {num=n;
     name=nam;
     sex=s; }
   ~Student( ) { }
  protected:                               
    int num;
    string name;
    char sex ;                           
};

/* 请在这里添加派生类定义 */
```

### 裁判测试程序样例

```
int main( )
 {Student1 stud1(10010,"Wang-li",'f',19,"115 Beijing Road,Shanghai");
  Student1 stud2(10011,"Zhang-fun",'m',21,"213 Shanghai Road,Beijing");
  stud1.show( );           
  stud2.show( );            
  return 0;
}
```

### 输出样例

```
num: 10010
name: Wang-li
sex: f
age: 19
address: 115 Beijing Road,Shanghai

num: 10011
name: Zhang-fun
sex: m
age: 21
address: 213 Shanghai Road,Beijing
```

### 代码

```
class Student1:public Student{
private:
    int age;
    string address;
public:
    Student1(int n,string nam,char s,int a,string add):Student(n,nam,s),age(a),address(add){};
    void show(){
        cout<<"num: "<<num<<endl;
        cout<<"name: "<<name<<endl;
        cout<<"sex: "<<sex<<endl;
        cout<<"age: "<<age<<endl;
        cout<<"address: "<<address<<endl<<endl;
    }
};
```

## 多重继承派生类构造函数

### 题目概述

根据所给的基类Student和Teacher，定义Graduate类。

### 函数接口定义

```
#include <iostream>
#include <string>
using namespace std;
class Teacher                          
 {public:                                 
   Teacher(string nam,int a,string t)      
    {name=nam;
     age=a;
     title=t;}
   void display()                         
     {cout<<"name:"<<name<<endl;
      cout<<"age"<<age<<endl;
      cout<<"title:"<<title<<endl;
     }
  protected:                          
    string name;
    int age;
    string title;                      
};

class Student                         
 {public:
   Student(string nam,char s,float sco)
     {name1=nam;
      sex=s;
      score=sco;}                        
   void display1()                      
    {cout<<"name:"<<name1<<endl;
     cout<<"sex:"<<sex<<endl;
     cout<<"score:"<<score<<endl;
    }
  protected:                             
   string name1;
   char sex;
   float score;                           
 };

 /* 请在这里填写答案 */
```

### 裁判测试程序样例

```
int main( )
 {Graduate grad1("Wang-li",24,'f',"assistant",89.5,1234.5);
  grad1.show( );
  return 0;
}
```

### 输出样例

```
name:Wang-li
age:24
sex:f
score:89.5
title:assistant
wages:1234.5
```

### 代码

```
class Graduate:public Student, public Teacher{
protected:
    float wages;
public:
    Graduate(string nam, int a, char s, string t, float sco, float w):Student(nam,s,sco),Teacher(nam,a,t),wages(w){};
    void show(){
        cout<<"name:"<<name<<endl;
        cout<<"age:"<<age<<endl;
        cout<<"sex:"<<sex<<endl;
        cout<<"score:"<<score<<endl;
        cout<<"title:"<<title<<endl;
        cout<<"wages:"<<wages<<endl;
    }
};
```

## 虚基类的应用-人与教师学生

### 题目概述

派生类定义：根据所给的基类，完成多重继承下的派生类定义

### 函数接口定义

```
#include <iostream>
#include <string>
using namespace std;
//定义公共基类Person
class Person                              
{public:
  Person(string nam,char s,int a)              
   {name=nam;sex=s;age=a;}
 protected:                              
   string name;
   char sex;
   int age;
};
//定义类Teacher
class Teacher:virtual public Person              
 {public:                                 
   Teacher(string nam,char s,int a,string t):Person(nam,s,a)       
    {title=t; 
    }
  protected:                                   
    string title;                                
};
//定义类Student
class Student:virtual public Person               
 {public:
   Student(string nam,char s,int a,float sco):   
      Person(nam,s,a),score(sco){}              
  protected:                                     
    float score;                               
 };

 /*这里添加派生类的定义*/
```

### 裁判测试程序样例

```
int main( )
 {Graduate grad1("Wang-li",'f',24,"assistant",89.5,1234.5);
  grad1.show( );
  return 0;
}
```

### 输出样例

```
name:Wang-li
age:24
sex:f
score:89.5
title:assistant
wages:1234.5
```

### 代码

```
class Graduate:virtual public Teacher, virtual public Student{
public:
    Graduate(string nam,char s,int a,string t,float sco,float w):
        Person(nam,s,a),Teacher(nam,s,a,t),Student(nam,s,a,sco),wages(w){};
    void show(){
        cout<<"name:"<<name<<endl;
        cout<<"age:"<<age<<endl;
        cout<<"sex:"<<sex<<endl;
        cout<<"score:"<<score<<endl;
        cout<<"title:"<<title<<endl;
        cout<<"wages:"<<wages<<endl;
    }
protected:
    float wages;
};
```

## 汽车类的继承

### 题目概述

根据给定的汽车类vehicle（包含的数据成员有车轮个数wheels和车重weight）声明，完成其中成员函数的定义，之后再定义其派生类并完成测试。
小车类car是它的派生类，其中包含载人数passenger_load。每个类都有相关数据的输出方法。

### 函数接口定义

```
#include<iostream>
using namespace std; 
class Vehicle 
{ 
    protected: 
        int wheels; 
        float weight; 
    public: 
        Vehicle(int wheels,float weight); 
        int get_wheels(); 
        float get_weight(); 
        float wheel_load(); 
        void show(); 
}; 

/* 请在这里填写答案 */
```

### 裁判测试程序样例

```
int main () 
{ 
    Vehicle v(4,1000);
    v.show(); 
    Car car1(4,2000,5);  
    car1.show (); 
    return 0;
}
```

### 输出样例

```
Type:Vehicle
Wheel:4
Weight:1000kg
Type:Car
Type:Vehicle
Wheel:4
Weight:2000kg
Load:5 persons
```

### 代码

```
Vehicle::Vehicle(int wheels, float weight) {
    this->wheels = wheels;
    this->weight = weight;
}

void Vehicle::show() {
    cout<<"Type:Vehicle"<<endl;
    cout<<"Wheel:"<<wheels<<endl;
    cout<<"Weight:"<<weight<<"kg"<<endl;
}

class Car:public Vehicle{
private:
    int passenger_load;
public:
    Car(int wheels,float weight,int load):Vehicle(wheels,weight),passenger_load(load){};
    void show(){
        cout<<"Type:Car"<<endl;
        Vehicle::show();
        cout<<"Load:"<<passenger_load<<" persons"<<endl;
    }
};
```

## 复数的实部和虚部

### 题目概述

通常用一个形如”a+bi”的字符串来表示一个复数，a为复数的实部，b为复数的虚部。现在需要对输入的字符串进行分离，自动识别该复数的实部和虚部，并独立输出。

例如，对于输入的复数字符串“3-4.05i”，输出

complex 3-4.05i

the real part is 3

and the imaginary part is -4.05

注意：

1、用于表示复数的字符串符合数学上的书写习惯。

2、每组测试数据仅包括一个用于表示复数的字符串。

3、输入保证合法。

### 输入样例

```
-4.567+3.987i
```

### 输出样例

```
complex -4.567+3.987i
the real part is -4.567
and the imaginary part is 3.987
```

### 代码

```
#include <iostream>
#include <cstring>
using namespace std;


int main(){
    string comp;
    int i=1;
    cin>>comp;
    if(comp[comp.length()-1]!='i'){
        cout<<"complex "<<comp<<endl;
        cout<<"the real part is "<<comp<<endl;
        cout<<"and the imaginary part is 0";
    }
    else{
        while(comp[i]!='\0'){
            if(comp[i] == '+' || comp[i] == '-') break;
            i++;
        }
        if(comp[i] != '+' && comp[i] != '-'){
            cout<<"complex "<<comp<<endl;
            cout<<"the real part is 0"<<endl;
            cout<<"and the imaginary part is ";
            if(comp[0]=='i') cout<<"1";
            else if (comp[0]=='-' && comp[1]=='i') cout<<"-1";
            else cout<<comp.substr(0,comp.length()-1);
        }
        else{
            cout<<"complex "<<comp<<endl;
            cout<<"the real part is "<<comp.substr(0,i)<<endl;
            cout<<"and the imaginary part is ";
            if(comp[i]=='+'&&comp[i+1]=='i') cout<<"1";
            else if(comp[i]=='-'&&comp[i+1]=='i') cout<<"-1";
            else{
                if(comp[i] == '-') cout<<"-";
                cout<<comp.substr(i+1,comp.length()-i-2);
            }
        }
    }
    return 0;
}
```

## 学生CPP成绩计算

### 题目概述

给出下面的人员基类框架：

```
class Person {
protected:
     string name;
     int age;
public:
     Person();    
     Person (string p_name, int p_age);
     void display () {cout<<name<<“:”<<age<<endl;}
};
```

建立一个派生类student,增加以下成员数据：

```
int ID;//学号
float cpp_score;//cpp上机成绩
float cpp_count;//cpp上机考勤
float cpp_grade;//cpp总评成绩
     //总评成绩计算规则：cpp_grade = cpp_score * 0.9 + cpp_count * 2;
```

增加以下成员函数：

```
student类的无参构造函数
student类的参数化构造函数//注意cpp_grade为上机成绩和考勤的计算结果
void print()//输出当前student的信息
                 //其中cpp_grade输出保留一位小数
                //输出格式为ID name cpp_grade
```

生成上述类并编写主函数，根据输入的学生基本信息，建立一个学生对象，计算其cpp总评成绩，并输出其学号、姓名、总评成绩。

### 函数接口定义

```
输入格式： 测试输入包含若干测试用例，每个测试用例占一行（学生姓名 学号 年龄 cpp成绩 cpp考勤）。当读入0时输入结束，相应的结果不要输出。
```

### 输入样例

```
Bob 10001 18 75.5 4
Mike 10005 17 95.0 5
0
```

### 输出样例

```
10001 Bob 75.9
10005 Mike 95.5
```

### 代码

```
#include <iostream>
#include <iomanip>
using namespace std;

class Person {
protected:
    string name;
    int age;
public:
    Person();
    Person (string p_name, int p_age);
    void display () {cout<<name<<":"<<age<<endl;}
};

Person::Person():name("0"),age(0){}
Person::Person(string p_name, int p_age):name(p_name),age(p_age) {}

class Student:public Person{
protected:
    int ID;//学号
    float cpp_score;//cpp上机成绩
    float cpp_count;//cpp上机考勤
    float cpp_grade;//cpp总评成绩
public:
    Student():Person(),ID(0),cpp_score(0),cpp_grade(0){ cpp_grade = cpp_score * 0.9 + cpp_count * 2; }
    Student(string s_name, int s_id, int s_age, float s_score, float s_count):Person(s_name,s_age),ID(s_id),cpp_score(s_score),cpp_count(s_count){ cpp_grade = cpp_score * 0.9 + cpp_count * 2; }
    void print(){
        cout<<ID<<" "<<name<<" ";
        cout.setf( ios::fixed );
        cout<<setprecision(1)<<cpp_grade<<endl; }
};

int main(){
    string name;
    int ID;
    int age;
    float cpp_score;
    float cpp_count;
    cin>>name;
    while(name!="0"){
        cin>>ID>>age>>cpp_score>>cpp_count;
        Student stu(name,ID,age,cpp_score,cpp_count);
        stu.print();
        cin>>name;
    }
    return 0;
}
```

## 学生cpp成绩统计

### 题目概述

完成“学生cpp成绩计算”之后，修改Person和Student类，各自增加两个无参构造函数。

仍以Person类为基础，建立一个派生类Teacher，增加以下成员数据：

```
   int ID;//教师工号
   Student stu[100];//学生数组
   int count;//学生数目，最多不超过100
   float cpp_average;//班级cpp平均分
```

增加以下成员函数：

```
  Teacher类的参数化构造函数
  void Add (Student & stu1)//在学生数组中增加一个学生记录
  void average();//计算当前班级cpp平均成绩cpp_average
  void print()//输出当前班级学生的信息
           //其中学生记录中cpp_score和cpp_grade输出保留一位小数
                  //当前班级cpp_average输出保留一位小数；
                  //输出格式如下：
             //第一行：教师工号 教师姓名 班级学生数 cpp_average
       //第二行至第count+1行每一行输出一个学生的信息，每一行格式
      // 学生学号 学生姓名 cpp_grade
     //cpp_grade保留一位小数
```

生成上述类并编写主函数，根据输入的教师基本信息，建立一个教师对象，根据输入的每一条学生基本信息，建立一个学生对象，计算学生cpp总评成绩并且加入学生数组中，由教师对象计算班级cpp平均成绩，并输出班级学生的全部信息。

### 函数接口定义

```
输入格式： 测试输入包含一个测试用例，该测试用例的第一行输入教师的基本信息（教师姓名 教师工号 年龄），第二行开始输入班级内学生信息，每个学生基本信息占一行（学生姓名 学号 年龄 cpp成绩 cpp考勤），最多不超过100行，当读入0时输入结束。
```

### 输入样例

```
Mike 901 30
Bob 10001 18 75.9 4
Anna 10003 19 87.0 5
0
```

### 输出样例

```
901 Michale 2 82.3
10001 Bob 76.3
10003 Anna 88.3
```

### 代码

```
#include <iostream>
#include <iomanip>
using namespace std;

class Person {
protected:
    string name;
    int age;
public:
    Person();
    Person (string p_name, int p_age);
    void display () {cout<<name<<":"<<age<<endl;}
};

Person::Person():name("0"),age(0){}
Person::Person(string p_name, int p_age):name(p_name),age(p_age) {}

class Student:public Person{
public:
    int ID;//学号
    float cpp_score;//cpp上机成绩
    float cpp_count;//cpp上机考勤
    float cpp_grade;//cpp总评成绩
    Student():Person(),ID(0),cpp_score(0),cpp_grade(0){ cpp_grade = cpp_score * 0.9 + cpp_count * 2; }
    Student(string s_name, int s_id, int s_age, float s_score, float s_count):Person(s_name,s_age),ID(s_id),cpp_score(s_score),cpp_count(s_count){ cpp_grade = cpp_score * 0.9 + cpp_count * 2; }
    void print(){
        cout<<ID<<" "<<name<<" ";
        cout.setf( ios::fixed );
        cout<<setprecision(1)<<cpp_grade<<endl; }
};

class Teacher:public Person{
protected:
    int ID;//教师工号
    Student stu[100];//学生数组
    int count=0;//学生数目，最多不超过100
    float cpp_average;//班级cpp平均分
public:
    Teacher(string t_name,int t_id,int t_age):Person(t_name,t_age),ID(t_id){}
    void Add (Student & stu1){
        stu[count++]=stu1;
    }
    void average(){
        double sum;
        for(int i=0;i<count;i++)
            sum += stu[i].cpp_grade;
        cpp_average = sum/count;
    }
    void print(){
        cout<<ID<<" "<<name<<" "<<count<<" ";
        cout.setf( ios::fixed );
        cout<<setprecision(1)<<cpp_average<<endl;
        for(int i=0;i<count;i++)
            stu[i].print();
    }
};

int main(){
    string name;
    int ID;
    int age;
    float cpp_score;
    float cpp_count;
    cin>>name>>ID>>age;
    Teacher t(name,ID,age);
    cin>>name;
    while(name!="0"){
        cin>>ID>>age>>cpp_score>>cpp_count;
        Student stu1(name,ID,age,cpp_score,cpp_count);
        t.Add(stu1);
        cin>>name;
    }
    t.average();
    t.print();
    return 0;
}
```

## 多边形周长计算（继承）

### 题目概述

给出下面的多边形基类框架：

```
class polygon
{ protected:
   int number;//边数，最多不超过100条边
private:
   int side_length[100];//边长数组
public:
   polygon();//构造函数根据需要重载
   int perimeter();//计算多边形边长
   void display();//输出多边形边数和周长
}
```

建立一个派生类rectangle(矩形)，增加以下数据成员：

```
  int height;
  int width;
```

增加以下成员函数：

```
 rectangle类的无参和参数化构造函数
 int perimeter();//计算矩形边长
 void display();//输出多边形边数和周长
```

建立一个派生类equal_polygon(等边多边形)，增加以下数据成员：

```
  int side_len;
```

增加以下成员函数：

```
 equal_polygon类的无参和参数化构造函数
 int perimeter();//计算等边多边形边长
 void display();//输出多边形边数和周长
```

生成上述类并编写主函数，根据输入的多边形信息，相应建立一个多边形类对象或矩形类对象或等边多边形类对象，计算每一个多边形的周长并且输出其边数和周长。

### 函数接口定义

```
输入格式： 测试输入包含一个测试用例，该测试用例的第一行输入多边形的个数n，接下来n行每一行给出一个多边形的基本信息，每行的第一个数字为当前多边形的类型，0为一般多边形，后面跟随m个数字为m条边的边长，-1为一般多边形边长输入结束标志，1为矩形，后面跟随两个数字，分别为height和width，2为等边多边形，后面跟随两个数字为等边多边形的边数和边长。
```

### 输入样例

```
3
0 32 54 76 88 24 -1
1 32 54
2 3 32
```

### 输出样例

```
5 274
4 172
3 96
```

### 代码

```
#include<iostream>
#include<string>
using namespace std;

class polygon
{ 
protected:
    int number;//边数，最多不超过100条边
private:
    int side_length[100];//边长数组
public:
    polygon(int num):number(num){}
    polygon(){}//构造函数根据需要重载
    void set_side(int s,int k)//设置边长
    {
        side_length[k]=s;
    }
    void set_number(int num)//设置边数
    {
        number=num;
    }
    int perimeter();//计算多边形周长
    void display();//输出多边形边数和周长
};

int polygon::perimeter()
{
    int sum(0);
    for(int i(0);i<number;i++)
    {
        sum+=side_length[i];//所有边相加
    }
    return sum;
}
void polygon::display()
{
    cout<<number<<' '<<perimeter()<<endl;//调用成员函数计算周长
}


class rectangle:public polygon//定义矩形类，继承多边形类
{
private:
    int height,width;
public:
    rectangle(int h,int w,int n):polygon(n)//构造函数赋值
    {
        height=h,width=w,number=n;
    }
    int perimeter()//计算矩形的周长
    {
        int sum(0);
        sum=(height+width)*2;
        return sum;
    }
    void display()//输出结果
    {
        cout<<number<<' '<<perimeter()<<endl;//调用成员函数
    }
};

class equal_polygon:public polygon//定义正多边形类，继承多边形类
{
private:
    int side_len;
public:
    equal_polygon(int s,int n):polygon(n)//构造函数赋值
    {
        number=n,side_len=s;
    }
    int perimeter()//计算周长
    {
        int sum(0);
        sum=side_len*number;
        return sum;
    }
    void display()//输出结果
    {
        cout<<number<<' '<<perimeter()<<endl;
    }
};

int main()
{    
    void ispolygon(),isrectangle(),isequal_polygon();//证明函数，在主函数后定义
    int a,i(0);
    cin>>a;//输入a表示图形个数
    do
    {
        int k;
        cin>>k;
        switch(k)
        {
        case 0:ispolygon();break;//多边形执行ispolygon()
        case 1:isrectangle();break;//矩形
        case 2:isequal_polygon();break;//正多边形
        }
    }
    while(i++,--a);//do{}while(i++,--a)等价于for(int i(0);i<a;i++)
    return 0;
}
void ispolygon()
{
    int a,i(0);
    polygon poly;
    while(cin>>a)
    {
        if(a!=-1) //输入不为-1则继续输入
        {
            poly.set_side(a,i);//poly对象设置边长
            i++;
        }
        else
            break;//输入为-1则停止输入
    }
    poly.set_number(i);//设置边数
    poly.display();//输出结果
}
void isrectangle()
{
    int a,b;
    cin>>a>>b;
    rectangle rect(a,b,4);//对象实体化类，赋初值
    rect.display();//输出结果
}
void isequal_polygon()
{
    int s,n;
    cin>>n>>s;
    equal_polygon equa(s,n);//实体化类
    equa.display();//输出结果
}
```

## 点到原点的距离（继承）

### 题目概述

给出下面的一个基类框架：

```
class Point_1D
{ protected:
float x;//1D 点的x坐标
public:
Point_1D(float p = 0.0);
float distance( );//计算当前点到原点的距离
}
```

以Point_1D为基类建立一个派生类Point_2D，增加一个保护数据成员：

```
float y;//2D平面上点的y坐标
以Point_2D为直接基类再建立一个派生类Point_3D，增加一个保护数据成员：
float z;//3D立体空间中点的z坐标
```

生成上述类并编写主函数，根据输入的点的基本信息，建立点对象，并能计算该点到原点的距离。

### 函数接口定义

```
输入格式： 测试输入包含若干测试用例，每个测试用例占一行（点的类型（1表示1D点，2表示2D点，3表示3D点） 第一个点坐标信息（与点的类型相关） 第二个点坐标信息（与点的类型相关））。当读入0时输入结束，相应的结果不要输出。
```

### 输入样例

```
1 -1
2 3 4
3 1 2 2
0
```

### 输出样例

```
Distance from Point -1 to original point is 1
Distance from Point(3,4) to original point is 5
Distance from Point(1,2,2) to original point is 3
```

### 代码

```
#include <iostream>
#include <cmath>
using namespace std;

class Point_1D{
protected:
    float x;//1D 点的x坐标
public:
    Point_1D(float p = 0.0):x(p){}
    float distance( ){ return abs(x); }//计算当前点到原点的距离
};

class Point_2D:public Point_1D{
protected:
    float y;
public:
    Point_2D(float p=0.0,float q=0.0):Point_1D(p),y(q){}
    float distance(){ return pow(pow(x,2)+pow(y,2),0.5); }
};

class Point_3D:public Point_2D{
protected:
    float z;
public:
    Point_3D(float p=0.0,float q=0.0,float r=0.0):Point_2D(p,q),z(r){}
    float distance(){ return pow(pow(x,2)+pow(y,2)+pow(z,2),0.5); }
};

int main(){
    int dn;
    float x,y,z;
    cin>>dn;
    while(dn!=0){
        switch(dn) {
            case 1: {
                cin >> x;
                Point_1D obj(x);
                cout<<"Distance from Point "<<x<<" to original point is "<<obj.distance()<<endl;
                break;
            }
            case 2: {
                cin >> x >> y;
                Point_2D obj(x, y);
                cout<<"Distance from Point("<<x<<","<<y<<") to original point is "<<obj.distance()<<endl;
                break;
            }
            case 3: {
                cin >> x >> y >> z;
                Point_3D obj(x, y, z);
                cout<<"Distance from Point("<<x<<","<<y<<","<<z<<") to original point is "<<obj.distance()<<endl;
                break;
            }
        }
        cin>>dn;
    }
}
```

## 时间模拟

### 题目概述

给出下面的基类Time的框架如下：

```
class Time
{
protected:
    int second;
    int minute;
    int hour;
public:
     void operator++();
     void operator--();
}
```

建立一个派生类Time_12hours，用于表示十二进制时间，增加以下成员数据：

```
string type；//标识为12进制时间，type=”12-hours-time”
string interval;//标识为AM或者PM，interval=”AM”或interval=”PM”
```

增加以下成员函数：

```
void operator++();
void operator--();
```

建立一个派生类Time_24hours，用于表示二十四进制时间，增加以下成员数据：

```
     string type；//标识为24进制时间，type=”24-hours-time”
```

增加以下成员函数：

```
void operator++();
void operator--();
```

生成上述类并编写主函数，根据输入的初始时间信息、自增或者自减类型、自增或者自减次数，输出其最后时间信息。

### 函数接口定义

```
输入格式：测试输入包含多个测试用例，一个测试用例为一行，每行共五个数字，第一个数字为进制，121表示输入为12进制AM时间，122表示输入为12进制PM时间，输入为24表示输入为24进制时间，第二个数字为hour，第三个数字为minute，第四个数字为second，第五个字符为运算类型，+表示自增，-表示自减，第六个数字为运算次数，0表示测试用例结束。
```

### 输入样例

```
121 11 59 59 + 3
24 11 59 59 + 3
122 11 59 59 + 3
122 00 00 00 - 3
121 00 00 00 - 5
24 00 00 00 - 1
0
```

### 输出样例

```
PM 00:00:02
12:00:02
AM 00:00:02
AM 11:59:57
PM 11:59:55
23:59:59
```

### 代码

```
#include <iostream>
#include <iomanip>
using namespace std;

class Time{
protected:
    int second;
    int minute;
    int hour;
public:
    Time(int h,int m,int s):hour(h),minute(m),second(s){}
    void operator++(){
        second++;
        if(second>=60){ second-=60;minute++; }
        if(minute>=60){ minute-=60;hour++; }
    }
    void operator--(){
        second--;
        if(second<0){ minute--;second+=60; }
        if(minute<0){ hour--;minute+=60; }
    }
};

class Time_12hours:public Time{
protected:
    string type;//标识为12进制时间，type=”12-hours-time”
    string interval;//标识为AM或者PM，interval=”AM”或interval=”PM”
public:
    Time_12hours(string in,int h,int m,int s):type("12-hours-time"),interval(in),Time(h,m,s){}
    void operator++(){
        Time::operator++();
        if(hour>=12){
            hour-=12;
            if(interval=="AM") interval="PM";
            else if (interval=="PM") interval="AM";
        }
    }
    void operator--(){
        Time::operator--();
        if(hour<0){
            hour+=12;
            if(interval=="AM") interval="PM";
            else if (interval=="PM") interval="AM";
        }
    }
    void print(){ cout<<interval<<" "<<setw(2)<<setfill('0')<<hour<<":"<<setw(2)<<setfill('0')<<minute<<":"<<setw(2)<<setfill('0')<<second<<endl; }
};

class Time_24hours:public Time{
protected:
    string type;//标识为24进制时间，type=”24-hours-time”
public:
    Time_24hours(int h,int m,int s):type("24-hours-time"),Time(h,m,s){}
    void operator++(){
        Time::operator++();
        if(hour>=24) hour-=24;
    }
    void operator--(){
        Time::operator--();
        if(hour<0) hour+=24;
    }
    void print(){ cout<<setw(2)<<setfill('0')<<hour<<":"<<setw(2)<<setfill('0')<<minute<<":"<<setw(2)<<setfill('0')<<second<<endl; }
};

int main(){
    int type;
    int hour, minute, second;
    char ch;
    int i,num;
    cin>>type;
    while(type!=0){
        cin>>hour>>minute>>second>>ch>>num;
        switch(type){
            case 121:{
                Time_12hours tim("AM",hour,minute,second);
                for(i=0;i<num;i++)
                    if(ch=='+') tim.operator++();
                    else if(ch=='-') tim.operator--();
                tim.print();
                break;
            }
            case 122:{
                Time_12hours tim("PM",hour,minute,second);
                for(i=0;i<num;i++)
                    if(ch=='+') tim.operator++();
                    else if(ch=='-') tim.operator--();
                tim.print();
                break;
            }
            case 24:{
                Time_24hours tim(hour,minute,second);
                for(i=0;i<num;i++)
                    if(ch=='+') tim.operator++();
                    else if(ch=='-') tim.operator--();
                tim.print();
                break;
            }
        }
        cin>>type;
    }
}
```

## 两点间距离计算

### 题目概述

给出下面的一个基类框架：

```
class Point_1D
{ protected:
float x;//1D 点的x坐标
public:
Point_1D(float p = 0.0);
float distance(const Point_1D & p2);
}
```

以Point_1D为基类建立一个派生类Point_2D，增加一个保护数据成员：

```
float y;//2D平面上点的y坐标
以Point_2D为直接基类再建立一个派生类Point_3D，增加一个保护数据成员：
float z;//3D立体空间中点的z坐标
```

生成上述类并编写主函数，根据输入的点的基本信息，建立点对象，并能计算该点到原点的距离。

### 函数接口定义

```
输入格式： 测试输入包含若干测试用例，每个测试用例占一行（点的类型（1表示1D点，2表示2D点，3表示3D点） 第一个点坐标信息（与点的类型相关） 第二个点坐标信息（与点的类型相关））。当读入0时输入结束，相应的结果不要输出。
```

### 输入样例

```
1 -1 0
2 3 4 0 0
3 1 2 2 0 0 0
0
```

### 输出样例

```
Distance from Point -1 to Point 0 is 1
Distance from Point(3,4) to Point(0,0) is 5
Distance from Point(3,3,3) to Point(0,0,0) is 3
```

### 代码

```
#include <iostream>
#include <cmath>
using namespace std;

class Point_1D{
protected:
    float x;//1D 点的x坐标
public:
    Point_1D(float p = 0.0):x(p){}
    float distance(const Point_1D & p2){ return abs(x-p2.x); }//计算当前点到原点的距离
};

class Point_2D:public Point_1D{
protected:
    float y;
public:
    Point_2D(float p=0.0,float q=0.0):Point_1D(p),y(q){}
    float distance(const Point_2D & p2){ return pow(pow(x-p2.x,2)+pow(y-p2.y,2),0.5); }
};

class Point_3D:public Point_2D{
protected:
    float z;
public:
    Point_3D(float p=0.0,float q=0.0,float r=0.0):Point_2D(p,q),z(r){}
    float distance(const Point_3D & p2){ return pow(pow(x-p2.x,2)+pow(y-p2.y,2)+pow(z-p2.z,2),0.5); }
};

int main(){
    int dn;
    float x,y,z;
    float x2,y2,z2;
    cin>>dn;
    while(dn!=0){
        switch(dn) {
            case 1: {
                cin >> x >> x2;
                Point_1D obj(x),obj2(x2);
                cout<<"Distance from Point "<<x<<" to Point "<<x2<<" is "<<obj.distance(obj2)<<endl;
                break;
            }
            case 2: {
                cin >> x >> y>>x2>>y2;
                Point_2D obj(x, y),obj2(x2,y2);
                cout<<"Distance from Point("<<x<<","<<y<<") to Point("<<x2<<","<<y2<<") is "<<obj.distance(obj2)<<endl;
                break;
            }
            case 3: {
                cin >> x >> y >> z>>x2>>y2>>z2;
                Point_3D obj(x, y, z),obj2(x2,y2,z2);
                cout<<"Distance from Point("<<x<<","<<y<<","<<z<<") to Point("<<x2<<","<<y2<<","<<z2<<") is "<<obj.distance(obj2)<<endl;
                break;
            }
        }
        cin>>dn;
    }
}
```

## 车辆选择（继承）

### 题目概述

有一个汽车类vehicle，它具有一个需传递参数的构造函数，汽车类vehicle中的数据成员为： 车轮个数wheels和车重weight放在保护段中，汽车类vehicle中的公有成员函数为：get_wheels()（返回车轮个数的值）、get_weight()（返回车重的值）、wheel_load()（返回每个轮胎的载重量的值：weight/wheels）、print()（输出车轮的个数和车重的公斤数）；

小车类car是vehicle类的派生类，它具有一个需传递参数的构造函数，小车类car中的私有数据成员为：车载人数passenger_load，小车类car中的公有成员函数为：get_passengers()（返回车载人数的值）、print()（输出小车车轮的个数和车重的公斤数以及车载人数的个数）；

卡车类truck是vehicle类的派生类，它具有一个需传递参数的构造函数，卡车类truck中的私有数据成员为：载人数passenger_load和载重量payload，卡车类truck中的公有成员函数为：get_passengers()（返回车载人数的值）、efficiency（）(返回卡车的载重效率的值：payload/(payload+weight)、print()（输出卡车车轮的个数和车重的公斤数以及车载人数的个数和卡车的载重效率的值）)。

生成上述类并编写主函数，根据输入的车辆基本信息，建立车辆对象，并能计算输出该车辆的基本信息。

### 函数接口定义

```
输入格式：测试输入包含一个测试用例，每一行给出一个车辆的基本信息，每行的第一个字符处为当前车辆的类型，第二个数字为当前车辆的编号，若车辆为vehicle，后面跟随两个数字分别为wheels和weight，若车辆为car，后面跟随三个数字分别为wheels，weight和车载人数，若车辆为truck，后面跟随四个数字分别是wheels，weight、车载人数和载重量。（以上数字均为整型）。-1表示输入结束，相应结果不要输出。请注意输出格式，按照输入顺序进行编号 说明：本题中轮胎载重量、载重效率若需输出保留小数点后两位。
```

### 输入样例

```
vehicle 101 4 1900
car 201 4 2000 5
truck 301 6 3000 2 9000
car 202 4 1800 4
-1
```

### 输出样例

```
The 1st object is Vehicle No. 101: weight 1900 Kg and wheels 4
The 2nd object is Car No. 201: passenger_load 5 weight 2000 Kg and wheels 4
The 3rd object is Truck No. 301: passenger_load 2 weight 3000 Kg wheels 6 and efficiency 0.75
The 4th object is Car No. 202: passenger_load 4 weight 1800 Kg and wheels 4
```

### 代码

```
#include <iostream>
#include <iomanip>
using namespace std;

class Vehicle
{
protected:
    int wheels;
    float weight;
public:
    Vehicle(int wheels,float weight){
        this->wheels = wheels;
        this->weight = weight;
    };
    int get_wheels(){ return wheels; }
    float get_weight(){ return weight; }
    float wheel_load(){ return weight/wheels; }
    void print(){
        cout<<"weight "<<weight<<" Kg and wheels "<<wheels<<endl;
    }
};

class Car:public Vehicle{
private:
    int passenger_load;
public:
    Car(int wheels,float weight,int load):Vehicle(wheels,weight),passenger_load(load){};
    int get_passengers(){ return passenger_load; };
    void print(){
        cout<<"passenger_load "<<passenger_load<<" weight "<<weight<<" Kg and wheels "<<wheels<<endl;
    }
};

class Truck:public Vehicle{
private:
    int passenger_load;
    float payload;
public:
    Truck(int wheels,float weight,int load,float pload):Vehicle(wheels,weight),passenger_load(load),payload(pload){}
    int get_passengers(){ return passenger_load; };
    float efficiency(){ return payload/(payload+weight); }
    void print(){
        cout<<"passenger_load "<<passenger_load<<" weight "<<weight<<" Kg wheels "<<wheels<<" and efficiency "<<fixed<<setprecision(2)<<efficiency()<<endl;
        cout<<fixed<<setprecision(0);
    }
};

int main ()
{
    string type, strn;
    int count=0;
    int no, wheels, load;
    float weight, payload;
    cin>>type;
    while(type!="-1"){
        cin>>no>>wheels>>weight;

        if(type=="vehicle"){}
        else if (type=="car"){ cin>>load; }
        else if (type=="truck"){ cin>>load>>payload; }

        cout<<"The "<<++count;
        strn = to_string(count);
        if( (strn.length()>=2 && strn[strn.length()-2]!='1') || strn.length()==1 ){
            if(strn[strn.length()-1]=='1') cout<<"st";
            else if(strn[strn.length()-1]=='2') cout<<"nd";
            else if(strn[strn.length()-1]=='3') cout<<"rd";
            else cout<<"th";
        }else cout<<"th";
        type[0]=type[0]-'a'+'A';
        cout<<" object is "<<type<<" No. "<<no<<": ";

        if(type=="Vehicle"){
            Vehicle veh(wheels,weight);
            veh.print();
        } else if (type=="Car"){
            Car car(wheels,weight,load);
            car.print();
        } else if (type=="Truck"){
            Truck tru(wheels,weight,load,payload);
            tru.print();
        }

        cin>>type;
    }
}
```

## 日程安排（多重继承+重载）

### 题目概述

已有一个日期类Date，包括三个protected成员数据

```
int year;
int month;
int day;
```

另有一个时间类Time，包括三个protected成员数据
```
int hour;
int minute;
int second;
```

现需根据输入的日程的日期时间，安排前后顺序，为此以Date类和Time类为基类，建立一个日程类Schedule，包括以下新增成员：

```
int ID；//日程的ID
bool operator < (const Schedule & s2);//判断当前日程时间是否早于s2
```

生成以上类，并编写主函数，根据输入的各项日程信息，建立日程对象，找出需要最早安排的日程，并输出该日程对象的信息。

### 函数接口定义

```
输入格式： 测试输入包含若干日程，每个日程占一行（日程编号ID 日程日期（**//）日程时间（::））。当读入0时输入结束，相应的结果不要输出。
```

### 输入样例

```
1 2014/06/27 08:00:01
2 2014/06/28 08:00:01
0
```

### 输出样例

```
The urgent schedule is No.1: 2014/6/27 8:0:1
```

### 代码

```
#include <iostream>
using namespace std;

class Date{
protected:
    int year;
    int month;
    int day;
public:
    Date(int ye,int mo,int da):year(ye),month(mo),day(da){}
};

class Time{
protected:
    int hour;
    int minute;
    int second;
public:
    Time(int ho,int mi,int se):hour(ho),minute(mi),second(se){}
};

class Schedule:public Date,public Time{
protected:
    int ID;
public:
    Schedule(int id,int ye,int mo,int da,int ho,int mi,int se):ID(id),Date(ye,mo,da),Time(ho,mi,se){}
    bool operator < (const Schedule & s2){
        if (year<s2.year) return true;
        else if (year>s2.year) return false;
        else{
            if (month<s2.month) return true;
            else if (month>s2.month) return false;
            else{
                if (day<s2.day) return true;
                else if (day>s2.day) return false;
                else{
                    if (hour<s2.hour) return true;
                    else if (hour>s2.hour) return false;
                    else{
                        if (minute<s2.minute) return true;
                        else if (minute>s2.minute) return false;
                        else{
                            if (second<s2.second) return true;
                            else return false;
                        }
                    }
                }
            }
        }
    }

    void print(){ cout<<"The urgent schedule is No."<<ID<<": "<<year<<"/"<<month<<"/"<<day<<" "<<hour<<":"<<minute<<":"<<second<<endl; }
};

int main(){
    int ID;
    int year, month, day;
    int hour, minute, second;
    char ch1,ch2,ch3,ch4;
    Schedule urgent(0,9999,0,0,0,0,0);

    cin>>ID;
    while(ID!=0){
        cin>>year>>ch1>>month>>ch2>>day>>hour>>ch3>>minute>>ch4>>second;
        Schedule sche(ID,year,month,day,hour,minute,second);
        if(sche<urgent) urgent=sche;
        cin>>ID;
    }
    urgent.print();
}
```

## 体育俱乐部积分管理

### 题目概述

一个俱乐部有篮球、足球和排球队。
下面给出基类的框架：

```
class Ball
{
protected:
string opponent;//对手
public:
void display();//显示比赛成绩
}
```

以Ball为基类，构建Basketball, Football和Volleyball三个类。

生成上述类并编写主函数，要求主函数中有一个基类Ball指针数组，数组元素不超过20个。

Ball *pb[20];

主函数根据输入的信息，相应建立Basketball, Football, Volleyball类对象。

### 函数接口定义

```
输入格式：每个测试用例占一行，第一项为类型，1为Basketball，2为Football，3为Volleyball, 第二项是对手名称，第二项是比分。输入0表示输入的结束。
输出时，依次打印对手名称和该场积分数（以万元为单位）：Basketball胜一场得2分，负一场得1分；Football胜一场得3分，平一场得1分；排球以3：0或3：1胜时，得3分，以3：2胜时，得2分，以2：3负时，得1分，其它情况得0分。
```

### 输入样例

```
1 AAA 108:106
2 BB 2:1
3 CCC 3:2
2 BB 2:2
3 EEE 3:1
3 FFF 2:3
3 CCC 0:3
1 AAA 95:99
0
```

### 输出样例

```
AAA 2
BB 3
CCC 2
BB 1
EEE 3
FFF 1
CCC 0
AAA 1
```

### 代码

```
#include <iostream>
using namespace std;

class Ball{
protected:
    string opponent;//对手
    int s1,s2;
    int score=0;
public:
    Ball(string op,int ss1,int ss2):opponent(op),s1(ss1),s2(ss2){}
    void display(){ cout<<opponent<<" "<<score<<endl; }
};

class Basketball:public Ball{
public:
    Basketball(string op,int ss1,int ss2):Ball(op,ss1,ss2){
        if(s1>s2){ score = 2; }
        else if(s1<s2){ score = 1; }
    };

};

class Football:public Ball{
public:
    Football(string op,int ss1,int ss2):Ball(op,ss1,ss2){
        if(s1>s2) {score = 3;}
        else if(s1==s2) {score = 1;}
    }
};

class Volleyball:public Ball{
public:
    Volleyball(string op,int ss1,int ss2):Ball(op,ss1,ss2){
        if(s1==3 && s2<=1) {score=3;}
        else if(s1==3 && s2==2) {score=2;}
        else if(s1==2 && s2==3) {score=1;}
        else {score=0;}
    }
};

int main(){
    Ball *pb[20];
    int type;
    string op;
    int s1,s2;
    int i,count=0;
    char ch;

    cin>>type;
    while(type!=0){
        cin>>op>>s1>>ch>>s2;
        switch(type){
            case 1:
                pb[count++] = new Basketball(op,s1,s2);
                break;
            case 2:
                pb[count++] = new Football(op,s1,s2);
                break;
            case 3:
                pb[count++] = new Volleyball(op,s1,s2);
                break;
        }
        cin>>type;
    }
    for(i=0;i<count;i++)
        pb[i]->display();
}
```

## 动物世界

### 题目概述

补充程序 ：

1、实现Mammal类的方法

2、由Mammal类派生出Dog类，在Dog类中增加itsColor成员(COLOR类型)

3、Dog类中增加以下方法：

constructors: Dog()、Dog(int age)、Dog(int age, int weight)、Dog(int age, COLOR color)、 Dog(int age, int weight, COLOR color)、~Dog()

accessors: GetColor()、SetColor()

Other methods: WagTail()、BegForFood() ，并实现以上这些方法 。

提示：类似Speak()、WagTail()这些动作，函数体可以是输出一句话。比如：Mammal is spaeking... , The Dog is Wagging its tail...

4、补充主函数的问号部分，并运行程序，检查输出是否合理。

### 裁判测试程序样例

```
enum COLOR{ WHITE, RED, BROWN, BLACK, KHAKI };

class Mammal
{
    public:
        //constructors
        Mammal();
        Mammal(int age);
        ~Mammal();
        
        //accessors
        int GetAge() const;
        void SetAge(int);
        int GetWeight() const;
        void SetWeight(int);
        
        //Other methods 
        void Speak() const;
        void Sleep() const;     
    protected:
        int itsAge;
        int itsWeight;
};

int main()
{
    Dog Fido;
    Dog Rover(5);
    Dog Buster(6, 8);
    Dog Yorkie(3, RED);
    Dog Dobbie(4, 20, KHAKI);
    Fido.Speak();
    Rover.WagTail();
    cout << "Yorkie is " << ?? << " years old." << endl;
    cout << "Dobbie weighs " << ?? << " pounds." << endl;   
    return 0;
}
```

### 输出样例

```
Mammal is speaking...
The dog is wagging its tail...
Yorkie is 3 years old.
Dobbie weighs 20 pounds.
```

### 代码

```
#include <iostream>
using namespace std;

enum COLOR{ WHITE, RED, BROWN, BLACK, KHAKI };

class Mammal
{
public:
    //constructors
    Mammal():itsAge(0),itsWeight(0){};
    Mammal(int age):itsAge(age),itsWeight(0){};
    Mammal(int age, int weight):itsAge(age),itsWeight(weight){};
//    ~Mammal();

    //accessors
    int GetAge() const{ return itsAge; };
    void SetAge(int age){ itsAge = age; };
    int GetWeight() const{ return itsWeight; };
    void SetWeight(int weight){ itsWeight = weight; };

    //Other methods
    void Speak() const{ cout<<"Mammal is speaking..."<<endl; };
    void Sleep() const{ cout<<"Mammal is sleeping..."<<endl; };
protected:
    int itsAge;
    int itsWeight;
};

class Dog:public Mammal{
public:
    //constructors
    Dog():Mammal(){};
    Dog(int age):Mammal(age){};
    Dog(int age, int weight):Mammal(age,weight){};
    Dog(int age, COLOR color):Mammal(age),itsColor(color){};
    Dog(int age, int weight, COLOR color):Mammal(age,weight),itsColor(color){};
//    ~Dog();

    //accessors
    COLOR GetColor(){ return itsColor; };
    void SetColor(COLOR color){ itsColor = color; };

    //Other methods
    void WagTail(){ cout<<"The dog is wagging its tail..."<<endl; };
    void BegForFood(){ cout<<"The dog is begging for food..."<<endl; };

protected:
    COLOR itsColor;
};

int main()
{
    Dog Fido;
    Dog Rover(5);
    Dog Buster(6, 8);
    Dog Yorkie(3, RED);
    Dog Dobbie(4, 20, KHAKI);
    Fido.Speak();
    Rover.WagTail();
    cout << "Yorkie is " << Yorkie.GetAge() << " years old." << endl;
    cout << "Dobbie weighs " << Dobbie.GetWeight() << " pounds." << endl;
    return 0;
}
```

## 定义基类Point和派生类Circle，求圆的周长

### 题目概述

定义基类Point（点）和派生类Circle（圆），求圆的周长。Point类有两个私有的数据成员float x,y;Circle类新增一个私有的数据成员半径float r和一个公有的求周长的函数getCircumference();主函数已经给出，请编写Point和Circle类。

### 函数接口定义

```
输入格式:
输入圆心和半径，x y r中间用空格分隔。

输出格式:
输出圆的周长，小数点后保留2位有效数字。
```

### 裁判测试程序样例

```
#include <iostream>
#include<iomanip>
using namespace std;
//请编写你的代码
int main()
{
    float x,y,r;
    cin>>x>>y>>r;
    Circle c(x,y,r);
    cout<<fixed<<setprecision(2)<<c.getCircumference()<<endl;
    return 0;
}
```

### 输入样例

```
1 2 3
```

### 输出样例

```
Point constructor called
Circle constructor called
18.84
Circle destructor called
Point destructor called
```

### 代码

```
#include <iostream>
#include <iomanip>
using namespace std;

class Point{
private:
    float x,y;
public:
    Point(float x1, float y1):x(x1),y(y1){ cout<<"Point constructor called"<<endl; }
    ~Point(){ cout<<"Point destructor called"<<endl; }
};

class Circle:public Point{
private:
    float r;
public:
    Circle(float x1,float y1,float r1):Point(x1,y1),r(r1){ cout<<"Circle constructor called"<<endl; }
    ~Circle(){ cout<<"Circle destructor called"<<endl; }
    float getCircumference(){ return 3.14*2*r; }
};

int main()
{
    float x,y,r;
    cin>>x>>y>>r;
    Circle c(x,y,r);
    cout<<fixed<<setprecision(2)<<c.getCircumference()<<endl;
    return 0;
}
```

## 师生信息管理

### 题目概述

给出下面的一个基类框架

```
class Person
{
protected:
int NO;//编号
public:
virtual void display()=0;//输出相关信息
}
```

以Person为基类，构建出Student、Teacher两个类。

生成上述类并编写主函数，要求主函数中有一个基类Person指针数组，数组元素不超过10个。

Person *pp[10];

主函数根据输入的信息，相应建立Student, Teacher类对象，对于Student给出期末5门课的成绩（为整数，缺考的科目填-1）， 对于Teacher则给出近3年，每年发表的论文数量。

### 函数接口定义

```
输入格式：每个测试用例占一行，第一项为人员类型，1为Student,2为Teacher.接下来为编号（0-9999），接下来Student是5门课程成绩，Teacher是3年的论文数。最后一行为0，表示输入的结束。
要求输出编号，以及Student缺考的科目数和已考科目的平均分(保留1位小数，已考科目数为0时，不输出平均分)，Teacher的3年论文总数。
提示：应用虚函数实现多态
```

### 输入样例

```
1 19 -1 -1 -1 -1 -1
1 125 78 66 -1 95 88
2 68 3 0 7
2 52 0 0 0
1 6999 32 95 100 88 74
0
```

### 输出样例

```
19 5
125 1 81.8
68 10
52 0
6999 0 77.8
```

### 代码

```
#include <iostream>
#include <iomanip>
using namespace std;

class Person{
protected:
    int NO;//编号
public:
    Person(int no):NO(no){};
    virtual void display()=0;//输出相关信息
};

class Student:public Person{
protected:
    double tscore=0;
    double count=0;
public:
    Student(int no,int s1,int s2,int s3,int s4,int s5):Person(no){
        if(s1!=-1){ tscore+=s1; count++; }
        if(s2!=-1){ tscore+=s2; count++; }
        if(s3!=-1){ tscore+=s3; count++; }
        if(s4!=-1){ tscore+=s4; count++; }
        if(s5!=-1){ tscore+=s5; count++; }
    }
    void display(){
        cout<<NO<<" "<<5-count;
        if(count>0){
            cout<<" "<<fixed<<setprecision(1)<<tscore/count;
            cout<<fixed<<setprecision(0);
        }
        cout<<endl;
    }
};

class Teacher:public Person{
protected:
    int total;
public:
    Teacher(int no,int s1,int s2,int s3):Person(no){ total = s1+s2+s3; }
    void display(){ cout<<NO<<" "<<total<<endl; }
};

int main(){
    Person *pp[10];
    int type;
    int no;
    int n1,n2,n3,n4,n5;
    int i, count=0;

    cin>>type;
    while(type!=0){
        cin>>no;
        if(type==1){
            cin>>n1>>n2>>n3>>n4>>n5;
            pp[count++]=new Student(no,n1,n2,n3,n4,n5);
        }else if(type==2){
            cin>>n1>>n2>>n3;
            pp[count++]=new Teacher(no,n1,n2,n3);
        }
        cin>>type;
    }

    for(i=0;i<count;i++)
        pp[i]->display();

}
```

## 汽车收费

### 题目概述

现在要开发一个系统，管理对多种汽车的收费工作。 给出下面的一个基类框架

```
class Vehicle
{ protected:
string NO;//编号
public:
virtual void display()=0;//输出应收费用
}
```

以Vehicle为基类，构建出Car、Truck和Bus三个类。

Car的收费公式为： 载客数8+重量2

Truck的收费公式为：重量*5

Bus的收费公式为： 载客数*3

生成上述类并编写主函数，要求主函数中有一个基类Vehicle指针数组，数组元素不超过10个。

Vehicle *pv[10];

主函数根据输入的信息，相应建立Car,Truck或Bus类对象，对于Car给出载客数和重量，Truck给出重量，Bus给出载客数。假设载客数和重量均为整数

### 函数接口定义

```
输入格式：每个测试用例占一行，每行给出汽车的基本信息，每一个为当前汽车的类型1为car，2为Truck，3为Bus。接下来为它的编号，接下来Car是载客数和重量，Truck给出重量，Bus给出载客数。最后一行为0，表示输入的结束。 要求输出各车的编号和收费。

提示：应用虚函数实现多态
```

### 输入样例

```
1 002 20 5
3 009 30
2 003 50
1 010 17 6
0
```

### 输出样例

```
002 170
009 90
003 250
010 148
```

### 代码

```
#include <iostream>
using namespace std;

class Vehicle{
protected:
    string NO;//编号
public:
    Vehicle(string no):NO(no){};
    virtual void display()=0;//输出应收费用
};

class Car:public Vehicle{
protected:
    int load;
    int weight;
public:
    Car(string no,int l,int w):Vehicle(no),load(l),weight(w){};
    void display(){ cout<<NO<<" "<<load*8+weight*2<<endl; };
};

class Truck:public Vehicle{
protected:
    int weight;
public:
    Truck(string no,int w):Vehicle(no),weight(w){};
    void display(){ cout<<NO<<" "<<weight*5<<endl; };
};

class Bus:public Vehicle{
protected:
    int load;
public:
    Bus(string no,int l):Vehicle(no),load(l){};
    void display(){ cout<<NO<<" "<<load*3<<endl; };
};

int main(){
    Vehicle *pv[10];
    int type;
    string no;
    int n1,n2;
    int i, count=0;

    cin>>type;
    while(type!=0){
        cin>>no;
        switch(type){
            case 1:{
                cin>>n1>>n2;
                pv[count++] = new Car(no,n1,n2);
                break;
            }
            case 2:{
                cin>>n1;
                pv[count++] = new Truck(no,n1);
                break;
            }
            case 3:{
                cin>>n1;
                pv[count++] = new Bus(no,n1);
                break;
            }
        }
        cin>>type;
    }

    for(i=0;i<count;i++)
        pv[i]->display();
}
```

## 表彰优秀学生（多态）

### 题目概述

学期结束，班主任决定表彰一批学生，已知该班学生数在6至50人之间，有三类学生：普通生，特招运动员，学科专长生，其中学科专长生不超过5人。

主函数根据输入的信息，相应建立GroupA, GroupB, GroupC类对象。

GroupA类是普通生，有2门课程的成绩（均为不超过100的非负整数）；

GroupB类是特招运动员，有2门课程的成绩（均为不超过100的非负整数），1次运动会的表现分，表现分有：A、B、C、D共4等。

GroupC类是学科专长生，有5门课程的成绩（均为不超过100的非负整数）。

表彰人员至少符合以下3个条件中的一个：

（1）2门课程平均分在普通生和特招运动员中，名列第一者。

a.该平均分称为获奖线。

b.存在成绩并列时，则全部表彰，例如某次考试有2人并列第1，则他们全部表彰。

（2）5门课程平均分达到或超过获奖线90%的学科专长生，给予表彰。

（3）2门课程平均分达到或超过获奖线70%的特招运动员，如果其运动会表现分为A，给予表彰。

### 函数接口定义

```
输入格式：每个测试用例占一行，第一项为类型，1为普通生，2为特招运动员，3为学科专长生, 输入0表示输入的结束。第二项是学号，第三项是姓名。对于普通生来说，共输入5项，第4、5项是课程成绩。对于特招运动员来说，共输入6项，第4、5项是课程成绩，第6项是运动会表现。对于学科专长生来说，共输入8项，第4、5、6、7、8项是课程成绩。

输出时，打印要表彰的学生的学号和姓名。(输出顺序与要表彰学生的输入前后次序一致)
```

### 裁判测试程序样例

```
#include<iostream>
#include <string>
using namespace std;

/* 请在这里填写答案 */

int main()
{
    const int Size=50;
    string num, name;
    int i,ty,s1,s2,s3,s4,s5;
    char gs;
    Student *pS[Size];
    int count=0;
    for(i=0;i<Size;i++){
        cin>>ty;
        if(ty==0) break;
        cin>>num>>name>>s1>>s2;
        switch(ty){
             case 1:pS[count++]=new GroupA(num, name, s1, s2); break;
             case 2:cin>>gs; pS[count++]=new GroupB(num, name, s1,s2, gs); break;
             case 3:cin>>s3>>s4>>s5; pS[count++]=new GroupC(num, name, s1,s2,s3,s4,s5); break;
        }           
    }
    for(i=0;i<count;i++) {
        pS[i]->display();
        delete pS[i];
    }
    return 0;
}
```

### 输入样例

```
1 001 AAAA 96 80
2 009 BBB 82 75 A
1 007 CC 100 99
3 012 CCCC 97 95 90 99 93
1 003 DDD 62 50
1 022 ABCE 78 92
2 010 FFF 45 40 A
3 019 AAA 93 97 94 82 80
0
```

### 输出样例

```
009 BBB
007 CC
012 CCCC
```

### 代码

```
class Student{
protected:
    string num;
    string name;
public:
    static double average;
    Student(string nu,string na):num(nu),name(na){}
    virtual double aver()=0;
    virtual void display()=0;
};

double Student::average=0;

class GroupA:public Student{
protected:
    int s1;
    int s2;
public:
    double aver(){ return (s1+s2)/2; }
    GroupA(string nu,string na,int sc1,int sc2):Student(nu,na),s1(sc1),s2(sc2){
        if(aver()>Student::average)
            Student::average=aver();
    }
    void display(){
        if(aver()==Student::average)
            cout<<num<<" "<<name<<endl;
    }
};

class GroupB:public Student{
protected:
    int s1;
    int s2;
    char gs;
public:
    double aver(){ return (s1+s2)/2; }
    GroupB(string nu,string na,int sc1,int sc2,char g):Student(nu,na),s1(sc1),s2(sc2),gs(g){
        if(aver()>Student::average)
            Student::average=aver();
    }
    void display(){
        if((aver()>=Student::average*0.7 && gs=='A')||aver()==Student::average)
            cout<<num<<" "<<name<<endl;
    }
};

class GroupC:public Student{
protected:
    int s1,s2,s3,s4,s5;
public:
    double aver(){ return (s1+s2+s3+s4+s5)/5; }
    GroupC(string nu,string na,int sc1,int sc2,int sc3,int sc4,int sc5):Student(nu,na),s1(sc1),s2(sc2),s3(sc3),s4(sc4),s5(sc5){}
    void display(){
        if(aver()>=Student::average*0.9)
            cout<<num<<" "<<name<<endl;
    }
};
```

## 虚函数的应用

### 题目概述

补充下列代码。

### 裁判测试程序样例

```
#include <iostream>
using namespace std;
class CMyClassA {
    int val;
public:
    CMyClassA(int);
    void virtual print();
};
CMyClassA::CMyClassA(int arg) {
    val = arg;
    printf("A:%d\n", val);
}
void CMyClassA::print() {
    printf("%d\n", val);
    return;
}

/* 在这里填写代码 */

int main(int argc, char** argv) {
    CMyClassA a(3), *ptr;
    CMyClassB b(5);
    ptr = &a;
    ptr->print();
    a = b;
    a.print();
    ptr = &b;
    ptr->print();
    return 0;
}
```

### 输出样例

```
A:3
A:15
B:5
3
15
5
```

### 代码

```
class CMyClassB:public CMyClassA{
protected:
    int valb;
public:
    CMyClassB(int arg):CMyClassA(arg*3),valb(arg){
        printf("B:%d\n", valb);
    }
    void print(){
        printf("%d\n", valb);
        return;
    }
};
```

## 抽象类Shape

### 题目概述

请编写一个抽象类Shape，包括两个纯虚函数，分别为计算面积getArea()和计算周长getPerim()。通过Shape类派生出矩形类Rectangle和圆类Circle，并计算各自的面积和周长。

测试用例具体要求：输入1表示测试矩形类，之后输入矩形长和宽。输入2表示测试圆类，之后输入圆半径。

### 函数接口定义

```
class Shape {
    public:
        virtual double getArea()=0;
        virtual double getPerim()=0;
};
```

### 裁判测试程序样例

```
#include <iostream>
using namespace std;
const double PI=3.14;

class Shape {
    public:
        virtual double getArea()=0;
        virtual double getPerim()=0;
};

/* ------请在这里填写答案 ------*/

int main() {
    Shape *p;
    int n;
    double w,h,r;
    scanf("%d",&n);
    switch(n) {
        case 1: {
            cin>>w>>h;
            Rectangle rect(w,h);
            cout<<"area="<<rect.getArea()<<endl;
            cout<<"perim="<<rect.getPerim()<<endl;
            break;
        }
        case 2: {
            cin>>r;
            Circle c(r);
            cout<<"area="<<c.getArea()<<endl;
            cout<<"perim="<<c.getPerim()<<endl;
            break;
        }
    }

    return 0;
}
```

### 输入样例

```
1
4 5
```

### 输出样例

```
area=20
perim=18
```

### 输入样例2

```
2
5
```

### 输出样例2

```
area=78.5
perim=31.4
```

### 代码

```
class Rectangle:public Shape{
private:
    double width;
    double height;
public:
    Rectangle(double w,double h):width(w),height(h){};
    double getArea(){ return width*height; };
    double getPerim(){ return 2*(width+height); };
};

class Circle:public Shape{
private:
    double radius;
public:
    Circle(double r):radius(r){};
    double getArea(){ return PI*radius*radius; };
    double getPerim(){ return 2*PI*radius; };
};
```

## 派生类的定义和使用

### 题目概述

按要求完成下面的程序：

1、定义一个Animal类，包含一个void类型的无参的speak方法，输出“animal language!”。

2、定义一个Cat类，公有继承自Animal类，其成员包括：

（1）私有string类型的成员m_strName;

（2）带参数的构造函数，用指定形参对私有数据成员进行初始化；

（3）公有的成员函数print_name，无形参，void类型，功能是输出成员m_strName的值，具体输出格式参见main函数和样例输出。

### 裁判测试程序样例

```
#include <iostream>
#include <string>
using namespace std;

/* 请在这里填写答案 */

int main()
{
    Cat cat("Persian"); //定义派生类对象
    cat.print_name();   //派生类对象使用本类成员函数
    cat.speak();    //派生类对象使用基类成员函数
    return 0;
}
```

### 输出样例

```
cat name: Persian
animal language!
```

### 代码

```
class Animal{
public:
    void speak(){ cout<<"animal language!"<<endl; }
};

class Cat:public Animal{
private:
    string m_strName;
public:
    Cat(string name):m_strName(name){};
    void print_name(){ cout<<"cat name: "<<m_strName<<endl; };
};
```

## 派生类使用基类的成员函数

### 题目概述

按要求完成下面的程序：

1、定义一个Animal类，成员包括：

（1）整数类型的私有数据成员m_nWeightBase，表示Animal的体重；

（2）整数类型的保护数据成员m_nAgeBase，表示Animal的年龄；

（3）公有函数成员set_weight，用指定形参初始化数据成员nWeightBase；

（4）公有成员函数get_weight，返回数据成员nWeightBase的值；

（5）公有函数成员set_age，用指定形参初始化数据成员m_nAgeBase；

2、定义一个Cat类，公有继承自Animal类，其成员包括：

（1）string类型的私有数据成员m_strName;

（2）带参数的构造函数，用指定形参对私有数据成员进行初始化；

（3）公有的成员函数print_age，功能是首先输出成员m_strName的值，然后输出“, age = ”，接着输出基类的数据成员m_nAgeBase的值。具体输出格式参见main函数和样例输出。

### 裁判测试程序样例

```
#include <iostream>
#include <string>
using namespace std;

/* 请在这里填写答案 */

int main()
{
    Cat cat("Persian");   //定义派生类对象cat
    cat.set_age(5);       //派生类对象调用从基类继承的公有成员函数
    cat.set_weight(6);    //派生类对象调用从基类继承的公有成员函数
    cat.print_age();      //派生类对象调用自己的公有函数
    cout << "cat weight = " << cat.get_weight() << endl;
    return 0;
}
```

### 输出样例

```
Persian, age = 5
cat weight = 6
```

### 代码

```
class Animal{
private:
    int m_nWeightBase;
protected:
    int m_nAgeBase;
public:
    void set_weight(int weight){ m_nWeightBase=weight; };
    int get_weight(){ return m_nWeightBase; };
    void set_age(int age){  m_nAgeBase=age; };
};

class Cat:public Animal{
private:
    string m_strName;
public:
    Cat(string name):m_strName(name){};
    void print_age(){ cout<<m_strName<<", age = "<<m_nAgeBase<<endl; };
};
```

## 私有继承派生类使用基类的成员函数

### 题目概述

按要求完成下面的程序：

1、定义一个Animal类，成员包括：

（1）整数类型的私有数据成员m_nWeightBase，表示Animal的体重；

（2）整数类型的保护数据成员m_nAgeBase，表示Animal的年龄；

（3）公有函数成员set_weight，用指定形参初始化数据成员m_nWeightBase；

（4）公有成员函数get_weight，返回数据成员m_nWeightBase的值；

（5）公有函数成员set_age，用指定形参初始化数据成员m_nAgeBase；

2、定义一个Cat类，私有继承自Animal类，其成员包括：

（1）string类型的私有数据成员m_strName;

（2）带参数的构造函数，用指定形参对私有数据成员进行初始化；

（3）公有的成员函数set_print_age，功能是首先调用基类的成员函数set_age设置数据成员m_nAgeBase的值为5，接着输出成员m_strName的值，然后输出“, age = ”，最后输出基类的数据成员m_nAgeBase的值。具体输出格式参见main函数和样例输出。

（4）公有的成员函数set_print_weight，功能是首先调用基类的成员函数set_weight设置数据成员nWeightBase的值为6，接着输出成员m_strName的值，然后输出“, weight = ”，最后调用基类的成员函数get_weight输出基类的数据成员m_nAgeBase的值。具体输出格式参见main函数和样例输出。

### 裁判测试程序样例

```
#include <iostream>
#include <string>
using namespace std;

/* 请在这里填写答案 */

int main()
{
    Cat cat("Persian");     //定义派生类对象cat
    cat.set_print_age();
    cat.set_print_weight(); //派生类对象调用自己的公有函数
    return 0;
}
```

### 输出样例

```
Persian, age = 5
Persian, weight = 6
```

### 代码

```
class Animal{
private:
    int m_nWeightBase;
protected:
    int m_nAgeBase;
public:
    void set_weight(int weight){ m_nWeightBase=weight; };
    int get_weight(){ return m_nWeightBase; };
    void set_age(int age){  m_nAgeBase=age; };
};

class Cat:public Animal{
private:
    string m_strName;
public:
    Cat(string name):m_strName(name){};
    void set_print_age(){
        set_age(5);
        cout<<m_strName<<", age = "<<m_nAgeBase<<endl;
    };
    void set_print_weight(){
        set_weight(6);
        cout<<m_strName<<", weight = "<<get_weight()<<endl;
    };
};
```

## 学生信息输入输出

### 题目概述

根据给出的基类，定义派生类，完成学生信息的输入输出（参见输入输出样例）

### 函数接口定义

```
#include <iostream>
#include <string>
using namespace std;
class Student
{public:
  void get_value()
   {cin>>num>>name>>sex;}
  void display( )
    {cout<<"num: "<<num<<endl;
     cout<<"name: "<<name<<endl;
     cout<<"sex: "<<sex<<endl;}
 private :
   int num;
   string name;
   char sex;
};   

/*在这里添加派生类的定义*/
```

### 裁判测试程序样例

```
int main()
 {Student1 stud1;
  stud1.get_value();
  stud1.get_value_1();
  stud1.display();
  stud1.display_1();
  return 0;
} 
```

### 输入样例

```
1002 Lisi F
20 beijing
```

### 输出样例

```
num: 1002
name: Lisi
sex: F
age: 20
address: beijing
```

### 代码

```
class Student1:public Student{
public:
    void get_value_1(){ cin>>age>>address; }
    void display_1(){
        cout<<"age: "<<age<<endl;
        cout<<"address: "<<address<<endl;
    }

private:
    int age;
    string address;
};
```

## 学生派生类

### 题目概述

根据所给的类Student定义其派生类，并利用构造函数进行数据初始化，使程序能按照"样例"的格式进行输出

### 函数接口定义

```
#include <iostream>
#include<string>
using namespace std;

class Student                             
 {public:                                
   Student(int n,string nam,char s )      
    {num=n;
     name=nam;
     sex=s; }
   ~Student( ) { }
  protected:                               
    int num;
    string name;
    char sex ;                           
};

/* 请在这里添加派生类定义 */
```

### 裁判测试程序样例

```
int main( )
 {Student1 stud1(10010,"Wang-li",'f',19,"115 Beijing Road,Shanghai");
  Student1 stud2(10011,"Zhang-fun",'m',21,"213 Shanghai Road,Beijing");
  stud1.show( );           
  stud2.show( );            
  return 0;
}
```

### 输出样例

```
num: 10010
name: Wang-li
sex: f
age: 19
address: 115 Beijing Road,Shanghai

num: 10011
name: Zhang-fun
sex: m
age: 21
address: 213 Shanghai Road,Beijing
```

### 代码

```
class Student1:public Student{
public:
    Student1(int n,string nam,char s,int a,string add):Student(n,nam,s),age(a),address(add){};
    void show(){
        cout<<"num: "<<num<<endl;
        cout<<"name: "<<name<<endl;
        cout<<"sex: "<<sex<<endl;
        cout<<"age: "<<age<<endl;
        cout<<"address: "<<address<<endl;
        cout<<endl;
    }
protected:
    int age;
    string address;
};
```

## 多重继承派生类构造函数

### 题目概述

根据所给的基类Student和Teacher，定义Graduate类

### 函数接口定义

```
#include <iostream>
#include <string>
using namespace std;
class Teacher                          
 {public:                                 
   Teacher(string nam,int a,string t)      
    {name=nam;
     age=a;
     title=t;}
   void display()                         
     {cout<<"name:"<<name<<endl;
      cout<<"age"<<age<<endl;
      cout<<"title:"<<title<<endl;
     }
  protected:                          
    string name;
    int age;
    string title;                      
};

class Student                         
 {public:
   Student(string nam,char s,float sco)
     {name1=nam;
      sex=s;
      score=sco;}                        
   void display1()                      
    {cout<<"name:"<<name1<<endl;
     cout<<"sex:"<<sex<<endl;
     cout<<"score:"<<score<<endl;
    }
  protected:                             
   string name1;
   char sex;
   float score;                           
 };

 /* 请在这里填写答案 */
```

### 裁判测试程序样例

```
int main( )
 {Graduate grad1("Wang-li",24,'f',"assistant",89.5,1234.5);
  grad1.show( );
  return 0;
}
```

### 输出样例

```
name:Wang-li
age:24
sex:f
score:89.5
title:assistant
wages:1234.5
```

### 代码

```
class Graduate:public Teacher,public Student{
public:
    Graduate(string nam,int a,char s,string t,float sco,float wag):Teacher(nam,a,t),Student(nam,s,sco),wages(wag){};
    void show(){
        cout<<"name:"<<name<<endl;
        cout<<"age:"<<age<<endl;
        cout<<"sex:"<<sex<<endl;
        cout<<"score:"<<score<<endl;
        cout<<"title:"<<title<<endl;
        cout<<"wages:"<<wages<<endl;
    }
protected:
    float wages;
};
```

## 虚基类的应用-人与教师学生

### 题目概述

派生类定义：根据所给的基类，完成多重继承下的派生类定义

### 函数接口定义

```
#include <iostream>
#include <string>
using namespace std;
//定义公共基类Person
class Person                              
{public:
  Person(string nam,char s,int a)              
   {name=nam;sex=s;age=a;}
 protected:                              
   string name;
   char sex;
   int age;
};
//定义类Teacher
class Teacher:virtual public Person              
 {public:                                 
   Teacher(string nam,char s,int a,string t):Person(nam,s,a)       
    {title=t; 
    }
  protected:                                   
    string title;                                
};
//定义类Student
class Student:virtual public Person               
 {public:
   Student(string nam,char s,int a,float sco):   
      Person(nam,s,a),score(sco){}              
  protected:                                     
    float score;                               
 };

 /*这里添加派生类的定义*/
```

### 裁判测试程序样例

```
int main( )
 {Graduate grad1("Wang-li",'f',24,"assistant",89.5,1234.5);
  grad1.show( );
  return 0;
}
```

### 输出样例

```
name:Wang-li
age:24
sex:f
score:89.5
title:assistant
wages:1234.5
```

### 代码

```
class Graduate:public Teacher,public Student{
public:
    Graduate(string nam,char s,int a,string t,float sco,float wag):
            Teacher(nam,s,a,t),Student(nam,s,a,sco),Person(nam,s,a),wages(wag){};
    void show(){
        cout<<"name:"<<name<<endl;
        cout<<"age:"<<age<<endl;
        cout<<"sex:"<<sex<<endl;
        cout<<"score:"<<score<<endl;
        cout<<"title:"<<title<<endl;
        cout<<"wages:"<<wages<<endl;
    }
protected:
    float wages;
};
```

## 汽车类的继承

### 题目概述

根据给定的汽车类vehicle（包含的数据成员有车轮个数wheels和车重weight）声明，完成其中成员函数的定义，之后再定义其派生类并完成测试。

小车类car是它的派生类，其中包含载人数passenger_load。每个类都有相关数据的输出方法。

### 函数接口定义

```
#include<iostream>
using namespace std; 
class Vehicle 
{ 
    protected: 
        int wheels; 
        float weight; 
    public: 
        Vehicle(int wheels,float weight); 
        int get_wheels(); 
        float get_weight(); 
        float wheel_load(); 
        void show(); 
}; 

/* 请在这里填写答案 */
```

### 裁判测试程序样例

```
int main () 
{ 
    Vehicle v(4,1000);
    v.show(); 
    Car car1(4,2000,5);  
    car1.show (); 
    return 0;
}
```

### 输出样例

```
Type:Vehicle
Wheel:4
Weight:1000kg
Type:Car
Type:Vehicle
Wheel:4
Weight:2000kg
Load:5 persons
```

### 代码

```
Vehicle::Vehicle(int wheels, float weight) {
    this->wheels=wheels;
    this->weight=weight;
}
int Vehicle::get_wheels() { return wheels; }
float Vehicle::get_weight() { return weight; }
float Vehicle::wheel_load() { return wheels/weight; }
void Vehicle::show() {
    cout<<"Type:Vehicle"<<endl;
    cout<<"Wheel:"<<get_wheels()<<endl;
    cout<<"Weight:"<<get_weight()<<"kg"<<endl;
}

class Car:public Vehicle{
protected:
    int passenger_load;
public:
    Car(int wheels, float weight, int load):Vehicle(wheels,weight),passenger_load(load){};
    void show(){
        cout<<"Type:Car"<<endl;
        Vehicle::show();
        cout<<"Load:"<<passenger_load<<" persons"<<endl;
    }
};
```

## 2017Final进位与借位

### 题目概述

凤湖小学二年级的陈老师吃惊地发现班上的同学竟然可以分成三类，一类总是可以正确地完成三位整数加减法(GroupA)；一类总是可以正确地完成三位整数的加法，但对于减法运算来说，总是忘记借位的处理(GroupB)；剩下的人总是忘记加法的进位，也总是忘记减法的借位(GroupC)。

现在请给出一次陈老师在课堂提问时，同学们会给出的回答。

实现时请基于下面的基类框架

```
class Group
{
public:
virtual int add(int x, int y)=0;//输出加法的运算结果
virtual int sub(int x, int y)=0;//输出减法的运算结果
}
```

构建出GroupA, GroupB和GroupC三个派出类:

并编写主函数，要求主函数中有一个基类Group指针数组，通过该数组元素统一地进行add和sub运算。

### 函数接口定义

```
输入格式:
首先输入一个整数n，这是班级的人数, 不超过20。

接下来再输入n个数字(取值为1,2,或3)，它是各个学生所属的类别，第一个数字是第一位学生的类别，接下来是第二位学生的类别，..., 最后是第n位学生的类别。类别为1时，表明该学生是第A类；为2时，表明该生是B类，为3时表明是C类。

接下来每一行输入一个数学问题。数学问题由两部分构成，第一部分被提问学生的编号，它是一个不超过n的正整数，1表示第一位学生，2表示第二位学生,n表示第n位学生； 第二部分是具体的数学题，可能是加法，也可能是减法。注意：运算对象和加减号之间没有空格，两个运算对象均是不超过999的非负整数， 减法时，被减数不小于减数。

如果某一行以0开头，则说明提问结束。

输出格式:
输出指定学生对于给定的问题的回答。
```

### 输入样例

```
4
1 2 3 3
1 79+81
2 81-79
4 183+69
0
```

### 输出样例

```
160
12
142
```

### 代码

```
#include <iostream>
#include <cmath>
using namespace std;

class Group{
public:
    virtual int add(int x, int y)=0;//输出加法的运算结果
    virtual int sub(int x, int y)=0;//输出减法的运算结果
};

class GroupA:public Group{
public:
    int add(int x,int y){ cout<<x+y<<endl; };
    int sub(int x,int y){ cout<<x-y<<endl; };
};

class GroupB:public Group{
public:
    int add(int x,int y){ cout<<x+y<<endl; };
    int sub(int x,int y){
        int result;
        int a,b,tx,ty;
        int count=0;
        result=x-y;
        tx=x;
        ty=y;
        while(tx!=0&&ty!=0){
            a=tx%10;
            b=ty%10;
            if(a-b<0) result+=pow(10,count+1);
            tx/=10;
            ty/=10;
            count++;
        }
        cout<<result<<endl;
    };

};

class GroupC:public Group{
public:
    int add(int x,int y){
        int result;
        int a,b,tx,ty;
        int count=0;
        result=x+y;
        tx=x;
        ty=y;
        while(tx!=0&&ty!=0){
            a=tx%10;
            b=ty%10;
            if(a+b>=10) result-=pow(10,count+1);
            tx/=10;
            ty/=10;
            count++;
        }
        cout<<result<<endl;
    };
    int sub(int x,int y){
        int result;
        int a,b,tx,ty;
        int count=0;
        result=x-y;
        tx=x;
        ty=y;
        while(tx!=0&&ty!=0){
            a=tx%10;
            b=ty%10;
            if(a-b<0) result+=pow(10,count+1);
            tx/=10;
            ty/=10;
            count++;
        }
        cout<<result<<endl;
    };
};

int main(){
    Group *st[20];
    int i,n;
    int type;
    int qu;
    int x,y;
    char ch;


    cin>>n;
    for(i=0;i<n;i++){
        cin>>type;
        switch(type){
            case 1:{
                st[i]=new GroupA;
                break;
            }
            case 2:{
                st[i]=new GroupB;
                break;
            }
            case 3:{
                st[i]=new GroupC;
                break;
            }
        }
    }

    cin>>qu;
    while(qu!=0){
        cin>>x>>ch>>y;
        if(ch=='+')
            st[qu-1]->add(x,y);
        else if(ch=='-')
            st[qu-1]->sub(x,y);
        cin>>qu;
    }


};
```

## 大整数求和（运算符重载）

### 题目概述

BigInt类表示不超过100位的无符号大整数。试重载>>，<<和+，以支持无符号大整数的输入、输出与求和（假设结果仍是一个不超过100位的无符号大整数）。

### 函数接口定义

```
重载面向BigInt类对象的运算符：
>>
<<
+
```

### 裁判测试程序样例

```
#include <iostream>
#include <string>
using namespace std;

/* 请在这里填写答案 */

int main(){
    BigInt a, b, c;
    cin>>a>>b;
   c=a+b;
    cout<<a<<"+"<<b<<"="<<c<<endl;
    return 0;
}
```

### 输入样例

```
123456789
987654321
```

### 输出样例

```
123456789+987654321=1111111110
```

### 代码

```
class BigInt{
private:
    string num;
public:
    friend istream& operator>>(istream &input,BigInt &b){
        input>>b.num;
        return input;
    }
    friend ostream& operator<<(ostream &output,BigInt &b){
        output<<b.num;
        return output;
    }
    BigInt operator+(BigInt &b){
        BigInt f;
        int len1,len2,len;
        int i;
        int num,flag;
        string tmp1,tmp2,tmp;

        len1=this->num.length();
        len2=b.num.length();

        if(len1>len2){
            tmp1=this->num;
            tmp2="";
            for(i=0;i<len1-len2;i++) tmp2+="0";
            tmp2+=b.num;
            len=len1;
        }
        else if(len1<len2){
            tmp1=b.num;
            tmp2="";
            for(i=0;i<len2-len1;i++) tmp2+="0";
            tmp2+=this->num;
            len=len2;
        }
        else{
            tmp1=this->num;
            tmp2=b.num;
            len=len1;
        }

        flag=0;
        tmp="";
        for(i=0;i<len;i++){ tmp+="0"; }

        for(i=0;i<len;i++){
            num=tmp1[len-i-1]-'0'+tmp2[len-i-1]-'0'+flag;
            if(num>=10){ flag=1;num-=10; }
            else{ flag=0; }
            tmp[len-i-1]=num+'0';
        }
        if(flag==1){ tmp="1"+tmp; }

        f.num=tmp;
        return f;
    }
};
```

## 学生成绩的输入和输出（运算符重载）

### 题目概述

现在需要输入一组学生的姓名和成绩，然后输出这些学生的姓名和等级。

输入时，首先要输入学生数（正整数）N。接着输入N组学生成绩，每组成绩包括两项：第一项是学生姓名，第二项是学生的成绩（整数）。

输出时，依次输出各个学生的序号（从1开始顺序编号），学生姓名，成绩等级（不小于60为PASS，否则为FAIL）

### 裁判测试程序样例

```
#include <iostream>
#include <string>
using namespace std;

/* 请在这里填写答案 */

int main(){
    int i, repeat;
    Student st;
    cin>>repeat;
    for(i=0;i<repeat;i++){
        cin>>st;
        cout<<st<<endl;
    }
    return 0;
}
```

### 输入样例

```
3
Li 75
Zhang 50
Yang 99
```

### 输出样例

```
1. Li PASS
2. Zhang FAIL
3. Yang PASS
```

### 代码

```
class Student{
private:
    string name;
    int score;
    int no;
public:
    static int NO;
    friend istream& operator>>(istream &input,Student &c){
        input>>c.name>>c.score;
        c.no=++NO;
        if(!input) c=Student();
        return input;
    }
    friend ostream& operator<<(ostream &output,const Student &c){
        string result;
        if(c.score>=60) result="PASS";
        else result="FAIL";
        output<<c.no<<". "<<c.name<<" "<<result;
        return output;
    }
};

int Student::NO=0;
```

## 键值对表的管理（运算符重载）

### 题目概述

键值对(key-value pair)表中存储的是一个个键值对，如table[2016]=19.5表示该表中有一项的key为2016，其value为19.5。我们需要管理的表中， key为整数，而value可为浮点数，整数或字符。所以我们定义一个类模板，得到了三个相应的模板类。

键值对表的容量是有限的，当表中存储的key的数量达到上限时，将不存入新的键值对，但对表中已有的key仍然会继续更新其value。每行输入有三项，第一项是类型（1为浮点数，2为整数，3为字符），第二项是key，第三项是value。在处理完每行输入后，如果输入的类型信息合法，则会输出当前该类型的键值表中最小的value及其key。对于整数和字符表，若干个键值对的value值可能相同，此时输出key最小的那一项。

现在要实现该类模板。

### 函数接口定义

```
template<class T>
class Table
```

### 裁判测试程序样例

```
#include <iostream>
using namespace std;

/* 请在这里填写答案 */

int main() {
    const int SIZE=10;
    int key, ty;
    string temp;
    Table<double> dT(SIZE);
    Table<int> iT(SIZE);
    Table<char> cT(SIZE);
    iT[2016]=19;
    iT[999]=27;
    while(cin>>ty){
        switch(ty) {
            case 1: cin>>key; cin>>dT[key]; dT.min(); break;
            case 2: cin>>key; cin>>iT[key]; iT.min(); break;
            case 3: cin>>key; cin>>cT[key]; cT.min(); break;
            default:getline(cin,temp);
        }
    }
    return 0;
}
```

### 输入样例

```
1 10 25.3
2 12 25
2 2016 25
```

### 输出样例

```
10 25.3
2016 19
12 25
```

输出说明：在输入第一行后，因为浮点数键值表只有一项，故直接输出。输入第二行后，整数表中将增加一项（12,25），此时最小的仍然是(2016,19)，故输出2016 19.在输入第三行后，key为12和2016对应的value均为25,但12的key更小，故输出12 25.

### 代码

```
template<class T>
class Table{
private:
    int *key;
    T *value;
    int size;
    int count;
    T tmp;
public:
    Table(int SIZE):size(SIZE){
        key=new int[SIZE];
        value=new T[SIZE];
        count=0;
    };
    T &operator[](int k){
        int i;
        for(i=0;i<count;i++)
            if(key[i]==k) break;

        if(key[i]==k){
            return value[i];
        }
        else if(i==count&&key[i]!=k){
            if(count>=size){ return tmp; }
            else{
                key[count++]=k;
                return value[count-1];
            }
        }
    };

    void min(){
        int i,index;
        index=0;
        for(i=0;i<count;i++){
            if(value[i]<value[index]) index=i;
            else if(value[i]==value[index]){
                if(key[i]<key[index]) index=i;
            }
        }
        cout<<key[index]<<" "<<value[index]<<endl;
    };
};
```

## 使用成员函数重载复数类的运算符+

### 题目概述

类Complex声明了一个复数类，有两个数据成员realPart（代表复数的实部）和imgPart（代表复数的虚部），并定义了成员函数实现了重载运算符“+”以实现两个复数对象的相加操作。成员函数Show用来输出复数的实部和虚部。请完成对运算符“+”的重载操作。

### 函数接口定义

```
Complex& Complex::operator+(Complex& com);
```

参数com为复数类Complex的对象的引用，函数的返回值为当前对象与com对象相加后的值。

### 裁判测试程序样例

```
#include<iostream>
using namespace std;

class Complex {
public:
    Complex(double realPart = 0, double imgPart = 0) {
        this->realPart = realPart;
        this->imgPart = imgPart;
    }
    Complex& operator+(Complex& com);
    void Show() {
        cout << realPart << " " << imgPart << endl;
    }
private:
    double realPart, imgPart;
};
int main() {
    int r1, i1;         //第1个复数对象的实部和虚部
    int r2, i2;         //第1个复数对象的实部和虚部
    cin >> r1 >> i1;
    cin >> r2 >> i2;
    Complex c1(r1, i1); //构造第1个复数对象c1
    Complex c2(r2, i2); //构造第2个复数对象c2
    c1 = c1 + c2;
    c1.Show();

    return 0;
}
/* 你的代码将被嵌在这里 */
```

### 输入样例

```
3 4
10 20
```

### 输出样例

```
13 24
```

### 代码

```
Complex & Complex::operator+(Complex& com){
    com.realPart+=realPart;
    com.imgPart+=imgPart;
    return com;
}
```

## 复数类重载加法、减法和乘法运算符

### 题目概述

以下定义了一个复数类及其部分实现，现要求将类的构造函数以及运算符+、- 和 * 函数重载补充完整。

### 函数接口定义

```
在这里描述复数类定义。具体如下：
class complex {
    public:
        complex(float r=0,float i=0);                   // 构造函数
        complex operator+(const complex &op2) const;    //重载运算符 +
        complex operator-(const complex &op2) const;    //重载运算符 -
        complex operator*(const complex &op2) const;    //重载运算符 *
        void display() const;                           // 按数学写法输出复数
    private:
        float real;
        float imag;
};
```

### 裁判测试程序样例

```
在这里给出复数类测试的例子。例如：
#include <iostream>
using namespace std;
class complex {
    public:
        complex(float r=0,float i=0);                   // 构造函数
        complex operator+(const complex &op2) const;    //重载运算符 +
        complex operator-(const complex &op2) const;    //重载运算符 -
        complex operator*(const complex &op2) const;    //重载运算符 *
        void display() const;                           // 按数学写法输出复数
    private:
        float real;
        float imag;
};


/* ------------------- 请在这里填写答案--------------------  */


void complex::display() const {
    if(real&&imag)
        if(imag==1||imag==-1)
            cout<<real<<(imag>0?"+":"-")<<"i"<<endl;
        else
            cout<<real<<(imag>0?"+":"")<<imag<<"i"<<endl;
    else if(real)
        cout<<real<<endl;
    else if (imag)
        if(imag==1||imag==-1)
            cout<<(imag>0?"":"-")<<"i"<<endl;
        else
            cout<<imag<<"i"<<endl;
    else
        cout<<0<<endl;
}

int main() {
    double real,imag;
    complex c,d,e;

    cin>>real>>imag;
    complex c1(real,imag);
    cin>>real>>imag;
    complex c2(real,imag);

    c=c1+c2;
    d=c1-c2;
    e=c1*c2;
    c.display() ;
    d.display() ;
    e.display();

    return 0;
}
```

### 输入样例

```
2 3
-4 -5
```

### 输出样例

```
-2-2i
6+8i
7-22i
```

### 代码

```
complex::complex(float r, float i) { real=r;imag=i; }
complex complex::operator+(const complex &op2) const {
    complex t;
    t.real=this->real+op2.real;
    t.imag=this->imag+op2.imag;
    return t;
}
complex complex::operator-(const complex &op2) const {
    complex t;
    t.real=this->real-op2.real;
    t.imag=this->imag-op2.imag;
    return t;
}
complex complex::operator*(const complex &op2) const {
    complex t;
    t.real=this->real*op2.real-this->imag*op2.imag;
    t.imag=this->real*op2.imag+this->imag*op2.real;
    return t;
}
```

## 单目运算符重载（时钟类）

### 题目概述

本题已给出时钟类及其成员函数实现，要求补充完整运算符++重载函数（前置和后置），使之能够实现时钟对象自增1秒。

### 函数接口定义

```
class Clock {
    public:
        Clock(int NewH=0, int NewM=0, int NewS=0);
        void ShowTime();
        friend Clock operator++(Clock& op);         //前置单目运算符重载
        friend Clock operator++(Clock& op,int);     //后置单目运算符重载
    private:
        int Hour, Minute, Second;
};
```

### 裁判测试程序样例

```
#include<iostream>
using namespace std;

class Clock {
    public:
        Clock(int NewH=0, int NewM=0, int NewS=0);
        void ShowTime();
        friend Clock operator++(Clock& op);         //前置单目运算符重载
        friend Clock operator++(Clock& op,int);     //后置单目运算符重载
    private:
        int Hour, Minute, Second;
};

Clock::Clock(int NewH, int NewM, int NewS) {
    Hour=NewH;
    Minute=NewM;
    Second=NewS;
}
void Clock::ShowTime() {
    cout<<Hour<<":"<<Minute<<":"<<Second<<endl;
}

/*---------请在这里填写答案-----------*/

int main() {
    int h, m, s;
    cin>>h>>m>>s;
    Clock a(h,m,s);

    (++a).ShowTime();
    (a++).ShowTime();
    a.ShowTime();

    return 0;
}
```

### 输入样例

```
10 10 10
```

### 输出样例

```
10:10:11
10:10:11
10:10:12
```

### 代码

```
Clock operator++(Clock &op) {
    op.Second++;
    if(op.Second==60){ op.Second=0;op.Minute++; }
    if(op.Minute==60){ op.Minute=0;op.Hour++; }
    if(op.Hour==24){ op.Hour=0; }
    return op;
}

Clock operator++(Clock &op, int) {
    Clock tmp=op;
    op.Second++;
    if(op.Second==60){ op.Second=0;op.Minute++; }
    if(op.Minute==60){ op.Minute=0;op.Hour++; }
    if(op.Hour==24){ op.Hour=0; }
    return tmp;
}
```

## 重载+-运算符

### 题目概述

请根据程序的输出结果，重载类A的+和-运算符。

### 函数接口定义

```
class A {
public:
    A(int x = 0, int y = 0) : x(x), y(y) {}
    void show() const;
    A operator+(A& a);  //重载+运算符
    A operator-(A& a);  //重载-运算符
private:
    int x, y;
};
```

### 裁判测试程序样例

```
#include <iostream>
using namespace std;

class A {
public:
    A(int x = 0, int y = 0) : x(x), y(y) {}
    void show() const;
    A operator+(A& a);
    A operator-(A& a);
private:
    int x, y;
};

void A::show() const {
    cout << "(x,y) = " << "(" << x << "," << y << ")" << endl;
}

/* 请在这里填写答案 */

int main() {
    A a1(1, 2);
    A a2(4, 5);
    A a;
    cout << "a1:";  a1.show();
    cout << "a2:";  a2.show();
    a = a1 + a2;
    cout << "a:";   a.show();
    a = a1 - a2;
    cout << "a:";   a.show();
    return 0;
}
```

### 输出样例

```
a1:(x,y) = (1,2)
a2:(x,y) = (4,5)
a:(x,y) = (5,7)
a:(x,y) = (-3,-3)
```

### 代码

```
A A::operator+(A &a) {
    A t;
    t.x=this->x+a.x;
    t.y=this->y+a.y;
    return t;
}
A A::operator-(A &a) {
    A t;
    t.x=this->x-a.x;
    t.y=this->y-a.y;
    return t;
}
```

## 大整数乘法（运算符重载）

### 题目概述

BigInteger类表示不超过1000位的无符号大整数。试重载>>，<<和*，以支持无符号大整数的输入、输出与乘法。

### 函数接口定义

```
重载面向BigInteger类对象的运算符：
>>
<<
*
```

### 裁判测试程序样例

```
#include <iostream>
#include <string>
using namespace std;

/* 请在这里填写答案 */

int main() 
{
    BigInteger a, b, c;
    cin >> a >> b;
    c = a * b;
    cout << a << "*" << b << "=" << c << endl;
    return 0;
}
```

### 输入样例

```
123456789
987654321
```

### 输出样例

```
123456789*987654321=121932631112635269
```

### 代码

```
class BigInteger{
private:
    string num;
public:
    friend istream& operator>>(istream &input,BigInteger &b){
        input>>b.num;
        return input;
    }
    friend ostream& operator<<(ostream &output,BigInteger &b){
        output<<b.num;
        return output;
    }
    BigInteger operator*(BigInteger &b){
        BigInteger bi;
        if(this->num=="0"||b.num=="0"){
            bi.num="0";
            return bi;
        }
        
        string tmp1,tmp2,tmpi,tmpj,tmp;
        int len1,len2,len;
        int i,j,count;
        int num,flagi,flagj;

        tmp1=this->num;
        tmp2=b.num;

        len1=tmp1.length();
        len2=tmp2.length();

        flagi=0;
        flagj=0;
        count=0;

        for(i=0;i<len1+len2;i++) tmpi+="0";

        for(i=0;i<len1;i++){

            tmpj="";
            for(j=0;j<len2+1;j++) tmpj+="0";

            for(j=0;j<len2;j++){
                num=(tmp1[len1-i-1]-'0')*(tmp2[len2-j-1]-'0')+flagj;
                flagj=num/10;
                num=num%10;
                tmpj[j]=num+'0';
            }
            if(flagj>0) tmpj[len2]=flagj+'0';
            flagj=0;

            for(j=0;j<len2+1;j++){
                num=tmpi[j+count]-'0'+tmpj[j]-'0'+flagi;
                flagi=num/10;
                num=num%10;
                tmpi[j+count]=num+'0';
            }
            if(flagi>0) tmpi[tmpj.length()]=flagi+'0';
            flagi=0;
            count++;
        }

        len=len1+len2;
        if(tmpi[len1+len2-1]=='0') len--;
        for(i=0;i<len;i++) tmp+=tmpi[len-i-1];

        bi.num=tmp;
        return bi;
    }
};
```

## 矩阵运算

### 题目概述

根据main函数中矩阵对象的定义与使用，定义相关的矩阵类Array，并利用运算符重载的方法实现矩阵的加法与输入输出操作。（为简化问题，矩阵中元素为2位以内整数，要求矩阵按照行列的格式输出，每个元素占3位宽度）

### 函数接口定义

```
class Array
/* 请在这里填写答案 */
```

### 裁判测试程序样例

```
int main()
{
    Array arr1,arr2,arr3;
    cin>>arr1;
    cin>>arr2; 
    cout<<arr1<<endl;
    cout<<arr2<<endl;
    arr3=arr1+arr2;
    cout<<arr3;
    return 0;   
}
```

### 输入样例

```
1 2 3 4 5 6
7 8 9 1 11 12
```

### 输出样例

```
  1  2  3
  4  5  6

  7  8  9
  1 11 12

  8 10 12
  5 16 18
```

### 代码

```
#include <iostream>
#include <iomanip>
using namespace std;

class Array{
private:
    int n00,n01,n02;
    int n10,n11,n12;
public:
    Array(){}
    Array operator+(Array &a){
        Array t;
        t.n00=this->n00+a.n00;
        t.n01=this->n01+a.n01;
        t.n02=this->n02+a.n02;
        t.n10=this->n10+a.n10;
        t.n11=this->n11+a.n11;
        t.n12=this->n12+a.n12;
        return t;
    }
    friend istream& operator>>(istream &input,Array &a){
        input>>a.n00>>a.n01>>a.n02>>a.n10>>a.n11>>a.n12;
        return input;
    }
    friend ostream& operator<<(ostream &output,Array &a){
        output<<setw(3)<<setiosflags(ios::right)<<a.n00<<setw(3)<<setiosflags(ios::right)<<a.n01<<setw(3)<<setiosflags(ios::right)<<a.n02<<endl;
        output<<setw(3)<<setiosflags(ios::right)<<a.n10<<setw(3)<<setiosflags(ios::right)<<a.n11<<setw(3)<<setiosflags(ios::right)<<a.n12<<endl;
        return output;
    }
};
```

## 时钟模拟

### 题目概述

一个Time类，数据成员有时、分、秒。要求模拟秒表，每次走一秒，满60秒进位，秒又从零开始计数。满60分进位，分又从零开始计数。输出时、分和秒的值。（使用重载++运算符实现）

### 函数接口定义

```
class MyTime
```

### 裁判测试程序样例

```
/* 请在这里填写答案 */

int main()
{
    MyTime t1,t2(23,59,59),t3;
    cin>>t3;
    ++t1;
    cout<<t1<<endl;
    ++t2;
    cout<<t2<<endl;
    ++t3;
    cout<<t3<<endl;
    return 0;
}
```

### 输入样例

```
12 35 59
```

### 输出样例

```
0:0:1
0:0:0
12:36:0
```

### 代码

```
#include <iostream>
using namespace std;

class MyTime{
private:
    int hour;
    int minute;
    int second;
public:
    MyTime(int h=0,int m=0,int s=0):hour(h),minute(m),second(s){}
    friend istream& operator>>(istream &input,MyTime &t){
        input>>t.hour>>t.minute>>t.second;
        return input;
    }
    friend ostream& operator<<(ostream &output,MyTime &t){
        output<<t.hour<<":"<<t.minute<<":"<<t.second;
        return output;
    }
    friend MyTime operator++(MyTime& t){
        t.second++;
        if(t.second==60){ t.second=0;t.minute++; }
        if(t.minute==60){ t.minute=0;t.hour++; }
        if(t.hour==24){ t.hour=0; }
        return t;
    }
    friend MyTime operator++(MyTime& t,int){
        MyTime tmp=t;
        t.second++;
        if(t.second==60){ t.second=0;t.minute++; }
        if(t.minute==60){ t.minute=0;t.hour++; }
        if(t.hour==24){ t.hour=0; }
        return tmp;
    }
};
```

## 日期类的设计与实现

### 题目概述

使用重载运算符（++，+=，<<等）实现日期类的操作。功能包括： 1）设置日期，如果日期设置不符合实际，则设置为默认日期（1900年1月1日） 2）在日期对象中向日期添加1或者加若干天（加入日期值后根据实际的日期进行月份、年份的变化） 3）重载流插入运算符进行日期的输出，其中月份要用名称表示

### 函数接口定义

```
class MyDate
```

### 裁判测试程序样例

```
#include <string>
#include <iostream>
using namespace std;

/* 请在这里填写答案 */

int main()
{
    int m,d,y;
    MyDate d1,d2,d3;
    cin>>m>>d>>y;
    d1.setDate(m,d,y);
    
    cin>>m>>d>>y;
    d2.setDate(m,d,y);
    
    cin>>m>>d>>y;
    d3.setDate(m,d,y);
    
    cout << "d1 is " << d1 << "\nd2 is " << d2;
    cout << "\n\nd1 += 7 is " << ( d1 += 7 );
    cout << "\n\n d2 is " << d2;
    cout << "\n++d2 is " << ++d2;
    cout << "\n\nTesting the prefix increment operator:\n"<< " d3 is " << d3 << endl;
    cout << "++d3 is " << ++d3 << endl;
    cout << " d3 is " << d3;
    cout << "\n\nTesting the postfix increment operator:\n"<< " d3 is " << d3 << endl;
    cout << "d3++ is " << d3++ << endl;
    cout << " d3 is " << d3 <<endl;
} 
```

### 输入样例

```
13 38 100
12 31 2009
2 28 2000
```

### 输出样例

```
d1 is January 1, 1900
d2 is December 31, 2009

d1 += 7 is January 8, 1900

d2 is December 31, 2009
++d2 is January 1, 2010

Testing the prefix increment operator:
d3 is February 28, 2000
++d3 is February 29, 2000
d3 is February 29, 2000

Testing the postfix increment operator:
d3 is February 29, 2000
d3++ is February 29, 2000
d3 is March 1, 2000
```

### 代码

```
class MyDate{
private:
    int year;
    int month;
    int day;
    string moname[12]={"January","February","March","April","May","June","July","August","September","October","November","December"};
    int daym[12]={31,29,31,30,31,30,31,31,30,31,30,31};
    int daynm[12]={31,28,31,30,31,30,31,31,30,31,30,31};
    void adjust(){
        int flag=1;

        while(flag==1){
            if(ismore(year)){
                while(day>daym[month-1]&&month<=12){
                    day-=daym[month-1];
                    month++;
                }
                if(month>12){
                    month-=12;
                    year++;
                    flag=1;
                }
                else flag=0;
            }
            else{
                while(day>daynm[month-1]&&month<=12){
                    day-=daynm[month-1];
                    month++;
                }
                if(month>12){
                    month-=12;
                    year++;
                    flag=1;
                }
                else flag=0;
            }
        }
    }

    bool ismore(int y){
        if((y%400!=0&&y%4==0)||(y%400==0)) return true;
        else return false;
    }
public:
    void setDate(int m,int d,int y){
        if(y<1000||m<0||m>12||d<0||d>31){
            year=1900;
            month=1;
            day=1;
        }
        else{
            year=y;
            month=m;
            day=d;
        }

    }

    MyDate operator+=(int d){
        this->day+=d;
        adjust();
        return *this;
    }

    MyDate operator++(){
        this->day++;
        adjust();
        return *this;
    }

    MyDate operator++(int){
        MyDate tmp=*this;
        this->day++;
        adjust();
        return tmp;
    }

    friend ostream& operator<<(ostream &output,const MyDate &m){
        output<<m.moname[m.month-1]<<" "<<m.day<<", "<<m.year;
        return output;
    }
};
```

## 时间相加

### 题目概述

设计一个时间类，用来保存时、分、秒等私有数据成员，通过重载操作符“+”实现2个时间的相加。要求： （1）小时的时间范围限制在大于等于0；（2）分的时间范围为0~59分；（3）秒的时间范围为0~59秒。

### 裁判测试程序样例

```
#include <iostream>
using namespace std;
class Time {
private:
 int hours,minutes, seconds;
public:
 Time(int h=0, int m=0, int s=0);
 Time operator + (Time &);
 void DispTime();
};

/* 请在这里填写答案 */

int main() {
 Time tm1(8,75,50),tm2(0,6,16), tm3;
 tm3=tm1+tm2;
 tm3.DispTime();
 return 0;
}
```

### 输出样例

```
9h:22m:6s
```

### 代码

```
Time::Time(int h, int m, int s) {
    hours=h;
    minutes=m;
    seconds=s;
}

Time Time::operator+(Time &t) {
    Time p;
    p.seconds=this->seconds+t.seconds;
    p.minutes=this->minutes+t.minutes;
    p.hours=this->hours+t.hours;
    if(p.seconds>=60){ p.seconds-=60;p.minutes++; }
    if(p.minutes>=60){ p.minutes-=60;p.hours++; }
    if(p.hours>=24){ p.hours-=24; }
    return p;
}

void Time::DispTime() {
    cout<<hours<<"h:"<<minutes<<"m:"<<seconds<<"s"<<endl;
}
```

## 运算符重载

### 题目概述

请定义一个分数类，拥有两个整数的私有数据成员，分别表示分子和分母（分母永远为正数，符号通过分子表示）。 重载运算符加号"+"，实现两个分数的相加，所得结果必须是最简分数。

### 函数接口定义

```
输入:
第一行的两个数 分别表示 第一个分数的分子和分母（分母不为0）。 第二行的两个数 分别表示 第二个分数的分子和分母。

输出:
第一个数表示分子，第二个数表示分母（若分数代表的是整数，则不输出分母）。
```

### 输入样例

```
1  5
2  5
```

### 输出样例

```
3 5
```

### 代码

```
#include <iostream>
using namespace std;

class Fraction{
private:
    int num;
    int deno;
    int gcd(int a, int b){
        int temp = a;
        while(a%b != 0){
            a = b;
            b = temp%b;
            temp = a;
        }
        return b;
    }
public:
    Fraction(int a=1,int b=1):num(a),deno(b){};
    Fraction operator+(Fraction &p){
        Fraction t;
        int g;

        t.deno=this->deno*p.deno;
        t.num=this->num*p.deno+p.num*this->deno;

        if(t.num==0){
            t.deno=1;
            return t;
        }

        if(t.deno>t.num)
            g=gcd(t.deno,t.num);
        else
            g=gcd(t.num,t.deno);

        t.deno/=g;
        t.num/=g;

        if(t.deno<0){
            t.deno=-t.deno;
            t.num=-t.num;
        }
        return t;

    }

    void show(){
        cout<<num;
        if(deno!=1)
            cout<<" "<<deno;
        cout<<endl;
    }
};

int main(){
    int n1,d1,n2,d2;
    cin>>n1>>d1>>n2>>d2;
    Fraction f1(n1,d1),f2(n2,d2),f3;
    f3=f1+f2;
    f3.show();
}
```

## 分数加法运算重载

### 题目概述

相信同学们对复数运算符重载已经相当熟悉啦，那今天那我们来看看分数又该如何处理呢?定义一个分数类FS，有私有成员分子fz，分母fm。另有公有成员函数FS operator + (const FS &f)对运算符“+”进行重载，实现两个分数相加。题目首先给出一个整型数n，紧跟着2n行输入，输入形如3z4m，代表分子为3，分母为4。其中分母不为0，输入时分母可以为负数，但输出时分母必须为正数。 要求对分数进行两两求和，并化简。(备注说明：分数为0时，表示成0z1m，如果结果为负数，那么分子取负数，分母为正数)

### 输入样例

```
3
4z9m
2z9m
4z5m
5z4m
2z-5m
1z-5m
```

### 输出样例

```
2z3m
41z20m
-3z5m
```

### 代码

```
#include <iostream>
using namespace std;

class FS{
private:
    double fz;
    double fm;
    int gcd(int a, int b){
        int temp = a;
        while(a%b != 0){
            a = b;
            b = temp%b;
            temp = a;
        }
        return b;
    }
public:
    FS(double z=1,double m=1):fz(z),fm(m){}
    FS operator+(const FS &f){
        FS t;
        int g;

        t.fz=this->fz*f.fm+f.fz*this->fm;
        t.fm=this->fm*f.fm;

        if(t.fz==0){
            t.fm=1;
            return t;
        }

        if(t.fz>t.fm) g=gcd(t.fz,t.fm);
        else g=gcd(t.fm,t.fz);

        t.fz/=g;
        t.fm/=g;

        if(t.fm<0){ t.fz=-t.fz;t.fm=-t.fm; }

        return t;
    }
    void show(){ cout<<fz<<"z"<<fm<<"m"<<endl; }

};

int main(){
    int i,n;
    double a,b;
    double c,d;
    char c1,c2;

    cin>>n;
    for(i=0;i<n;i++){
        cin>>a>>c1>>b>>c2;
//        if(b<0){ a=-a;b=-b; }
        FS f1(a,b);
        cin>>c>>c1>>d>>c2;
//        if(d<0){ c=-c;d=-d; }
        FS f2(c,d);
        FS f3;
        f3=f1+f2;
        f3.show();
    }
}
```

## 复数相加

### 题目概述

一个复数类，运算符重载 + ，实现复数和复数的相加。输入一组复数，每行一个复数，直到输入0结束。 输出这组复数的结果。

提示: 复数的输入和输出符合数学书写规范

### 输入样例

```
3+2i
2+3i
0
```

### 输出样例

```
5+5i
```

### 代码

```
#include <iostream>
#include <sstream>
using namespace std;

class Complex{
private:
    double real;
    double imag;
public:
    Complex(double r=0,double i=0):real(r),imag(i){}
    Complex operator+(Complex &c){
        Complex t;
        t.real=this->real+c.real;
        t.imag=this->imag+c.imag;
        return t;
    }

    void show(){
        if(imag==0){
            cout<<real<<endl;
            return;
        }

        else{
            if(real!=0) cout<<real;

            if(imag>0){
                cout<<"+";
                if(imag!=1) cout<<imag;
            }
            else{
                if(imag!=-1) cout<<imag;
                else cout<<"-";
            }

            cout<<"i"<<endl;
        }

    }
};

int main(){
    double real,imag;
    string comp;
    char ch;
    int i;
    Complex result;

    cin>>comp;
    while(comp!="0"){
        i=1;
        if(comp[comp.length()-1]!='i'){
            stringstream ss;
            ss<<comp;
            ss>>real;
            ss.clear();
            ss.str("");
            imag=0;
        }
        else{
            while(comp[i]!='\0'){
                if(comp[i]=='+'||comp[i]=='-') break;
                i++;
            }

            if(comp[i]!='+'&&comp[i]!='-'){
                real=0;
                if(comp[0]=='i') imag=1;
                else if(comp[0]=='-'&&comp[1]=='i') imag=-1;
                else{
                    stringstream ss;
                    ss<<comp.substr(0,comp.length()-1);
                    ss>>imag;
                    ss.clear();
                    ss.str("");
                }
            }
            else{
                stringstream ss;
                ss<<comp.substr(0,i);
                ss>>real;
                ss.clear();
                ss.str("");

                if(comp[i]=='+'&&comp[i+1]=='i') imag=1;
                else if(comp[i]=='-'&&comp[i+1]=='i') imag=-1;
                else{
                    ss<<comp.substr(i+1,comp.length()-i-2);
                    ss>>imag;
                    ss.clear();
                    ss.str("");
                    if(comp[i]=='-') imag=-imag;
                }
            }
        }

        Complex c(real,imag);
        result=result+c;

        cin>>comp;
    }

    result.show();
}
```

## 复数的比较

### 题目概述

建立一个复数类，实数和虚数是其私有数据成员。建立一个>（大于号）的运算符重载，比较两个复数间模的大小。

### 函数接口定义

```
输入格式：测试输入包含若干测试用例，每个测试用例占一行。每个测试用例包括四个数字，前两个数字分别表示第一个复数的实部和虚部，第三个和第四个数字分别表示第二个复数的实部和虚部。每个数字之间用空格间隔。当读入一个测试用例是0 0 0 0时输入结束，相应的结果不要输出。

输出格式：对每个测试用例输出一行。当第一个复数的模大于第二个复数的模时，输出 true ，当第一个复数的模小于或等于第二个复数的模时，输出false
```

### 输入样例

```
3 5 4 0
0 3 4 1
0 0 0 0
```

### 输出样例

```
true
false
```

### 代码

```
#include <iostream>
#include <cmath>
using namespace std;

class Complex{
private:
    double real;
    double imag;
    double dist;
public:
    Complex(double r=0,double i=0):real(r),imag(i){
        dist=pow(pow(r,2)+pow(i,2),0.5);
    }
    bool operator>(Complex &c){
        if(this->dist>c.dist) return true;
        else return false;
    }
};

int main(){
    double r1,i1,r2,i2;

    cin>>r1>>i1>>r2>>i2;
    while(r1!=0||i1!=0||r2!=0||i2!=0){
        Complex c1(r1,i1),c2(r2,i2);
        if(c1>c2) cout<<"true"<<endl;
        else cout<<"false"<<endl;

        cin>>r1>>i1>>r2>>i2;
    }
}
```

## 分钟秒钟的时间相减

### 题目概述

定义一个时间类，分钟和秒钟是其两个私有成员数据。输入一个起始时间和一个结束时间(起始时间早于结束时间)，通过运算符重载-（减号），计算这两个时间相隔多少秒钟。说明：这两个时间在同一小时之内，且采用60分钟60秒钟的计时分式，即从00:00-59:59。

### 函数接口定义

```
输入格式： 测试输入包含若干测试用例，每个测试用例占一行。每个测试用例包括四个数，每个数之间用空格间隔，每个数都由两位数字组成，第一个数和第二个数分别表示起始时间的分钟和秒钟，第三个数和第四个数分别表示结束时间的分钟和秒钟。当读入一个测试用例是00 00 00 00时输入结束，相应的结果不要输出。

输出格式：对每个测试用例输出一行。输出一个数即可，表示两者之间间隔的秒钟数。
```

### 输入样例

```
12 11 12 58
00 13 16 00
09 07 23 59
00 00 00 00
```

### 输出样例

```
47
947
892
```

### 代码

```
#include <iostream>
using namespace std;

class Time{
private:
    int minute;
    int second;
public:
    Time(int m,int s):minute(m),second(s){}
    void operator-(Time &t){
        cout<<(this->minute-t.minute)*60+(this->second-t.second)<<endl;
    }
};

int main(){
    int m1,s1,m2,s2;
    cin>>m1>>s1>>m2>>s2;
    while(m1!=0||s1!=0||m2!=0||s2!=0){
        Time t1(m1,s1),t2(m2,s2);
        t2-t1;
        cin>>m1>>s1>>m2>>s2;
    }
}
```

## 时间换算

### 题目概述

定义一个时间类time,内有数据成员hour,minute,second，另有成员函数：构造函数用于初始化数据成员，输出函数，运算符重载+（加号），。编写主函数：创建时间对象，再输入秒数 n，通过运算符重载+（减号），计算该时间再过 n 秒后的时间值，时间的表示形式为时:分:秒，超过 24 时从 0 时重新开始计时。

### 函数接口定义

```
测试输入包含若干测试用例，每个测试用例占一行。当读入0 0 0 0时输入结束，相应的结果不要输出。
```

### 输入样例

```
0 0 1 59 (时间为0:0:1,秒数n=59)
11 59 40 30 (时间为11:59:40,秒数n=30)
23 59 40 3011 (时间为23:59:40,秒数n=3011)
0 0 0 0
```

### 输出样例

```
time:0:1:0↙(0:0:01加上59秒的新时间)   
time:12:0:10↙(11:59:40加上30秒的新时间)
time:0:49:51↙(23:59:40加上3011秒的新时间)
```

### 代码

```
#include <iostream>
using namespace std;

class Time{
private:
    int hour;
    int minute;
    int second;
public:
    Time(int h=0,int m=0,int s=0):hour(h),minute(m),second(s){}
    void show(){ cout<<"time:"<<hour<<":"<<minute<<":"<<second<<endl; }
    Time operator+(int t){
        Time q;
        q.second=this->second+t;
        q.minute=this->minute;
        q.hour=this->hour;
        while(q.second>=60){ q.second-=60;q.minute++; }
        while(q.minute>=60){ q.minute-=60;q.hour++; }
        while(q.hour>=24){ q.hour-=24; }
        return q;
    }
    Time operator-(int t){
        Time q;
        q.second=this->second-t;
        q.minute=this->minute;
        q.hour=this->hour;
        while(q.second<0){ q.second+=60;q.minute--; }
        while(q.minute<0){ q.minute+=60;q.hour--; }
        while(q.hour<0){ q.hour+=24; }
        return q;
    }
};

int main(){
    int hour,minute,second,n;

    cin>>hour>>minute>>second>>n;
    while(hour!=0||minute!=0||second!=0||n!=0){
        Time t1(hour,minute,second),t2;
        if(n>=0) t2=t1+n;
        else{ n=-n; t2=t1-n; }
        t2.show();
        cin>>hour>>minute>>second>>n;
    }
}
```

## 数据的最大值问题（重载+函数模板）

### 题目概述

两个类如下设计：类time有三个数据成员，hh，mm，ss，分别代表时，分和秒，并有若干构造函数和一个重载-（减号）的成员函数。类date有三个数据成员，year，month，day分别代表年月日，并有若干构造函数和一个重载>（<）（大于号或者小于号）的成员函数。

要求设计一个函数模板template <\class T>\ double maxn(T x[], int len) 对int，float，time和date或者其他类型的数据，返回最大值。

主函数有如下数据成员：

```
int intArray[100];
double douArray[100];time timeArray[100];
date dateArray[100];
```

其中，hh = 3600 ss， mm = 60 ss， year = 365 day， month = 30 day，对于time和date类型，数据在转换成ss或者day后进行运算。

### 函数接口定义

```
输入格式：

每行为一个操作，每行的第一个数字为元素类型，1为整型元素，2为浮点型元素，3为time类型，4为date类型，若为整型元素，接着输入整型数据，以0结束。若为浮点型元素，接着输入浮点型数据，以0结束。若为time型元素， 输入time型数据（hh1 mm1 ss1 hh2 mm2 ss2），以0结束。若为date型数据，输入date型数据（year1 month1 day1 year2 month2 day2），以0结束。输入-1时表示全体输入结束。

输出格式：

对每次输入，输出一个最大值。
```

### 输入样例

```
1 4 5 9 3 7 0
2 4.4 5.5 6.9 3.2 2.7 0
3 18 21 22 18 20 31 18 21 49 0
4 2013 5 14 2013 5 15 2013 4 1 0
-1
```

### 输出样例

```
9
6.9
18 21 49
2013 5 15
```

### 代码

```
#include <iostream>
using namespace std;

template <class T>
double maxn(T x[], int len){
    int i,index;
    index=0;
    for(i=1;i<len;i++){
        if(x[i]>x[index]) index=i;
    }
    cout<<x[index]<<endl;
    return 0;
}

class Time{
private:
    int hh;
    int mm;
    int ss;
public:
    Time(){}
    Time(int h,int m,int s):hh(h),mm(m),ss(s){}

    bool operator>(Time &t){
        if((this->hh*3600+this->mm*60+this->ss)-(t.hh*3600+t.mm*60+t.ss)>0) return true;
        else return false;
    }

    friend ostream& operator<<(ostream &output,Time &t){
        output<<t.hh<<" "<<t.mm<<" "<<t.ss;
        return output;
    }

};

class Date{
private:
    int year;
    int month;
    int day;
public:
    Date(){}
    Date(int y,int m,int d):year(y),month(m),day(d){}
    bool operator>(Date &d){
        if((this->year*365+this->month*30+this->day)-(d.year*365+d.month*30+d.day)>0) return true;
        else return false;
    }

    friend ostream& operator<<(ostream &output,Date &d){
        output<<d.year<<" "<<d.month<<" "<<d.day;
        return output;
    }
};

int main(){
    int intArray[100];
    double douArray[100];
    Time timeArray[100];
    Date dateArray[100];
    int type;
    int cint,cdou,ctime,cdate;
    int di;
    double dd;
    int di1,di2,di3;

    cint=0;
    cdou=0;
    ctime=0;
    cdate=0;

    cin>>type;
    while(type!=-1){
        switch(type){
            case 1:{
                cin>>di;
                while(di!=0){
                    intArray[cint++]=di;
                    cin>>di;
                }
                maxn(intArray,cint);
                break;
            }
            case 2:{
                cin>>dd;
                while(dd!=0){
                    douArray[cdou++]=dd;
                    cin>>dd;
                }
                maxn(douArray,cdou);
                break;
            }
            case 3:{
                cin>>di1;
                while(di1!=0){
                    cin>>di2>>di3;
                    timeArray[ctime++]=*new Time(di1,di2,di3);
                    cin>>di1;
                }
                maxn(timeArray,ctime);
                break;
            }
            case 4:{
                cin>>di1;
                while(di1!=0){
                    cin>>di2>>di3;
                    dateArray[cdate++]=*new Date(di1,di2,di3);
                    cin>>di1;
                }
                maxn(dateArray,cdate);
                break;
            }
        }
        cin>>type;
    }
}
```

## 数据的间距问题（重载+函数模板）

### 题目概述

三个类如下设计：类cTime有三个数据成员，hh，mm，ss，分别代表时，分和秒，并有若干构造函数和一个重载-（减号）的成员函数。类point有两个数据成员，x，y分别坐标，并有若干构造函数和一个重载-（减号）的成员函数。类date有三个数据成员，year，month，day分别代表年月日，并有若干构造函数和一个重载-（减号）的成员函数。 要求设计一个函数模板template <\class T>\ double dist(T a, T b) 对int，float，cTime，point和date或者其他类型的数据，返回间距。

其中，hh = 3600 ss， mm = 60 ss， year = 365 day， month = 30 day，对于cTime和date类型，数据在转换成ss或者day后进行运算。

### 函数接口定义

```
输入格式：

每一行为一个操作，每行的第一个数字为元素类型，1为整型元素，2为浮点型元素，3为point类型，4,为time类型，5为date类型，若为整型元素，接着输入两个整型数据，

若为浮点型元素，接着输入两个浮点型数据，若为point型元素，输入两个point型数据（x1 y1 x2 y2），若为time型元素， 输入两个cTime型数据（hh1 mm1 ss1 hh2 mm2 ss2），若为date型数据，输入两个date型数据（year1 month1 day1 year2 month2 day2）。输入0时标志输入结束。

输出格式：

对每个输入，每行输出一个间距值。
```

### 输入样例

```
1 2 5
4 18 21 22 18 20 31
3 2 4 5 9
5 2013 5 14 2013 5 15
2 2.2 9.9
0
```

### 输出样例

```
3
51
5.83095
1
7.7
```

### 代码

```
#include <iostream>
#include <cmath>
using namespace std;

template <class T>
double dist(T a,T b){
    return abs(a-b);
}

class cTime{
private:
    int hh;
    int mm;
    int ss;
public:
    cTime(int h,int m,int s):hh(h),mm(m),ss(s){}
    double operator-(cTime &c){
        return abs((this->hh*3600+this->mm*60+this->ss)-(c.hh*3600+c.mm*60+c.ss));
    }
};

class point{
private:
    double x;
    double y;
public:
    point(double x1,double y1):x(x1),y(y1){}
    double operator-(point &c){
        return pow(pow(this->x-c.x,2)+pow(this->y-c.y,2),0.5);
    }
};

class date{
private:
    int year;
    int month;
    int day;
public:
    date(int y,int m,int d):year(y),month(m),day(d){}
    double operator-(date &c){
        return abs((this->year*365+this->month*30+this->day)-(c.year*365+c.month*30+c.day));
    }
};

int main(){
    int type;
    int di1,di2,di3,di4,di5,di6;
    double dd1,dd2,dd3,dd4;

    cin>>type;
    while(type!=0){
        switch(type){
            case 1:{
                cin>>di1>>di2;
                cout<<dist(di1,di2)<<endl;
                break;
            }
            case 2:{
                cin>>dd1>>dd2;
                cout<<dist(dd1,dd2)<<endl;
                break;
            }
            case 3:{
                cin>>dd1>>dd2>>dd3>>dd4;
                point p1(dd1,dd2),p2(dd3,dd4);
                cout<<dist(p1,p2)<<endl;
                break;
            }
            case 4:{
                cin>>di1>>di2>>di3>>di4>>di5>>di6;
                cTime c1(di1,di2,di3),c2(di4,di5,di6);
                cout<<dist(c1,c2)<<endl;
                break;
            }
            case 5:{
                cin>>di1>>di2>>di3>>di4>>di5>>di6;
                date d1(di1,di2,di3),d2(di4,di5,di6);
                cout<<dist(d1,d2)<<endl;
                break;
            }
        }
        cin>>type;
    }
}
```

## 集合的模拟实现（函数模板）

### 题目概述

我们可以用一个数组来模拟集合，add运算用以实现集合元素的增加，delete运算用于实现集合元素的删除，find运算用以实现集合元素的查找，但是目前集合元素类型未知，可以是int、char、double等基本数据类型，也可以是String、Time、Student等对象类型，要求采用模板函数实现集合元素的增加、删除和查找功能。

三个模板函数如下：

```
int addSet(T * myset, T elem，int len)
int deleSet(T * myset, T elem, int len)
int findElem(T * myset, T elem, int len)
```

其中，addSet向集合中添加一个元素，deleSet从集合中删除一个元素，findElem判断elem是否是集合成员,三个函数分别返回元素插入位置，删除位置和存在位置。

主函数有如下数据成员：

```
int intSet[100]
double douSet[100]
String StrSet[100] 分别是int类型、double类型、String的数组集合。
int intLen, douLen, strLen分别是int类型、double类型、String的数组集合的长度
```

完成上述函数模板和主函数，主函数根据输入的信息，建立初始的空集合，调用三个模板函数分别对intSet、douSet和StrSet执行相应的操作，并输出对应的集合信息。

### 函数接口定义

```
输入格式：
每一行为一个集合操作，每行的第一个数字为集合元素类型，1为整型元素，2为浮点型元素，3为String类型，第二个数字为集合操作类型，1为插入，2为删除，3为查找，第三个为集合元素，集合元素类型视第一个数字给定的集合元素类型而定。输入0时标志输入结束。

输出格式：
输出当前操作的执行位置（插入位置、删除位置和存在位置）
删除操作时，如果元素X不存在，输出“X is not exist!”。
插入操作时，如果集合已满，输出“Full Set.”若元素已存在，输出“X is already exist!”
查找操作时，如果找不到元素，输出“X is not exist!”。
```

### 输入样例

```
1 1 1
1 1 2
1 3 1
1 2 1
1 2 3
1 3 1
2 1 1.1
2 1 2.2
2 1 3.3
2 3 1.1
2 2 2.2
2 2 2.2
3 1 abc
3 1 bcd
3 3 abc
3 2 abc
3 3 abc
0
```

### 输出样例

```
0
1
0
0
3 is not exist!
1 is not exist!
0
1
2
0
1
2.2 is not exist!
0
1
0
0
abc is not exist!
```

### 代码

```
#include <iostream>
using namespace std;

template<class T>
int addSet(T *myset,T elem,int len){
    int i;
    if(len>=100){
        cout<<"Full Set."<<endl;
        return -1;
    }

    for(i=0;i<len;i++){
        if(myset[i]==elem) break;
    }
    if(myset[i]==elem){
        cout<<elem<<" is already exist!"<<endl;
        return -1;
    }
    else if(i==len&&myset[i]!=elem){
        myset[len++]=elem;
        cout<<i<<endl;
        return i;
    }
}

template<class T>
int deleSet(T *myset,T elem,int len){
    int i;
    for(i=0;i<len;i++){
        if(myset[i]==elem) break;
    }
    if(myset[i]==elem){
        cout<<i<<endl;
        for(;i<len-1;i++){
            myset[i]=myset[i+1];
        }
        len--;
        return i;
    }
    else if(i==len&&myset[i]!=elem){
        cout<<elem<<" is not exist!"<<endl;
        return -1;
    }
}

template<class T>
int findElem(T *myset,T elem,int len){
    int i;
    for(i=0;i<len;i++){
        if(myset[i]==elem) break;
    }
    if(myset[i]==elem){
        cout<<i<<endl;
        return i;
    }
    else if(i==len&&myset[i]!=elem){
        cout<<elem<<" is not exist!"<<endl;
        return -1;
    }
}

int main(){
    int intSet[100];
    double douSet[100];
    string strSet[100];
    int intLen, douLen, strLen;
    intLen=0;
    douLen=0;
    strLen=0;

    int type,move;
    int d1;
    int ren;
    double d2;
    string d3;

    cin>>type;
    while(type!=0){
        cin>>move;
        if(type==1) cin>>d1;
        else if(type==2) cin>>d2;
        else if(type==3) cin>>d3;

        if(type==1){
            if(move==1){
                ren=addSet(intSet,d1,intLen);
                if(ren!=-1) intLen++;
            }
            else if(move==2){
                ren=deleSet(intSet,d1,intLen);
                if(ren!=-1) intLen--;
            }
            else if(move==3) findElem(intSet,d1,intLen);
        }
        else if(type==2){
            if(move==1){
                ren=addSet(douSet,d2,douLen);
                if(ren!=-1) douLen++;
            }
            else if(move==2){
                ren=deleSet(douSet,d2,douLen);
                if(ren!=-1) douLen--;
            }
            else if(move==3) findElem(douSet,d2,douLen);
        }
        else if(type==3){
            if(move==1){
                ren=addSet(strSet,d3,strLen);
                if(ren!=-1) strLen++;
            }
            else if(move==2){
                ren=deleSet(strSet,d3,strLen);
                if(ren!=-1) strLen--;
            }
            else if(move==3) findElem(strSet,d3,strLen);
        }

        cin>>type;
    }
}
```

## 集合的模拟实现（类模板）

### 题目概述

我们可以用一个类来模拟集合及集合运算，add运算用以实现集合元素的增加，delete运算用于实现集合元素的删除，find运算用以实现集合元素的查找，但是目前集合元素类型未知，可以是int、char、double等基本数据类型，也可以是String、Time、Student等对象类型，要求采用类模板实现集合及集合运算，包括集合元素的增加、删除和查找的等基本功能。

集合模板类MySet包括数据如下：

```
T data[100];//用数组来存放所有的集合元素，最多不超过100个元素

int count;//表示目前集合中有多少个元素
```

包括成员函数如下：

构造函数若干个，集合运算函数如下：

```
int addSet( T elem)

int deleSet(T elem)

int findElem(T elem)
```

其中，addSet向集合中添加一个元素，deleSet从集合中删除一个元素，findElem判断elem是否是集合成员,三个函数分别返回元素插入位置，删除位置和存在位置。

主函数有如下数据成员 ：

```
MySet<\int>\ intSet;（反斜杠是转义字符，使用时去掉）

MySet<\double>\ douSet;

MySet<\string>\ strSet;
```

分别是int类型、double类型、String的集合。

完成上述类模板和主函数，主函数根据输入的信息，建立初始的三种不同类型的空集合对象，调用成员函数分别对intSet、douSet和StrSet执行相应的操作，并输出对应的集合信息。

### 函数接口定义

```
输入格式：

每一行为一个集合操作，每行的第一个数字为集合元素类型，1为整型元素，2为浮点型元素，3为String类型，第二个数字为集合操作类型，1为插入，2为删除，3为查找，第三个为集合元素，集合元素类型视第一个数字给定的集合元素类型而定。输入0时标志输入结束。

输出格式：

输出当前操作的执行位置（插入位置、删除位置和存在位置）

删除操作时，如果元素X不存在，输出“X is not exist!”。

插入操作时，如果集合已满，输出“Full Set.”若元素已存在，输出“X is already exist!”

查找操作时，如果找不到元素，输出“X is not exist!”。
```

### 输入样例

```
1 1 1
1 1 2
1 3 1
1 2 1
1 2 3
1 3 1
2 1 1.1
2 1 2.2
2 1 3.3
2 3 1.1
2 2 2.2
2 2 2.2
3 1 abc
3 1 bcd
3 3 abc
3 2 abc
3 3 abc
0
```

### 输出样例

```
0
1
0
0
3 is not exist!
1 is not exist!
0
1
2
0
1
2.2 is not exist!
0
1
0
0
abc is not exist!
```

### 代码

```
#include <iostream>
using namespace std;

template<class T>
class MySet{
private:
    T data[100];
    int count;
public:
    MySet(){ count=0; }
    int addSet(T elem){
        int i;
        if(count>=100){
            cout<<"Full Set."<<endl;
            return -1;
        }

        for(i=0;i<count;i++){
            if(data[i]==elem) break;
        }
        if(data[i]==elem){
            cout<<elem<<" is already exist!"<<endl;
            return -1;
        }
        else if(i==count&&data[i]!=elem){
            data[count++]=elem;
            cout<<i<<endl;
            return i;
        }
    }

    int deleSet(T elem){
        int i;
        for(i=0;i<count;i++){
            if(data[i]==elem) break;
        }
        if(data[i]==elem){
            cout<<i<<endl;
            for(;i<count-1;i++){
                data[i]=data[i+1];
            }
            count--;
            return i;
        }
        else if(i==count&&data[i]!=elem){
            cout<<elem<<" is not exist!"<<endl;
            return -1;
        }
    }
    int findElem(T elem){
        int i;
        for(i=0;i<count;i++){
            if(data[i]==elem) break;
        }
        if(data[i]==elem){
            cout<<i<<endl;
            return i;
        }
        else if(i==count&&data[i]!=elem){
            cout<<elem<<" is not exist!"<<endl;
            return -1;
        }
    }
};

int main(){
    MySet<int> intSet;
    MySet<double> douSet;
    MySet<string> strSet;

    int type,move;
    int d1;
    double d2;
    string d3;

    cin>>type;
    while(type!=0){
        cin>>move;
        if(type==1) cin>>d1;
        else if(type==2) cin>>d2;
        else if(type==3) cin>>d3;

        if(type==1){
            if(move==1) intSet.addSet(d1);
            else if(move==2) intSet.deleSet(d1);
            else if(move==3) intSet.findElem(d1);
        }
        else if(type==2){
            if(move==1) douSet.addSet(d2);
            else if(move==2) douSet.deleSet(d2);
            else if(move==3) douSet.findElem(d2);
        }
        else if(type==3){
            if(move==1) strSet.addSet(d3);
            else if(move==2) strSet.deleSet(d3);
            else if(move==3) strSet.findElem(d3);
        }

        cin>>type;
    }
}
```

## 矩阵的乘法运算

### 题目概述

线性代数中的矩阵可以表示为一个row＊column的二维数组，当row和column均为1时，退化为一个数，当row为1时，为一个行向量，当column为1时，为一个列向量。 建立一个整数矩阵类matrix，其私有数据成员如下：

```
int row;
int column;
int **mat;
```

建立该整数矩阵类matrix构造函数； 建立一个 *（乘号）的运算符重载，以便于对两个输入矩阵进行乘法运算； 建立输出函数void display()，对整数矩阵按行进行列对齐输出，格式化输出语句如下：

```
cout<<setw(10)<<mat[i][j];
//需要＃include <iomanip>
```

主函数里定义三个整数矩阵类对象m1、m2、m3.

### 函数接口定义

```
输入格式：
分别输入两个矩阵，分别为整数矩阵类对象m1和m2。 每个矩阵输入如下： 第一行两个整数 r c，分别给出矩阵的行数和列数 接下来输入r行，对应整数矩阵的每一行 每行输入c个整数，对应当前行的c个列元素

输出格式：
整数矩阵按行进行列对齐（宽度为10）后输出 判断m1和m2是否可以执行矩阵相乘运算。 若可以，执行m3=m1*m2运算之后，调用display函数，对m3进行输出。 若不可以，输出"Invalid Matrix multiplication!" 提示：输入或输出的整数矩阵，保证满足row>=1和column>=1。
```

### 输入样例

```
4  5
1 0 0 0 5
0 2 0 0 0
0 0 3 0 0
0 0 0 4 0
5  5
1 2 3 4 5
2 3 4 5 6
3 4 5 6 7
4 5 6 8 9
5 6 7 8 9
```

### 输出样例

```
        26        32        38        44        50
         4         6         8        10        12
         9        12        15        18        21
        16        20        24        32        36
```

### 代码

```
#include <iostream>
#include <iomanip>
using namespace std;

class Matrix{
private:
    int row;
    int column;
    int **mat;
    int flag=1;
public:
    Matrix(){}
    Matrix(int r,int c):row(r),column(c){
        int i;
        mat=new int*[r];
        for(i=0;i<r;i++)
            mat[i]=new int[c];
    }

    friend istream& operator>>(istream &input,Matrix &p){
        int i,j;
        for(i=0;i<p.row;i++)
            for(j=0;j<p.column;j++)
                input>>p.mat[i][j];

        return input;
    }

    Matrix operator*(Matrix &p){
        int i,j,k,num;
        if(this->row==1&&this->column==1){
            Matrix m(p.row,p.column);
            for(i=0;i<p.row;i++)
                for(j=0;j<p.column;j++)
                    m.mat[i][j]=this->mat[0][0]*p.mat[i][j];
            return m;
        }
        else if(p.row==1&&p.column==1){
            Matrix m(this->row,this->column);
            for(i=0;i<p.row;i++)
                for(j=0;j<p.column;j++)
                    m.mat[i][j]=p.mat[0][0]*this->mat[i][j];
            return m;
        }
        else{
            if(this->column!=p.row){
                Matrix m;
                m.flag=0;
                return m;
            }
            else{
                Matrix m(this->row,p.column);

                for(i=0;i<this->row;i++){
                    for(j=0;j<p.column;j++){
                        num=0;
                        for(k=0;k<this->column;k++)
                            num+=this->mat[i][k]*p.mat[k][j];
                        m.mat[i][j]=num;
                    }
                }
                return m;
            }
        }
    }

    void display(){
        int i,j;
        if(flag==1){
            for(i=0;i<row;i++) {
                for (j = 0; j < column; j++)
                    cout << setw(10) << mat[i][j];
                cout<<endl;
            }
        }
        else if(flag==0){
            cout<<"Invalid Matrix multiplication!"<<endl;
        }

    }
};

int main(){
    int row,column;
    int i,j;
    int num;

    cin>>row>>column;
    Matrix m1(row,column);
    cin>>m1;
    cin>>row>>column;
    Matrix m2(row,column);
    cin>>m2;
    Matrix m3;
    m3=m1*m2;

    m3.display();
}
```

## 2017final复数的比较

### 题目概述

请定义一个复数类，实数和虚数是其私有数据成员。再定义一个>（大于号）的运算符重载函数，用于比较两个复数间模的大小。

### 函数接口定义

```
输入格式:
测试输入包含若干测试用例，每个测试用例占一行。每个测试用例包括四个数字，前两个数字分别表示第一个复数的实部和虚部，第三个和第四个数字分别表示第二个复数的实部和虚部。每个数字之间用空格间隔。当读入一个测试用例是0 0 0 0时输入结束，相应的结果不要输出。

输出格式:
对每个测试用例输出一行。当第一个复数的模大于第二个复数的模时，输出 true ，当第一个复数的模小于或等于第二个复数的模时，输出false。
```

### 输入样例

```
4 6 4 0
1 3 4 1
0 0 0 0
```

### 输出样例

```
true
false
```

### 代码

```
#include <iostream>
#include <cmath>
using namespace std;

class Complex{
private:
    double real;
    double imag;
    double dist;
public:
    Complex(double r=0,double i=0):real(r),imag(i){
        dist=pow(pow(r,2)+pow(i,2),0.5);
    }
    bool operator>(Complex &c){
        if(this->dist>c.dist) return true;
        else return false;
    }
};

int main(){
    double r1,i1,r2,i2;

    cin>>r1>>i1>>r2>>i2;
    while(r1!=0||i1!=0||r2!=0||i2!=0){
        Complex c1(r1,i1),c2(r2,i2);
        if(c1>c2) cout<<"true"<<endl;
        else cout<<"false"<<endl;

        cin>>r1>>i1>>r2>>i2;
    }
}
```

## 复数类的运算

### 题目概述

根据以下代码段完善 ?? 处内容及程序内容，以实现规定的输出。

### 函数接口定义

```
输入格式:
输入有两行，分别为两个复数的实部与虚部。

输出格式:
按样例格式输出结果。
```

### 裁判测试程序样例

```
class Complex
{
    public:
        Complex(double r=0, double i=0):real(r), imag(i){   }
        Complex operator+( ?? ) const;//重载双目运算符'+'
        Complex operator-=( ?? ); //重载双目运算符'-='
        friend Complex operator-( ?? ) const;//重载双目运算符'-'
        void Display() const;
    private:
        double real;
        double imag;
};

void Complex::Display() const
{
    cout << "(" << real << ", " << imag << ")" << endl;
}

int main()
{
    double r, m;
    cin >> r >> m;
    Complex c1(r, m);
    cin >> r >> m;
    Complex c2(r, m);
    Complex c3 = c1+c2;
    c3.Display();
    c3 = c1-c2;
    c3.Display();
    c3 -= c1;
    c3.Display();
    return 0;
}
```

### 输入样例

```
4 2
3 -5
```

### 输出样例

```
(7, -3)
(1, 7)
(-3, 5)
```

### 代码

```
#include <iostream>
using namespace std;

class Complex
{
public:
    Complex(double r=0, double i=0):real(r), imag(i){   }
    Complex operator+(Complex &c) const;//重载双目运算符'+'
    Complex operator-=(Complex &c); //重载双目运算符'-='
    friend const Complex operator-(Complex &c1,Complex &c2);//重载双目运算符'-'
    void Display() const;
private:
    double real;
    double imag;
};

void Complex::Display() const
{
    cout << "(" << real << ", " << imag << ")" << endl;
}

Complex Complex::operator+(Complex &c) const {
    Complex t;
    t.real=this->real+c.real;
    t.imag=this->imag+c.imag;
    return t;
}

Complex Complex::operator-=(Complex &c) {
    this->real-=c.real;
    this->imag-=c.imag;
    return *this;
}

Complex const operator-(Complex &c1,Complex &c2) {
    Complex t;
    t.real=c1.real-c2.real;
    t.imag=c1.imag-c2.imag;
    return t;
}

int main()
{
    double r, m;
    cin >> r >> m;
    Complex c1(r, m);
    cin >> r >> m;
    Complex c2(r, m);
    Complex c3 = c1+c2;
    c3.Display();
    c3 = c1-c2;
    c3.Display();
    c3 -= c1;
    c3.Display();
    return 0;
}
```

## 有序数组（类模板）

### 题目概述

实现一个类模板，它可以接受一组数据，能对数据排序，也能输出数组的内容。

每行输入的第一个数字为0，1，2或3:为0时表示输入结束； 为1时表示将输入整数，为2时表示将输入有一位小数的浮点数，为3时表示输入字符。

如果第一个数字非0，则接下来将输入一个正整数，表示即将输入的数据的数量。

从每行第三个输入开始，依次输入指定类型的数据。

### 函数接口定义

```
template <class T>
class MyArray
```

### 裁判测试程序样例

```
#include <iostream>
using namespace std;

/* 请在这里填写答案 */

template<class T>
MyArray<T>::~MyArray(){ delete[] data;}

template<class T>
bool MyArray<T>::check(){
    int i;
    for(i=0;i<size-1;i++)
        if(data[i]>data[i+1]) { cout<<"ERROR!"<<endl;return false;}
    return true;
}
int main( )
{
    MyArray<int> *pI;
    MyArray<float> *pF;
    MyArray<char> *pC;
    int ty, size;
    cin>>ty;
    while(ty>0){
        cin>>size;
        switch(ty){
            case 1: pI = new MyArray<int>(size);   pI->sort(); pI->check(); pI->display(); delete pI; break;
            case 2: pF = new MyArray<float>(size); pF->sort(); pF->check(); pF->display(); delete pF; break;
            case 3: pC = new MyArray<char>(size);  pC->sort(); pC->check(); pC->display(); delete pC; break;
        }
        cin>>ty;
    }
    return 0;
}
```

### 输入样例

```
1 3 2 3 1
2 4 1.5 2.6 3.7 0.5
3 2 A a
0
```

### 输出样例

```
1 2 3
0.5 1.5 2.6 3.7
A a
```

### 代码

```
template <class T>
class MyArray{
private:
    T *data;
    int size;
public:
    MyArray(int s){
        int i;
        T da;

        size=s;
        data=new T[size];
        for(i=0;i<size;i++){
            cin>>da;
            data[i]=da;
        }
    }
    ~MyArray();
    bool check();
    void sort(){
        int i,j;
        T tmp;

        for(i=0;i<size;i++){
            for(j=i;j<size;j++){
                if(data[i]>data[j]){
                    tmp=data[i];
                    data[i]=data[j];
                    data[j]=tmp;
                }
            }
        }
    };
    void display(){
        int i;
        for(i=0;i<size;i++){
            cout<<data[i];
            if(i<size-1) cout<<" ";
        }
        cout<<endl;
    };
};
```

## 数组排序输出（函数模板）

### 题目概述

对于输入的每一批数，按从小到大排序后输出。

一行输入为一批数，第一个输入为数据类型（1表示整数，2表示字符型数，3表示有一位小数的浮点数，4表示字符串，0表示输入结束），第二个输入为该批数的数量size`（0<size<=10）`，接下来为size个指定类型的数据。

输出将从小到大顺序输出数据。

### 函数接口定义

sort函数将接受size个数据，将它们从小到大排序后存在a指向的一段连续空间中。

```
template <class T>
void sort(T *a, int size)；
```

### 裁判测试程序样例

```
#include <iostream>
#include <string>
using namespace std;

/* 请在这里填写答案 */

template <class T>
void display(T* a, int size){
    for(int i=0; i<size-1; i++) cout<<a[i]<<' ';
    cout<<a[size-1]<<endl;
}
int main() {
     const int SIZE=10;
     int a[SIZE];
     char b[SIZE];
     double c[SIZE];
     string d[SIZE];
     int ty, size;
     cin>>ty;
     while(ty>0){
         cin>>size;
         switch(ty){
             case 1:sort(a,size); display(a,size); break;
             case 2:sort(b,size); display(b,size); break;
             case 3:sort(c,size); display(c,size); break;
             case 4:sort(d,size); display(d,size); break;
         }
         cin>>ty;
     }
      return 0;
}
```

### 输入样例

```
1 3 3 2 1
2 2 a A
3 3 1.5 2.6 2.2
4 2 bca abc
0
```

### 输出样例

```
1 2 3
A a
1.5 2.2 2.6
abc bca
```

### 代码

```
template <class T>
void sort(T *a, int size){
    int i,j;
    T tmp;

    for(i=0;i<size;i++){
        cin>>tmp;
        a[i]=tmp;
    }

    for(i=0;i<size;i++){
        for(j=i;j<size;j++){
            if(a[i]>a[j]){
                tmp=a[i];
                a[i]=a[j];
                a[j]=tmp;
            }
        }
    }
}
```

## vector

### 题目概述

本题要求实现一个Vector类模板，能实现数据的存储和访问。通过[]运算符访问时只能访问已经存在的元素，而通过add()方法访问时可以自动扩展内部存储空间。

注意，这个Vector的行为和std::vector是不同的

### 函数接口定义

```
template <class T>
class Vector {
...
```

### 裁判测试程序样例

```
#include <iostream>
using namespace std;

/* 请在这里填写答案 */

int main()
{
    Vector<int> vint;
    int n,m;
    cin >> n >> m;
    for ( int i=0; i<n; i++ ) {
        //  add() can inflate the vector automatically
        vint.add(i);    
    }
    //  get_size() returns the number of elements stored in the vector
    cout << vint.get_size() << endl;
    cout << vint[m] << endl;
    //  remove() removes the element at the index which begins from zero
    vint.remove(m);
    cout << vint.add(-1) << endl;
    cout << vint[m] << endl;
    Vector<int> vv = vint;
    cout << vv[vv.get_size()-1] << endl;
    vv.add(m);
    cout << vint.get_size() << endl;
}
```

### 输入样例

```
100 50
```

### 输出样例

```
100
50
99
51
-1
100
```

### 代码

```
template <class T>
class Vector{
private:
    T *data;
    int size;
public:
    Vector(){
        size=0;
        data=new T;
    }

    Vector(int si){
        size=si;
        data=new T[size];
    }


    int add(T da){
        data[size++]=da;
        return size-1;
    };

    int get_size(){ return size; };
    void remove(int m){
        int i;
        for(i=m;i<size;i++)
            data[i]=data[i+1];
        size--;
    };

    const T &operator[](int m) const{
        if(m>=0&&m<size) return data[m];
        else return -1;
    };

};
```

## 筛法求质数

### 题目概述

本题要求使用筛法求出1~N以内的质数。

### 函数接口定义

```
vector<int> sieve(int n); //函数声明, 求n以内的质数
```

求n以内的质数。其中 n是传入的参数。n 的值不超过10 000 000的范围； 求出的质数存入容器vector并返回。

### 裁判测试程序样例

```
#include <iostream>
#include <vector>
using namespace std;

vector<int> sieve(int n); //函数声明,求n以内的质数

int main(int argc, char const *argv[])
{
    int n;
    cin >> n;

    vector<int> ans = sieve(n);

    cout << ans.size() << endl;
    for (int i = 0; i < ans.size(); i++) {
        cout << ans[i];
        if (i < ans.size() - 1)cout << " ";
    }
    cout << endl;

    return 0;
}

/* 请在这里填写答案 */
```

### 输入样例

输入在一行中给出一个正整数N，其值不超过10 000 000。

```
10
```

### 输出样例

输出首先在一行中输出指定范围内的质数个数，然后在另一行输出指定范围内的所有质数，以空格分隔，但是最后一个质数后面没有多余空格。

```
4
2 3 5 7
```

### 代码

```
int a[10000002]={0};
vector<int> sieve(int n){
    vector<int> ans;
    int i,j;

    for(i=2;i*i<=n;i++){
        if(a[i]==1) continue;
        j=2;
        while(i*j<=n){
            a[i*j]=1;
            j++;
        }
    }

    for(i=2;i<=n;i++){
        if(a[i]!=1) ans.push_back(i);
    }
    return ans;
}
```

## 数据的最大值问题（重载+函数模板）

### 题目概述

两个类如下设计：类time有三个数据成员，hh，mm，ss，分别代表时，分和秒，并有若干构造函数和一个重载-（减号）的成员函数。类date有三个数据成员，year，month，day分别代表年月日，并有若干构造函数和一个重载>（<）（大于号或者小于号）的成员函数。

要求设计一个函数模板template <\class T>\ double maxn(T x[], int len) 对int，float，time和date或者其他类型的数据，返回最大值。

主函数有如下数据成员：

```
int intArray[100];
double douArray[100];time timeArray[100];
date dateArray[100];
```

其中，hh = 3600 ss， mm = 60 ss， year = 365 day， month = 30 day，对于time和date类型，数据在转换成ss或者day后进行运算。

### 函数接口定义

```
输入格式：
每行为一个操作，每行的第一个数字为元素类型，1为整型元素，2为浮点型元素，3为time类型，4为date类型，若为整型元素，接着输入整型数据，以0结束。若为浮点型元素，接着输入浮点型数据，以0结束。若为time型元素， 输入time型数据（hh1 mm1 ss1 hh2 mm2 ss2），以0结束。若为date型数据，输入date型数据（year1 month1 day1 year2 month2 day2），以0结束。输入-1时表示全体输入结束。

输出格式：
对每次输入，输出一个最大值。
```

### 输入样例

```
1 4 5 9 3 7 0
2 4.4 5.5 6.9 3.2 2.7 0
3 18 21 22 18 20 31 18 21 49 0
4 2013 5 14 2013 5 15 2013 4 1 0
-1
```

### 输出样例

```
9
6.9
18 21 49
2013 5 15
```

### 代码

```
#include <iostream>
using namespace std;

template <class T>
double maxn(T x[], int len){
    int i,index;
    index=0;
    for(i=1;i<len;i++){
        if(x[i]>x[index]) index=i;
    }
    cout<<x[index]<<endl;
    return 0;
}

class Time{
private:
    int hh;
    int mm;
    int ss;
public:
    Time(){}
    Time(int h,int m,int s):hh(h),mm(m),ss(s){}

    bool operator>(Time &t){
        if((this->hh*3600+this->mm*60+this->ss)-(t.hh*3600+t.mm*60+t.ss)>0) return true;
        else return false;
    }

    friend ostream& operator<<(ostream &output,Time &t){
        output<<t.hh<<" "<<t.mm<<" "<<t.ss;
        return output;
    }

};

class Date{
private:
    int year;
    int month;
    int day;
public:
    Date(){}
    Date(int y,int m,int d):year(y),month(m),day(d){}
    bool operator>(Date &d){
        if((this->year*365+this->month*30+this->day)-(d.year*365+d.month*30+d.day)>0) return true;
        else return false;
    }

    friend ostream& operator<<(ostream &output,Date &d){
        output<<d.year<<" "<<d.month<<" "<<d.day;
        return output;
    }
};

int main(){
    int intArray[100];
    double douArray[100];
    Time timeArray[100];
    Date dateArray[100];
    int type;
    int cint,cdou,ctime,cdate;
    int di;
    double dd;
    int di1,di2,di3;

    cint=0;
    cdou=0;
    ctime=0;
    cdate=0;

    cin>>type;
    while(type!=-1){
        switch(type){
            case 1:{
                cin>>di;
                while(di!=0){
                    intArray[cint++]=di;
                    cin>>di;
                }
                maxn(intArray,cint);
                break;
            }
            case 2:{
                cin>>dd;
                while(dd!=0){
                    douArray[cdou++]=dd;
                    cin>>dd;
                }
                maxn(douArray,cdou);
                break;
            }
            case 3:{
                cin>>di1;
                while(di1!=0){
                    cin>>di2>>di3;
                    timeArray[ctime++]=*new Time(di1,di2,di3);
                    cin>>di1;
                }
                maxn(timeArray,ctime);
                break;
            }
            case 4:{
                cin>>di1;
                while(di1!=0){
                    cin>>di2>>di3;
                    dateArray[cdate++]=*new Date(di1,di2,di3);
                    cin>>di1;
                }
                maxn(dateArray,cdate);
                break;
            }
        }
        cin>>type;
    }
}
```

## 数据的间距问题

### 题目概述

复数类Complex有两个数据成员：a和b, 分别代表复数的实部和虚部，并有若干构造函数和一个重载-（减号，用于计算两个复数的距离）的成员函数。 要求设计一个函数模板

```
template < class T >
double dist(T a, T b)
```

对int，float，Complex或者其他类型的数据，返回两个数据的间距。

以上类名和函数模板的形式，均须按照题目要求，不得修改

### 函数接口定义

```
输入格式:
每一行为一个操作，每行的第一个数字为元素类型，1为整型元素，2为浮点型元素，3为Complex类型，若为整型元素，接着输入两个整型数据，若为浮点型元素，接着输入两个浮点型数据，若为Complex型元素，输入两个Complex型数据（a1 b1 a2 b2），输入0时标志输入结束。

输出格式:
对每个输入，每行输出一个间距值。
```

### 输入样例

```
1 2 5
3 2 4 5 9
2 2.2 9.9
0
```

### 输出样例

```
3
5.83095
7.7
```

### 代码

```
#include <iostream>
#include <cmath>
using namespace std;

template <class T>
double dist(T a,T b){
    return abs(a-b);
}

class Complex{
private:
    double real;
    double imag;
public:
    Complex(int r,int i):real(r),imag(i){}
    double operator-(Complex &c){
        return pow(pow(this->real-c.real,2)+pow(this->imag-c.imag,2),0.5);
    }
};

int main(){
    int type;
    int di1,di2;
    double dd1,dd2;
    double a1,b1,a2,b2;

    cin>>type;
    while(type!=0){
        switch(type){
            case 1:{
                cin>>di1>>di2;
                cout<<dist(di1,di2)<<endl;
                break;
            }
            case 2:{
                cin>>dd1>>dd2;
                cout<<dist(dd1,dd2)<<endl;
                break;
            }
            case 3:{
                cin>>a1>>b1>>a2>>b2;
                Complex c1(a1,b1),c2(a2,b2);
                cout<<dist(c1,c2)<<endl;
                break;
            }
        }
        cin>>type;
    }
}
```

## 2017final函数模板

### 题目概述

数据的间距问题（函数模板） 类point有三个数据成员：x、y和z, 分别代表x坐标、y坐标和z坐标，并有若干构造函数和一个重载-（减号，计算两点距离）的成员函数。 要求设计一个函数模板，

```
template < class T> double dist(T a, T b)
```

对int，float，point或者其他类型的数据，返回间距。

### 函数接口定义

```
输入格式:
每一行为一个操作，每行的第一个数字为元素类型，1为整型元素，2为浮点型元素，3为point类型，若为整型元素，接着输入两个整型数据，若为浮点型元素，接着输入两个浮点型数据，若为point型元素，输入两个point型数据（x1 y1 z1 x2 y2 z2），输入0时标志输入结束。

输出格式:
对每个输入，每行输出一个间距值。
```

### 输入样例

```
1 2 5
3 2 4 7 5 9 7
2 2.2 9.9
0
```

### 输出样例

```
3
5.83095
7.7
```

### 代码

```
#include <iostream>
#include <cmath>
using namespace std;

template <class T>
double dist(T a,T b){
    return abs(a-b);
}

class point{
private:
    double x;
    double y;
    double z;
public:
    point(double x1,double y1,double z1):x(x1),y(y1),z(z1){}
    double operator-(point &p){
        return pow(pow(this->x-p.x,2)+pow(this->y-p.y,2)+pow(this->z-p.z,2),0.5);
    }
};

int main() {
    int type;
    int di1, di2;
    double dd1, dd2;
    double x1,y1,z1,x2,y2,z2;

    cin >> type;
    while (type != 0) {
        switch (type) {
            case 1: {
                cin >> di1 >> di2;
                cout << dist(di1, di2) << endl;
                break;
            }
            case 2: {
                cin >> dd1 >> dd2;
                cout << dist(dd1, dd2) << endl;
                break;
            }
            case 3: {
                cin >> x1>>y1>>z1>>x2>>y2>>z2;
                point p1(x1,y1,z1),p2(x2,y2,z2);
                cout << dist(p1,p2) << endl;
                break;
            }
        }
        cin >> type;
    }
}
```

## 学号解析

### 题目概述

川师的学号的某些位有特殊的含义，如从2016110101中可以看出该学生为2016级，就读于11系，班级为1班。根据输入的学号，利用程序进行解析，输出对应的信息。

### 函数接口定义

```
输入格式:
一个学号

输出格式:
相关信息
```

### 输入样例

```
2016110101
```

### 输出样例

```
year:2016
department:11
class:01
```

### 代码

```
#include <iostream>
using namespace std;

int main(){
    string str;

    cin>>str;
    cout<<"year:"<<str.substr(0,4)<<endl;
    cout<<"department:"<<str.substr(4,2)<<endl;
    cout<<"class:"<<str.substr(6,2)<<endl;
}
```

## 对称排序

### 题目概述

你供职于由一群丑星作为台柱子的信天翁马戏团。你刚完成了一个程序编写，它按明星们姓名字符串的长度非降序（即当前姓名的长度至少与前一个姓名长度一样）顺序输出他们的名单。然而，你的老板不喜欢这种输出格式，提议输出的首、尾名字长度较短，而中间部分长度稍长，显得有对称性。老板说的具体办法是对已按长度排好序的名单逐对处理，将前者放于当前序列的首部，后者放在尾部。如输入样例中的第一个案例，Bo和Pat是首对名字，Jean和Kevin是第二对，余此类推。

### 函数接口定义

```
输入格式:
输入包含若干个测试案例。每个案例的第一行含一个整数n（n>=1），表示名字串个数。接下来n行每行为一个名字串，这些串是按长度排列的。名字串中不包含空格，每个串至少包含一个字符。n=0为输入结束的标志。

输出格式:
对每一个测试案例，先输出一行“Set n”，其中n从1开始取值，表示案例序号。接着是n行名字输出，如输出样例所示。
```

### 输入样例

```
7
Bo
Pat
Jean
Kevin
Claude
William
Marybeth
6
Jim
Ben
Zoe
Joey
Frederick
Annabelle
5
John
Bill
Fran
Stan
Cece
0
```

### 输出样例

```
SET 1
Bo
Jean
Claude
Marybeth
William
Kevin
Pat
SET 2
Jim
Zoe
Frederick
Annabelle
Joey
Ben
SET 3
John
Fran
Cece
Stan
Bill
```

### 代码

```
#include <iostream>
#include <vector>
using namespace std;

int main(){
    int count=1;
    int i,n;
    vector<string> v;
    vector<string>::iterator p;
    string str;

    cin>>n;
    while(n!=0){
        v.resize(n);
        for(i=0;i<n;i++){
            cin>>str;
            if(i%2==0) v[i/2]=str;
            else v[n-i/2-1]=str;
        }

        cout<<"SET "<<count++<<endl;
        for(p=v.begin();p!=v.end();p++)
            cout<<*p<<endl;

        cin>>n;
    }
}
```

## Score Processing

### 题目概述

Write a program to process students score data.

The input of your program has lines of text, in one of the two formats:

Student's name and student id, as `<student id>, <name>`, and
Score for one student of one course, as `<student id>, <course name>, <marks>`.
Example of the two formats are:

```
3190101234, Zhang San
3190101111, Linear Algebra, 89.5
```

Comma is used as the seperator of each field, and will never be in any of the fields. Notice that there are more than one word for name of the person and name of the course. To make your code easier, the score can be treated as `double`.

The number of the students and the number of the courses are not known at the beginning. The number of lines are not known at the beginning either. The lines of different format appear in no order. One student may not get enrolled in every course.

Your program should read every line in and print out a table of summary in .csv format.

The first line of the output is the table head, consists fields like this:

```
student id, name, <course name 1>, <course name 2>, ..., average
```

where the course names are all the courses read, in alphabet order. There should be one space after each comma.

Then each line of the output is data for one student, in the ascended order of their student id, with score of each course, like:

```
3190101234, Zhang San, 85.0, , 89.5, , , 87.3
```

For the course that hasn't been enrolled, leave a blank before the comma, and should not get included in the average. The average has one decimal place. There should be one space after each comma.

And the last line of the output is a summary line for average score of every course, like:

```
, , 76.2, 87.4, , , 76.8
```

All the number output, including the averages have one decimal place.

### 函数接口定义

```
Input Format
As described in the text above.

Output Format
As described in the text above. The standard output is generated by a program compiled by gcc, that the round of the first decimal place is in the "gcc way".
```

### 输入样例

```
3180111435, Operating System, 34.5
3180111430, Linear Algebra, 80
3180111435, Jessie Zhao
3180111430, Zhiwen Yang
3180111430, Computer Architecture, 46.5
3180111434, Linear Algebra, 61.5
3180111434, Anna Teng
```

### 输出样例

```
student id, name, Computer Architecture, Linear Algebra, Operating System, average
3180111430, Zhiwen Yang, 46.5, 80.0, , 63.2
3180111434, Anna Teng, , 61.5, , 61.5
3180111435, Jessie Zhao, , , 34.5, 34.5
, , 46.5, 70.8, 34.5, 50.6
```

### 代码（答案错误）

```
#include <iostream>
#include <map>
#include <vector>
#include <sstream>
#include <iomanip>
#include <algorithm>
using namespace std;

class Student{
public:
    Student(){ empty_elem(); }
    void empty_elem(){
        name="";
        score.clear();
    }
    string name;
    map<string,double> score;
};

int main(){
    map<string,Student> lst;
    map<string,Student>::iterator p;

    vector<string> cst;
    vector<string>::iterator q;

    map<string,double>::iterator r;

    vector<double> total;
    vector<int> count;

    double stotal;
    int scount;

    double atotal;
    int acount;

    Student stu;
    stringstream ss;
    string str;
    string id,name,cour,tmp;
    int posn1,posn2;
    double scor;
    int i,index;

    getline(cin,str);
    while(str!=""){
        posn1=str.find(", ",0);
        posn2=str.find(", ",posn1+1);

        if(posn2==str.npos){
            id=str.substr(0,posn1);
            name=str.substr(posn1+2,str.length()-posn1-2);

            for(p=lst.begin();p!=lst.end();p++)
                if(p->first==id) break;

            if(p->first==id)
                p->second.name=name;
            else{
                stu.name=name;
                lst.insert(pair<string,Student>(id,stu));
                stu.empty_elem();
            }
        }
        else{
            id=str.substr(0,posn1);
            cour=str.substr(posn1+2,posn2-posn1-2);
            tmp=str.substr(posn2+2,str.length()-posn2-2);

            ss.clear();
            ss.str("");
            ss<<tmp;
            ss>>scor;

            q=find(cst.begin(),cst.end(),cour);
            if(q==cst.end())
                cst.push_back(cour);

            for(p=lst.begin();p!=lst.end();p++){
                if(p->first==id) break;
            }

            if(p->first==id){
                p->second.score.insert(pair<string,double>(cour,scor));
            }
            else{
                stu.score.insert(pair<string,double>(cour,scor));
                lst.insert(pair<string,Student>(id,stu));
                stu.empty_elem();
            }
        }

        getline(cin,str);
    }

    sort(cst.begin(),cst.end());
    total.resize(cst.size());
    count.resize(cst.size());


    cout<<"student id, name, ";
    for(q=cst.begin();q!=cst.end();q++)
        cout<<*q<<", ";
    cout<<"average"<<endl;

   for(p=lst.begin();p!=lst.end();p++){
       stotal=0;
       scount=0;
       index=0;
       cout<<p->first<<", "<<p->second.name<<", ";

       for(q=cst.begin();q!=cst.end();q++){
           for(r=p->second.score.begin();r!=p->second.score.end();r++)
               if(r->first==*q) break;

           if(r->first==*q){
               cout<<setiosflags(ios::fixed)<<setprecision(1)<<r->second;
               stotal+=r->second;
               scount++;
               total[index]+=r->second;
               count[index]++;
           }
           cout<<", ";
           index++;
       }
       if(scount>0)
           cout<<setiosflags(ios::fixed)<<setprecision(1)<<stotal/scount;

       cout<<endl;
   }

   atotal=0;
   acount=0;
   cout<<", , ";
   for(i=0;i<cst.size();i++){
       cout<<total[i]/count[i]<<", ";
       atotal+=total[i];
       acount+=count[i];
   }

   if(acount>0)
       cout<<setiosflags(ios::fixed)<<setprecision(1)<<atotal/acount;

   cout<<endl;
}
```

## 办事大厅排队

### 题目概述

在郑州大学综合办事大厅，每天陆陆续续有很多人来排队办事。现在你能否写程序帮助老师时刻了解当前办理业务的情况。

请同学们学习C++ STL中 list相关内容后，编程实践。

### 函数接口定义

```
输入格式:
第一行一个数字N，表示排队信息或者查询信息条目的数量。

以下N行，每行的内容有以下3种情况

(1) in name 表示名字为name的人员新来到办事大厅，排在队伍的最后。（in和name间存在一个空格，name是名字对应字符串，长度不超过10）。

(2) out 表示当前排在最前面的人已经办理完业务，离开了。

(3) q 表示一次查询，请输出当前正在办理业务的人，也就是队伍的第1个人。如果当前无人办理业务，则输出“NULL”，不包括引号。

输出格式:
请根据以上信息，每次遇到查询时，对应一行输出。如果这时队伍有人，则输出第一个人的姓名，否则输出NULL。
```

### 输入样例

```
5
in A
out
q
in B
q
```

### 输出样例

```
NULL
B
```

### 代码

```
#include <iostream>
#include <list>
using namespace std;

int main(){
    int i,n;
    list<string> lst;
    list<string>::iterator p;
    string str;

    cin>>n;
    getchar();
    for(i=0;i<n;i++){
        getline(cin,str);
        if(str.substr(0,2)=="in"){
            lst.push_back(str.substr(3,str.length()-3));
        }
        else if(str=="out"){
            if(!lst.empty()){
                lst.pop_front();
            }
        }
        else if(str=="q"){
            if(!lst.empty()){
                p=lst.begin();
                cout<<*p<<endl;
            }
            else{
                cout<<"NULL"<<endl;
            }
        }
    }

}
```

## 统计英文单词个数

### 题目概述

给出一篇英文文章，现在需要统计文章中出现英文单词的数量。

### 函数接口定义

```
输入格式:
第一行一个T，代表数据组数

对于每组数据，第一行一个n，代表文章中单词的个数，其后n行每行一个只包含小写字母的长度为1到10的字符串

输出格式:
每组数据输出若干行，每行输出单词以及它出现的次数（中间空格隔开），不同单词按单词字典序从小到大输出

保证单词出现的总次数<=1e5
```

### 输入样例

```
1
8
it
is
a
pen
it
is
a
dog
```

### 输出样例

```
a 2
dog 1
is 2
it 2
pen 1
```

### 代码

```
#include <iostream>
#include <map>
using namespace std;

int main(){
    int t,n;
    int i,j;
    map<string,int> cmap;
    map<string,int>::iterator p;
    string str;

    cin>>t;
    for(i=0;i<t;i++){
        cin>>n;
        for(j=0;j<n;j++){
            cin>>str;

            p=cmap.find(str);
            if(p!=cmap.end()){
                cmap[str]++;
            }
            else{
                cmap.insert(pair<string,int>(str,1));
            }

        }

        for(p=cmap.begin();p!=cmap.end();++p){
            cout<<p->first<<" "<<p->second<<endl;
        }

        cmap.clear();

    }
}
```

## 括号匹配

### 题目概述

给定仅包含`()[]{}`六种括号的字符串，请你判断该字符串中，括号的匹配是否是合法的，也就是对应括号的数量、嵌套顺序完全正确。

### 函数接口定义

```
输入格式:
第一行一个整数T（T<=10）

其后T行每行一个字符串只包含[{()}]六种字符（字符串长度2e5以内）

输出格式:
对于每个字符串，匹配输出Yes,否则输出No
```

### 输入样例

```
2
{()[]}
([)]
```

### 输出样例

```
Yes
No
```

### 代码

```
#include <iostream>
#include <stack>
using namespace std;

int main(){
    int i,n;
    int k;
    string str;
    char ch;
    stack<char> stk;

    cin>>n;
    for(i=0;i<n;i++){
        cin>>str;

        k=0;
        while(str[k]!='\0'){
            if(str[k]=='['||str[k]=='('||str[k]=='{') stk.push(str[k]);
            else if(stk.empty()) break;
            else{
                ch=stk.top();
                if((str[k]==')'&&ch=='(')||(str[k]==']'&&ch=='[')||(str[k]=='}'&&ch=='{')){
                    stk.pop();
                }
                else
                    break;
            }
            k++;
        }
        if(str[k]==0&&stk.empty()) cout<<"Yes"<<endl;
        else{
            cout<<"No"<<endl;
            while(!stk.empty()) stk.pop();
        }
    }
}
```

### 代码2（未验证）

```
#include<stdio.h>
int main(void){
    char str[100];
    int match(char str[]);
    scanf("%s",str);
    if(match(str)==1) printf("True\n");
    else printf("Falue\n");
}

int match(char str[]){
    int i, k, pd;
    char a[100];
    k=0;
    for(i=0;str[i]!='\0';i++){
        pd=0;
        if(str[i]=='('||str[i]=='['||str[i]=='{') a[++k]=str[i];
        else if((str[i]==')'&&a[k]=='(')||(str[i]==']'&&a[k]=='[')||(str[i]=='}'&&a[k]=='{')){
            k--;
            pd=1;
        }
    }
    if(pd==1&&k==0) return 1;
    else return 0;
}
```

## 组最大数

### 题目概述

设有n个正整数，将他们连接成一排，组成一个最大的多位整数。

如:n=3时，3个整数13,312,343连成的最大整数为34331213。

如:n=4时，4个整数7,13,4,246连接成的最大整数为7424613。

### 函数接口定义

```
输入格式:
有多组测试样例，每组测试样例包含两行，第一行为一个整数N（N<=100），第二行包含N个数(每个数不超过1000，空格分开)。

输出格式:
每组数据输出一个表示最大的整数。
```

### 输入样例

```
2
12 123
4
7 13 4 246
```

### 输出样例

```
12312
7424613
```

### 代码

```
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

bool compare(string i,string j){
    return (i+j)>(j+i);
}

int main(){
    int i,n;
    

    while(cin>>n){
        vector<string> v(n,"");
        for(i=0;i<n;i++)
            cin>>v[i];

        sort(v.begin(),v.end(),compare);

        for(i=0;i<n;i++)
            cout<<v[i];

        cout<<endl;
    }

}
```

## Disk Scheduling

### 题目概述

Everyone knows that the files are stored in the magnetic disk. It reads the information according to the horizontal movement of the magnetic head on the track and the rotation of the magnetic disk. In order to reduce the file access time, many excellent disk scheduling algorithms have appeared.

In this problem, Shortest Seek Time First Algorithm (SSTF), which requires that the track to be accessed next is closest to the track where the current magnetic head is located. For example, given that the current magnetic head is on a track of 100, the number of tracks which need to be accessed is (55, 58, 39, 18, 90, 160, 150, 38, 184). Because 90 is the closest track to 100, the head moves to track 90 with a distance of 10 (∣100−90∣), and then finds the closest track to 90 is 58 and the distance is 32 (∣90−58∣), followed by 55 ... Finally, the resulting access sequence is (90, 58, 55, 39, 38, 18, 150, 160, 184), and the sum of the travel distances is ∣100−90∣ + ∣90−58∣ + ∣58−55∣ + ∣55−39∣ + ∣39−38∣ + ∣38−18∣ + ∣18−150∣ + ∣150−160∣ + ∣160−184∣ = 10 + 32 + 3 + 16 + 1 + 20 + 132 + 10 + 24 = 248.

Note that if there are multiple tracks with the smallest distance from the current track, the track with the smallest number will be taken as the next track.

Now given the number of the current magnetic head and the track sequence which you need to access, Little Gyro wants to calculate the sum of the magnetic head's moving distance.

### 函数接口定义

```
Input Specification:
Each input file only contains one test case.

The first line contains two integer n, m (1 ≤ n ≤ 10^6, 1 ≤ m ≤ 10^9), indicating the length of the track sequence and the number of the current magnetic head.

The second line contains n integers a1,a​2​​ ,...,a​n​(1 ≤ ai​​  ≤ 10^​9​​), indicating the track sequence.

Output Specification:
For each test case output an integer indicating the sum of the magnetic head's moving distance.
```

### 输入样例

```
9 100
55 58 39 18 90 160 150 38 184
```

### 输出样例

```
248
```

### 代码（答案错误）

```
#include <iostream>
#include <vector>
using namespace std;

int main(){
    long int n,m;
    long int num;
    long int i,index;
    long int total;
    int flag;
    vector<long int> v;

    flag=0;
    total=0;
    cin>>n>>m;
    for(i=0;i<n;i++){
        cin>>num;
        if(num==m&&flag==0) flag=1;
        else v.push_back(num);
    }

    if(flag==1) n--;

    if(v.begin()==v.end()){
        total=abs(m-v[0]);
    }
    else{
        while(n>=1){
            index=0;
            for(i=0;i<n;i++){
                if(v[i]==m) continue;
                else if((abs(v[i]-m)<abs(v[index]-m))||(abs(v[i]-m)==abs(v[index]-m)&&v[i]==v[index])) index=i;
            }
            total+=(abs(v[index]-m));
            m=v[index];
            v.erase(v.begin()+index);
            n--;
        }
    }

    cout<<total<<endl;
}
```

## stringstream类的使用

### 题目概述

使用 stringstream 实现整数排序。要求把输入保存到在一个stringstream对象中，再这10个整数放到一个整型数组中，将整型数组按大小排序，然后再存回到stringstream对象中。

### 函数接口定义

```
输入格式:
从键盘在一行中输入10个整数，以空格相隔，

输出格式:
输入的字符串，排序前的整型数组，排序后的stringstream对象，整数之间以空格分割，最后一个整数后面没有空格。
```

### 输入样例

```
12 34 65 -23 -32 33 61 99 321 32
```

### 输出样例

```
12 34 65 -23 -32 33 61 99 321 32
12 34 65 -23 -32 33 61 99 321 32
-32 -23 12 32 33 34 61 65 99 321
```

### 代码

```
#include <iostream>
#include <sstream>
using namespace std;

int main(){
    string ss,na,result;
    stringstream stream;
    int tmp,n;
    int i,j;
    int a[10];

    getline(cin,ss);
    cout<<ss<<endl;
    stream<<ss;
    for(i=0;i<10;i++){
        stream>>n;
        a[i]=n;
    }

    for(i=0;i<10;i++){
        n=a[i];
        cout<<n;
        if(i<9) cout<<" ";
    }
    cout<<endl;

    for(i=0;i<10;i++)
        for(j=i;j<10;j++)
            if(a[j]<a[i]){
                tmp=a[i];
                a[i]=a[j];
                a[j]=tmp;
            }

    stream.str("");

    for(i=0;i<10;i++){
        n=a[i];
        stream.clear();
        stream<<n;
        stream>>na;
        result+=na;
        if(i<9) result+=" ";
    }

    cout<<result<<endl;
}
```

## 科学计数法的值

### 题目概述

科学计数法是一种数学专用术语。将一个数表示成 a×10的n次幂的形式，其中1≤|a|<10，n为整数，这种记数方法叫科学计数法。例如920000可以表示为9.2*10^5

现在需要对输入的字符串进行分离，自动识别该科学计数法中的a和幂次，计算其表征的具体数值并输出该值。

例如，对于输入的复数字符串“9.210^5”，输出 The actual value for 9.210^5 is 920000

注意：

1、每组测试数据仅包括一个用于科学计数法的字符串。

2、输入字符串保证合法。

3、字符串长度不超过1000

4、幂次不超过200

### 输入样例

```
9.2*10^5
```

### 输出样例

```
The actual value for 9.2*10^5 is 920000
```

### 代码

```
#include <iostream>
#include <sstream>
using namespace std;

int main(){
    stringstream ss;
    string str,result;
    int i1,i2,j,k;
    int n;

    result="";
    cin>>str;

    i1=str.find('*');
    i2=str.find('.');
    if(i2!=-1){
        result+=str.substr(0,i2);
        result+=str.substr(i2+1,i1-i2-1);
    }
    else{
        result+=str.substr(0,i1);
    }

    j=str.find('^');
    ss<<str.substr(j+1,str.length()-j-1);
    ss>>n;

    if(n>0){
        if(n>=i1-i2-1){
            for(k=0;k<n-(i1-i2-1);k++)
                result+="0";
        }
        else{
            result=result.substr(0,result.length()-(i1-i2-1-n))+"."+result.substr(result.length()-(i1-i2-1-n),i1-i2-1-n);
        }
    }
    else if(n<0){
        for(k=1;k<(-n);k++) result="0"+result;
        result="0."+result;
    }
    else{
        result=str.substr(0,i1);
    }

    cout<<"The actual value for "<<str<<" is "<<result<<endl;
}
```

## 计算平均值

### 题目概述

现在为若干组整数分别计算平均值。

已知这些整数的绝对值都小于100，每组整数的数量不少于1个，不大于20个。

输入格式:首先输入K（不小于2，不大于20）。接下来每一行输入一组数据（至少有一组数据），每组至少有一个数据，在有多个数据时，两个数据之间有1到3个空格。最后一行输入100，标志输入的结束。

输出格式：对于每一组数据，输出其前K个数据的均值，如果该组数据个数少于K时，则输出该组所有数据的均值。输出的均值只输出整数部分，直接忽略小数部分。

### 输入样例

```
3
10 30 20 40
-10 17 10
10 9
100
```

### 输出样例

```
20
5
9
```

### 代码

```
#include <iostream>
using namespace std;

int main(){
    int k,i;
    char ch;
    int n,sum;

    cin>>k;
    getchar();
    cin>>sum;
    ch = getchar();
    while(sum!=100) {
        i=1;
        while (ch != '\n') {
            cin >> n;
            if (i < k) sum += n;
            i++;
            ch = getchar();
        }
        if (i < k) cout << sum / i << endl;
        else cout << sum / k << endl;
        cin>>sum;
        ch=getchar();
    }
}
```

## 美丽的字符正方形FINAL

### 题目概述

输入一个长度不超过50的由小字字母构成的字符串，输出由这个字符序列构成的最大的正方形。

### 函数接口定义

```
输入格式:
由小写字母构成的一个字符串。

输出格式:
将字符串围成最大可能的正方形输出，字符串从正方形的左上方开始，按顺时针方向绕行。
```

### 输入样例

```
a
```

### 输出样例

```
a
```

### 输入样例2

```
happy
```

### 输出样例2

```
ha
pp
```

### 输入样例3

```
abcdefghijklmn
```

### 输出样例3

```
abcd
l  e
k  f
jihg
```

### 代码

```
#include <iostream>
#include <algorithm>
#include <cmath>
using namespace std;

int main(){
    string str,st;
    int i,j;
    int n,len;

    cin>>str;
    n=str.length();
    len=floor(n/4)+1;

    if(len==1){
        cout<<str[0]<<endl;
        return 0;
    }

    cout<<str.substr(0,len)<<endl;

    for(i=0;i<len-2;i++){
        cout<<str[4*len-5-i];
        for(j=0;j<len-2;j++)
            cout<<" ";
        cout<<str[len+i]<<endl;
    }


    st=str.substr(2*len-2,len);
    reverse(st.begin(),st.end());
    cout<<st<<endl;

}
```

## 测试c++

### 题目概述

倒序输出从控制台输入的n个整数

### 函数接口定义

```
输入格式:
第一行输入一个数n，代表行数
依次输入n个整数

输出格式:
将n个整数倒序输出
```

### 输入样例

```
3
1 2 3
```

### 输出样例

```
321
```

### 代码

```
#include <iostream>
using namespace std;

int main(){
    int num[10000];
    int i,n;

    cin>>n;
    for(i=0;i<n;i++)
        cin>>num[i];

    for(i=0;i<n;i++)
        cout<<num[n-i-1];

    cout<<endl;
}
```

## 计算圆的面积

### 题目概述

从键盘输入圆的半径，计算圆的面积并输出。圆周率PI=3.1415926。

### 函数接口定义

```
输入格式:
在这里写输入圆的半径，例如： 3.6

输出格式:
在这里输出圆的面积，例如： 40.715
```

### 输入样例

```
1.5
```

### 输出样例

```
7.06858
```

### 代码

```
#include <iostream>
#define PI 3.1415926
using namespace std;

int main(){
    double r,s;
    cin>>r;
    cout<<PI*r*r<<endl;
}
```

## 计算三角形面积

### 题目概述

从键盘输入三个数，用来表示三角形的三条边长。如果能构成三角形就输出三角形的面积，否则就输出No。

### 函数接口定义

```
输入格式:
请在这里写输入三角形的三条边长，例如： 3.1 4.2 5.3

输出格式:
请在这里输出三角形的面积，例如：6.50661
```

### 输入样例

```
3.0 4.0 5.0
```

### 输出样例

```
6
```

### 代码

```
#include <iostream>
#include <cmath>
using namespace std;

int main(){
    double a,b,c;
    double p,s;
    cin>>a>>b>>c;

    if(a+b<=c || a+c<=b || b+c<=a){
        cout<<"No"<<endl;
        return 0;
    }

    p=(a+b+c)/2;
    s=pow(p*(p-a)*(p-b)*(p-c),0.5);
    cout<<s<<endl;

}
```

## 计算正五边形的面积和周长

### 题目概述

从键盘输入一个数作为正五边形的边长，计算并输出该正五边形的周长和面积。

计算正五边形的面积公式为：$S=\frac{a^{2}\sqrt{25+10\sqrt{5}}}{4}$
​​
### 函数接口定义

```
输入格式:
输入正五边形的边长。例如：
5

输出格式:
输出正五边形的面积和周长。第一行输出面积，第二行输出周长。例如：
43.0119
25
```

### 输入样例

```
16.8
```

### 输出样例

```
485.5875
84
```

### 代码

```
#include <iostream>
#include <cmath>
using namespace std;

int main(){
    double a;
    double c,s;

    cin>>a;
    c=a*5;
    s=pow(a,2)*pow(25+10*pow(5,0.5),0.5)/4;

    cout<<s<<endl<<c<<endl;
}
```

## 问候

### 题目概述

输出问候：Hello!What's your name? 从键盘输入名字，然后输出欢迎信息。

### 函数接口定义

```
输入格式:
请在这里写输入姓名。例如： GaiFuShuai

输出格式:
请在这里描述输出，例如：
Hello!What's your name?
GaiFuShuai,Welcome to learn OOP using C++!
```

### 输入样例

```
BaiFuMei
```

### 输出样例

```
Hello!What's your name?
BaiFuMei,Welcome to learn OOP using C++!
```

### 代码

```
#include <iostream>
using namespace std;

int main(){
    string name;

    cout<<"Hello!What's your name?"<<endl;
    cin>>name;
    cout<<name<<",Welcome to learn OOP using C++!"<<endl;
}
```

## 2017final英文语句格式简单检查

### 题目概述

英文书写中，句首字母通常为大写，其余为小写，单词“I”除外，单词与单词之间用一个空格隔开，句中用“,”断句，句末用“.”结束，“,”和“.”与其前置单词间无需空格隔开。 Word等文本编辑器通常根据以上规则，对我们输入的英文语句进行自动修正。 请编写一个功能，可对输入的一句英文句子，根据以上规则，修订为正确格式后输入。 例如，对于输入的英文句子“This is an Example with one mistake.”， 由于单词“Example”中的字符"E"应该为小写"e"，所以修订后输出该句子的正确格式 This is an example with one mistake.

注意：

1、 每组测试数据仅包括一个以字符“.”结束的英文句子。

2、 输入的英文句子中出现的字符包括二十六个大写英文字母(ASCII码65～90)，二十六个小写英文字母(ASCII码97～122)，“,”和“.”，空格字符。

### 函数接口定义

```
输入格式:
以字符“."为结束字符的一个英文句子。

输出格式:
输入英文句子经过格式纠错后的输出。
```

### 输入样例

```
This is an Example with one mistake.
```

### 输出样例

```
This is an example with one mistake.
```

### 代码

```
#include<iostream>
#include<string>
using namespace std;

char *transform(char *ch){//用于大小写的转换
    if(*ch>='A' && *ch <= 'Z')
        *ch += 32;
    else if(*ch>='a' && *ch <= 'z')
        *ch -= 32;

    return ch;
}

int main()
{
    string str, result;     //str用于输入，result用于接收结果
    int flag=1;                                //flag用于判断是否刚出现过逗号（即是否需要添加空格）,=0为出现逗号，=1为未出现逗号
    int pos=0;                        //pos指向正在被判断的字母
    getline(cin, str);

    if(str[pos]>='a' && str[pos] <= 'z')          //首字母是否为大写
        result.append(transform(&str[pos]), 1);
    else
        result.append(&str[pos], 1);                //

    while(str[++pos] != '.')                        //对每一个字母进行检查，句号不检查
    {
        if(!flag && str[pos] != ' ')                //如果刚出现逗号且正在判断的字母不为空格，则添加空格
        {
            result.append(" ");
            flag = 1;
        }

        if(flag)
        {
            if(str[pos] == 'I' &&
            (str[pos - 1] == ' ' || str[pos - 1] == ',') &&
            (str[pos + 1] == ' ' || str[pos - 1] == ',' || str[pos - 1] == '.')) //如果I为大写，且是单独字符，直接输出
            {
                result.append(&str[pos], 1);
            }
            else if(str[pos] == 'i' &&
            (str[pos - 1] == ' ' || str[pos - 1] == ',') &&
            (str[pos + 1] == ' ' || str[pos + 1] == ',' || str[pos + 1] == '.')) //如果i为小写，且是单独字符，转换后输出
            {
                result.append(transform(&str[pos]), 1);
            }
            else if(str[pos]>='A' && str[pos] <= 'Z')     //如果字母为大写，则转换后输出
            {
                result.append(transform(&str[pos]), 1);
            }
            else if(str[pos] == ',')                        //如果字母为','
            {
                for(int i=1; str[pos + i] != '.'; i++)     //判断：1.如果之后为空格，则继续向后移动 2.如果之后为','、'.'，则将pos位移至该符号 3.如果为大写或小写，则直接输出逗号
                {
                    if(str[pos + i] == ' ')
                        continue;
                    else if(str[pos + i] == ',' || str[pos + i] == '.')
                    {
                        pos += i - 1;
                        break;
                    }
                    else
                    {
                        result.append(&str[pos], 1);
                        flag = 0;
                        break;
                    }
                }
            }
            else if(str[pos] == ' ')    //如果字母为空格
            {
                for(int i=1; str[pos + i] != '.'; i++) //判断：1.如果之后为空格，则继续向后移动 2.如果之后为','、'.'，则将pos位移至该符号 3.如果之后为字母，则输出空格并将pos位移至该字母
                {
                    if(str[pos + i] == ' ')
                        continue;
                    else if(str[pos + i] == ',' || str[pos + i] == '.')
                    {
                        pos += i - 1;
                        break;
                    }
                    else
                    {
                        result.append(" ");
                        pos += i - 1;
                        break;
                    }
                }
            }
            else        //如果判断字符为小写字母，则直接输出
            {
                result.append(&str[pos], 1);
            }
        }
    }

    result.append(".");   //结尾添加句号
    cout << result;
    return 0;
}
```

## 2017Final 圆周率山

### 题目概述

为了参加学校的社团风采展，怡山小学数学组的同学们决定画一座圆周率山，以宣传圆周率。

已知圆周率为：3.
1415926535 8979323846 2643383279 5028841971 6939937510
5820974944 5923078164 0628620899 8628034825 3421170679
8214808651 3282306647 0938446095 5058223172 5359408128
4811174502 8410270193 8521105559 6446229489 5493038196

### 函数接口定义

```
输入格式:
输入山的高度，为一个不超过10的正整数。

输出格式:
以上尖下宽，左右对称的三角形形式，给出圆周率的前若干位（不含小数点）。注意：每行均以数字结尾，即数字右边无空格。
```

### 输入样例

```
1
```

### 输出样例

```
3
```

### 输入样例2

```
4
```

### 输出样例2

```
   3
  141
 59265
3589793
```

### 代码

```
#include <iostream>
using namespace std;

int main(){
    string pi="314159265358979323846264338327950288419716939937510582097494459230781640628620899862803482534211706798214808651328230664709384460955058223172535940812848111745028410270193852110555964462294895493038196";
    int i,j,h;
    int index;

    index=0;
    cin>>h;
    for(i=1;i<=h;i++){
        for(j=0;j<h-i;j++)
            cout<<" ";
        cout<<pi.substr(index,i*2-1)<<endl;
        index += i*2-1;
    }

}
```

## 拆整数

### 代码

```
#include "stdio.h"
int a[100];
int t=0;
int total=0;
void part(int x,int n);
int main(void){
    int n;
    printf("Please enter the number: ");
    scanf("%d",&n);
    part(n-1,n);
}

void part(int x,int n){
    int i,j;
    for(i=x;i>=1;i--)
        if(i+total<=n){
            a[t++]=i;
            total+=i;
            part(i,n);
        } 
    if(total==n){
        printf("%d = ",n);
        for(j=0;j<t;j++){
            printf("%d",a[j]);
            if(j<t-1) printf(" + ");
            else printf("\n");
        }
    }
    t--;
    total-=a[t];
}
```

## 多项式链表计算

### 函数接口定义

```
输入样式：input.txt
```

### 代码

```
#include<stdio.h>
#include<stdlib.h>
FILE *fp;
struct stud_node{
    int num, index;
    struct stud_node *next;
};

struct stud_node *Create_Stu_Doc(int n);
struct stud_node *Copy_Doc(struct stud_node *q);
struct stud_node *AddDoc(struct stud_node *head1,struct stud_node *head2);
struct stud_node *MinusDoc(struct stud_node *head1,struct stud_node *head2);
struct stud_node *MultiDoc(struct stud_node *head1,struct stud_node *head2);
struct stud_node *DeleteDoc(struct stud_node *head);
void Print_Stu_Doc(struct stud_node *head);

int main(void){
    int n1,n2;
    struct stud_node *head1,*head2,*head3;
    struct stud_node *HEAD1,*HEAD2,*HEAD3;
    if((fp=fopen("input.txt","r"))==NULL){
        printf("File open error!\n");
        exit(0);
    }
    fscanf(fp,"%d",&n1);
    head1=Create_Stu_Doc(n1);
    head2=Copy_Doc(head1);
    head3=Copy_Doc(head1);
    fscanf(fp,"%d",&n2);
    HEAD1=Create_Stu_Doc(n2);
    HEAD2=Copy_Doc(HEAD1);
    HEAD3=Copy_Doc(HEAD1);  
    printf("Before calculating:\n");
    printf("\n");
    Print_Stu_Doc(head1);
    printf("\n");
    Print_Stu_Doc(HEAD1);
    printf("\n");
    head1=AddDoc(head1,HEAD1);
    head2=MinusDoc(head2,HEAD2);
    head3=MultiDoc(head3,HEAD3);
    head1=DeleteDoc(head1);
    head2=DeleteDoc(head2);
    head3=DeleteDoc(head3);
    printf("After calculating:\n");
    printf("\n");
    printf("(+)");
    Print_Stu_Doc(head1);
    printf("\n");
    printf("(-)");
    Print_Stu_Doc(head2);
    printf("\n");
    printf("(*)");
    Print_Stu_Doc(head3);
    printf("\n");
} 

struct stud_node *Create_Stu_Doc(int n){
    int i;
    int num, index; 
    int size=sizeof(struct stud_node);
    struct stud_node *head,*tail,*p;
    head=tail=NULL;
    for(i=1;i<=n;i++){
        fscanf(fp,"%d%d",&num,&index);
        p=(struct stud_node*)malloc(size);
        p->num=num;
        p->index=index;
        p->next=NULL;
        if(head==NULL)head=p;
        else tail->next=p;
        tail=p;
    }
    return head;
} 

struct stud_node *Copy_Doc(struct stud_node *q){
    struct stud_node *ptr;
    int size=sizeof(struct stud_node);
    struct stud_node *head,*tail,*p;
    head=tail=NULL;
    for(ptr=q;ptr!=NULL;ptr=ptr->next){
        p=(struct stud_node*)malloc(size);
        p->num=ptr->num;
        p->index=ptr->index;
        p->next=NULL;
        if(head==NULL)head=p;
        else tail->next=p;
        tail=p;
    }
    return head;
} 

struct stud_node *AddDoc(struct stud_node *head1,struct stud_node *head2){
    struct stud_node *ptr1,*ptr2;
    struct stud_node *p;
    for(ptr2=head2;ptr2!=NULL;){
        ptr1=head1;
        while(ptr1->index!=ptr2->index&&ptr1->next!=NULL) ptr1=ptr1->next;
        if(ptr1->index==ptr2->index){
            ptr1->num=ptr1->num+ptr2->num;
            ptr2=ptr2->next;
        }
        else if(ptr1->next==NULL&&ptr1->index!=ptr2->index){
            ptr1->next=ptr2;
            p=ptr2;
            ptr2=ptr2->next;
            p->next=NULL;
        }
    }
    return head1;
} 

struct stud_node *MinusDoc(struct stud_node *head1,struct stud_node *head2){
    struct stud_node *ptr1,*ptr2;
    struct stud_node *p;
    for(ptr2=head2;ptr2!=NULL;){
        ptr1=head1;
        while(ptr1->index!=ptr2->index&&ptr1->next!=NULL) ptr1=ptr1->next;
        if(ptr1->index==ptr2->index){
            ptr1->num=(ptr1->num)-(ptr2->num);
            ptr2=ptr2->next;
        }
        else if(ptr1->next==NULL&&ptr1->index!=ptr2->index){
            ptr2->num=-ptr2->num;
            ptr1->next=ptr2;
            p=ptr2;
            ptr2=ptr2->next;
            p->next=NULL;
        }
    }
    return head1;
} 

struct stud_node *MultiDoc(struct stud_node *head1,struct stud_node *head2){
    int i, j;
    int num, index; 
    int size=sizeof(struct stud_node);
    struct stud_node *head,*tail,*p;
    struct stud_node *str1,*str2;
    head=tail=NULL;
    for(str1=head1;str1!=NULL;str1=str1->next){
        for(str2=head2;str2!=NULL;str2=str2->next){
            p=(struct stud_node*)malloc(size);
            p->num=(str1->num)*(str2->num);
            p->index=(str1->index)+(str2->index);
            p->next=NULL;
            if(head==NULL) head=p;
            else tail->next=p;
            tail=p;
        }
    }
    struct stud_node *ptr1,*ptr2;
    for(ptr1=head;ptr1!=NULL;ptr1=ptr1->next){
        for(ptr2=ptr1->next;ptr2!=NULL;ptr2=ptr2->next){
            if(ptr2->index==ptr1->index){
                ptr1->num=ptr1->num+ptr2->num;
                ptr2->num=0;
            }
        }   
    }   
    return head;
} 

struct stud_node *DeleteDoc(struct stud_node *head){
    struct stud_node *ptr1,*ptr2;
    while(head!=NULL&&head->num==0){
        ptr2=head;
        head=head->next;
        free(ptr2);
    }
    if(head==NULL) return NULL;
    ptr1=head;
    ptr2=head->next;
    while(ptr2!=NULL){
        if(ptr2->num==0){
            ptr1->next=ptr2->next;
            free(ptr2);
        }
        else ptr1=ptr2;
        ptr2=ptr1->next;
    }
    return head;
} 

void Print_Stu_Doc(struct stud_node *head){
    struct stud_node *ptr;
    if(head==NULL){
        printf("NULL\n");
        return;
    }
    for(ptr=head;ptr->next!=NULL;ptr=ptr->next)
        printf("%d*x^%d + ",ptr->num,ptr->index);
    printf("%d*x^%d\n",ptr->num,ptr->index);
} 

```

## 快速排序

### 函数接口定义

```
输入样式：input.txt
输出样式：result.txt
```

### 代码

```
#include<stdio.h>
#include<stdlib.h>
#include<time.h> 

FILE *fp1,*fp2;
void QUICKSORT(int a[], int left, int right);
int PARTITION(int a[], int left, int right);
void SWAP(int a[],int i,int j);
int comp(const void*a,const void*b){
    return *(int*)a-*(int*)b;
}
int main(void){
    int a[100000],b[100000];
    int i, n;
    clock_t t0, t1;
    double total;
    if((fp1=fopen("input.txt","r"))==NULL){
        printf("File open error!\n");
        exit(0);
    }
    printf("Please enter the number of integers fetch from input.txt:");
    scanf("%d",&n);
    for(i=0; i<n; i++){
        fscanf(fp1,"%d",&a[i]);
        b[i]=a[i]; 
    } 
    if((fp2=fopen("result.txt","w+"))==NULL){
        printf("File create error!\n");
        exit(0);
    }
    fprintf(fp2,"Before sorting:\n");
    for(i=0; i<n; i++) fprintf(fp2,"%d ",a[i]);
    fprintf(fp2,"\n");
    t0=clock();
    QUICKSORT(a, 0, n-1);
    t1=clock();
    fprintf(fp2,"After sorting:\n");
    for(i=0; i<n; i++) fprintf(fp2,"%d ",a[i]);
    fprintf(fp2,"\n");
    total=1.0*(t1-t0)/CLOCKS_PER_SEC;
    fprintf(fp2,"Finish sorting in %lf seconds\n",total);
    t0=clock();
    qsort(b,n,sizeof(int),comp);
    t1=clock();
    total=1.0*(t1-t0)/CLOCKS_PER_SEC;
    fprintf(fp2,"Finish sorting by function 'qsort' in %lf seconds\n",total);
    printf("Results have saved in result.txt successfully!\n");
}

void QUICKSORT(int a[], int left, int right){
    int pivot;
    if(left<right){
        pivot=PARTITION(a,left,right);
        QUICKSORT(a,left,pivot-1);
        QUICKSORT(a,pivot+1,right);
    }
}

int PARTITION(int a[], int left, int right){
    int pivotkey;
    pivotkey=a[left];
    while(left<right){
        while(left<right && a[right]>=pivotkey) right--;
        SWAP(a,left,right);
        while(left<right && a[left]<=pivotkey) left++;
        SWAP(a,left,right);
    }
    return left;
}

void SWAP(int a[],int i,int j){
    int temp;
    temp=a[i];
    a[i]=a[j];
    a[j]=temp;
}
```

## 迷宫问题

### 代码

```

```

## 迷宫问题

### 函数接口定义

```
输入样式：maze.txt
```

### 代码

```

```

## 数单词

### 代码

```
#include<stdio.h>
int main(void){
    char c;
    int count, word;
    int repeat, ri;
    scanf("%d",&repeat);
    getchar();
    for(ri=1;ri<=repeat;ri++){
        c=getchar();
        count=0;
        do{
            if(c==' ')word=0;
            else if(c=='\n') break;
            else word=1;
            if(word==1){
                count++;
                while(c!=' '&&c!='\n') c=getchar();
            }
            else if(word==0){
                while(c==' ')c=getchar();
            }
        }while(1);
        printf("count = %d\n",count);
    }
}
```

## 报数

### 题目概述

第一个数是总人数，第二个数是报数的号码，被报号码的人需出列，下一个人由1开始。

### 代码

```
#include<stdio.h>
int main(void)
{
    int count, i, m, n, no;
    int num[50];
    int *p;

    scanf("%d%d", &n, &m);
    for(i = 0; i < n; i++)
        num[i] = i + 1;
    p = num;
    count=0;
    no=0;
    i=1;
    int j=0;
    while(1){
        if(i>n){
            i=1;
            p=num;
        }
        if(*p!=0) count++;
        if(count==m){
            no++;
            printf("No%d: %d\n", no, *p);
            count=0;
            *p=0;
            j++;
        }
        i++,p++;
        if(j==n-1) break;
    }
    p = num;
    while(*p == 0)
        p++;
    printf("Last No is: %d\n", *p);
}
```

## 迷宫问题

### 函数接口定义

```
直接输入
```

### 代码

```
#include<stdio.h>
struct PosType{
    int x;
    int y;
};

int x=10;
int y=10;
int m[10][10];
struct PosType end;
void Print(int x,int y);
void Try(struct PosType cur);

int main(){
    struct PosType begin;
    int i, j, x1, y1;
    for(i=0;i<x;i++){
        m[0][i]=0;
        m[x-1][i]=0;
    }
    for(j=1;j<y-1;j++){
        m[j][0]=0;
        m[j][y-1]=0;
    }
    for(i=1;i<x-1;i++)
        for(j=1;j<y-1;j++) m[i][j]=1;
    printf("迷宫初始结构如下：\n"); 
    Print(x,y);
    printf("请输入迷宫内墙单元数：");
    scanf("%d",&j);
    if(j)
        printf("请输入迷宫内墙每个单元的行数、列数，以空格隔开：\n");
    for(i=1;i<=j;i++){
        scanf("%d%d",&x1,&y1);
        m[x1][y1]=0;
    }
    printf("现迷宫结构如下：\n"); 
    Print(x,y);
    printf("请输入起点的行数、列数，以空格隔开：");
    scanf("%d%d",&begin.x,&begin.y);
    printf("请输入终点的行数、列数，以空格隔开：");
    scanf("%d%d",&end.x,&end.y);
    m[begin.x][begin.y]=2;
    Try(begin);
}

void Print(int x,int y){
    int i, j;
    printf("  0 1 2 3 4 5 6 7 8 9\n");
    for(i=0;i<x;i++){
        printf("%d ",i);
        for(j=0;j<y;j++){
            if(m[i][j]==0) printf("# ");
            else if(m[i][j]==1) printf("  ");
            else if(m[i][j]==2) printf("+ ");
            }
        printf("\n");
    } 
    printf("\n");
}

void Try(struct PosType cur){
    int i;
    struct PosType next;
    struct PosType direc[4]={{0,1},{1,0},{0,-1},{-1,0}};
    for(i=0;i<=3;i++){
        next.x=cur.x+direc[i].x;
        next.y=cur.y+direc[i].y;
        if(m[next.x][next.y]==1){
            m[next.x][next.y]=2;
            if(next.x==end.x&&next.y==end.y){
                printf("路线为：\n");
                Print(x,y);
                return;
            }
            else Try(next);
            m[next.x][next.y]=1;
        }
    }
}
```

## 迷宫问题

### 函数接口定义

```
输入：maze.txt
```

### 代码

```
#include<stdio.h>
#include<stdlib.h>
struct PosType{
    int x;
    int y;
};

int x;
int y;
FILE *fp;
int m[100][100];
struct PosType end;
void Print(int x,int y);
void Try(struct PosType cur);

int main(){
    struct PosType begin;
    int i, j;
    if((fp=fopen("maze.txt","r"))==NULL){
        printf("文件打开失败！");
        exit(0);
    }
    printf("请输入迷宫的长：");
    scanf("%d",&x);
    printf("请输入迷宫的宽：");
    scanf("%d",&y);
    for(i=0;i<x;i++)
        for(j=0;j<x;j++)
            fscanf(fp,"%d",&m[i][j]);
    printf("迷宫结构如下：\n");
    Print(x,y);
    printf("请输入起点的行数、列数，以空格隔开：");
    scanf("%d%d",&begin.x,&begin.y);
    printf("请输入终点的行数、列数，以空格隔开：");
    scanf("%d%d",&end.x,&end.y);
    m[begin.x][begin.y]=2;
    Try(begin);
}

void Print(int x,int y){
    int i, j;
    for(i=0;i<x;i++){
        for(j=0;j<y;j++){
            if(m[i][j]==0) printf("# ");
            else if(m[i][j]==1) printf("  ");
            else if(m[i][j]==2) printf("+ ");
            }
        printf("\n");
    } 
    printf("\n");
}

void Try(struct PosType cur){
    int i;
    struct PosType next;
    struct PosType direc[4]={{0,1},{1,0},{0,-1},{-1,0}};
    for(i=0;i<=3;i++){
        next.x=cur.x+direc[i].x;
        next.y=cur.y+direc[i].y;
        if(m[next.x][next.y]==1){
            m[next.x][next.y]=2;
            if(next.x==end.x&&next.y==end.y){
                printf("路线为：\n");
                Print(x,y);
                return;
            }
            else Try(next);
            m[next.x][next.y]=1;
        }
    }
}
```










































# 数据结构

## 哈夫曼树

### 题目概述

哈夫曼树，第一行输入一个数n，表示叶结点的个数。

需要用这些叶结点生成哈夫曼树，根据哈夫曼树的概念，这些结点有权值，即weight，题目需要输出哈夫曼树的带权路径长度（WPL）。

### 函数接口定义

```
输入格式:
第一行输入一个数n，第二行输入n个叶结点（叶结点权值不超过1000，2<=n<=1000）。

输出格式:
在一行中输出WPL值。
```

### 输入样例

```
5
1 2 2 5 9
```

### 输出样例

```
37
```

### 代码

```
#include <iostream>
#include <queue>
using namespace std;

int main(){
    int i,n;
    int num;
    int result=0;
    int tmp1,tmp2;
    priority_queue<int,vector<int>,greater<int> > q;

    cin>>n;
    for(i=0;i<n;i++){
        cin>>num;
        q.push(num);
    }

    while(q.size()>1){
        tmp1=q.top();
        q.pop();
        tmp2=q.top();
        q.pop();
        q.push(tmp1+tmp2);
        result+=(tmp1+tmp2);
    }

    cout<<result<<endl;
}
```

## 把二叉树打印成多行

### 题目概述

从上到下按层打印二叉树，同一层结点从左至右输出。每一层输出一行，数据之间用一个空格分隔，行尾无多余空格。

### 函数接口定义

```
struct TreeNode {
    int val;
    struct TreeNode *left;
    struct TreeNode *right;
    TreeNode(int x) :
        val(x), left(NULL), right(NULL) {
    }
};

vector<vector<int> > Print(TreeNode* pRoot);
```

其中 pRoot为指向二叉树的根结点的指针。函数须返回二叉树层次遍历结果。

### 裁判测试程序样例

```
#include <bits/stdc++.h>
using namespace std;

struct TreeNode {
    int val;
    struct TreeNode *left;
    struct TreeNode *right;
    TreeNode(int x) :
        val(x), left(NULL), right(NULL) {
    }
};

class Solution {
public:

    /* 请在这里填写答案 */

};
```

### 输入样例

![](https://tva1.sinaimg.cn/large/0081Kckwly1gk9rp9290qj306f052q2w.jpg)

### 输出样例

```
7
2 6
9 4 8 5
10 3
```

### 代码

```
    vector<vector<int> >  Print(TreeNode* pRoot){
        vector<vector<int> > result;
        vector<int> line;
        queue<TreeNode* > q;
        int i,size;

        if(pRoot==NULL) return result;

        q.push(pRoot);
        while(!q.empty()){
            size=q.size();
            for(i=0;i<size;i++){
                line.push_back(q.front()->val);
                if(q.front()->left!=NULL) q.push(q.front()->left);
                if(q.front()->right!=NULL) q.push(q.front()->right);
                q.pop();
            }
            result.push_back(line);

            while(!line.empty())
                line.pop_back();
        }

        return result;
    }
```

## Add Two Polynomials

### 题目概述

Write a function to add two polynomials. Do not destroy the input. Use a linked list implementation with a dummy head node. Note: The zero polynomial is represented by an empty list with only the dummy head node.

### 函数接口定义

```
Polynomial Add( Polynomial a, Polynomial b );
```

where `Polynomial` is defined as the following:

```
typedef struct Node *PtrToNode;
struct Node {
    int Coefficient;
    int Exponent;
    PtrToNode Next;
};
typedef PtrToNode Polynomial;
/* Nodes are sorted in decreasing order of exponents.*/  
```

The function `Add` is supposed to return a polynomial which is the sum of `a` and `b`.

### 裁判测试程序样例

```
#include <stdio.h>
#include <stdlib.h>
typedef struct Node *PtrToNode;
struct Node  {
    int Coefficient;
    int Exponent;
    PtrToNode Next;
};
typedef PtrToNode Polynomial;

Polynomial Read(); /* details omitted */
void Print( Polynomial p ); /* details omitted */
Polynomial Add( Polynomial a, Polynomial b );

int main()
{
    Polynomial a, b, s;
    a = Read();
    b = Read();
    s = Add(a, b);
    Print(s);
    return 0;
}

/* Your function will be put here */
```

### 输入样例

```
4
3 4 -5 2 6 1 -2 0
3
5 20 -7 4 3 1
```

### 输出样例

```
5 20 -4 4 -5 2 9 1 -2 0
```

### 代码

```
Polynomial Add( Polynomial a, Polynomial b ){
    if(a->Next==NULL&&a->Coefficient==0&&a->Exponent==0) return b;
    if(b->Next==NULL&&b->Coefficient==0&&b->Exponent==0) return a;

    Polynomial prev1=a;
    Polynomial prev2=b;
    Polynomial node=(Polynomial)malloc(sizeof(struct Node));
    Polynomial head=node;
    Polynomial tail=node;

    if(a->Exponent>b->Exponent){
        node->Coefficient=a->Coefficient;
        node->Exponent=a->Exponent;
        prev1=prev1->Next;
    }
    else if(a->Exponent<b->Exponent) {
        node->Coefficient = b->Coefficient;
        node->Exponent = b->Exponent;
        prev2=prev2->Next;
    }
    else{
        node->Coefficient = a->Coefficient+b->Coefficient;
        node->Exponent = b->Exponent;
        prev1=prev1->Next;
        prev2=prev2->Next;
    }
    node->Next=NULL;

    while(prev1!=NULL || prev2!=NULL) {
        if((prev1!=NULL &&prev2 == NULL) || prev1->Exponent > prev2->Exponent) {
            while ((prev1!=NULL &&prev2 == NULL) || (prev1!=NULL && prev2!=NULL &&prev1->Exponent > prev2->Exponent)) {
                Polynomial node=(Polynomial)malloc(sizeof(struct Node));
                node->Coefficient = prev1->Coefficient;
                node->Exponent = prev1->Exponent;
                node->Next = NULL;
                prev1 = prev1->Next;
                tail->Next = node;
                tail = tail->Next;
            }
        }
        else if((prev1==NULL &&prev2 != NULL) || (prev1!=NULL && prev2!=NULL &&prev1->Exponent < prev2->Exponent)) {
            while ((prev1==NULL &&prev2 != NULL) || prev1->Exponent < prev2->Exponent) {
                Polynomial node=(Polynomial)malloc(sizeof(struct Node));
                node->Coefficient = prev2->Coefficient;
                node->Exponent = prev2->Exponent;
                node->Next = NULL;
                prev2 = prev2->Next;
                tail->Next = node;
                tail = tail->Next;
            }
        }
        else if (prev1!=NULL && prev2!=NULL &&prev1->Exponent == prev2->Exponent) {
            if(prev1->Coefficient + prev2->Coefficient!=0){
                Polynomial node=(Polynomial)malloc(sizeof(struct Node));
                node->Coefficient = prev1->Coefficient + prev2->Coefficient;
                node->Exponent = prev2->Exponent;
                node->Next=NULL;
                tail->Next = node;
                tail = tail->Next;
            }
            prev1=prev1->Next;
            prev2 = prev2->Next;

        }
    }
    return head;
}
```

## Reverse Linked List

### 题目概述

Write a nonrecursive procedure to reverse a singly linked list in O(N) time using constant extra space.

### 函数接口定义

```
List Reverse( List L );
```

where List is defined as the following:

```
typedef struct Node *PtrToNode;
typedef PtrToNode List;
typedef PtrToNode Position;
struct Node {
    ElementType Element;
    Position Next;
};
```

The function Reverse is supposed to return the reverse linked list of L, with a dummy header.

### 裁判测试程序样例

```
#include <stdio.h>
#include <stdlib.h>

typedef int ElementType;
typedef struct Node *PtrToNode;
typedef PtrToNode List;
typedef PtrToNode Position;
struct Node {
    ElementType Element;
    Position Next;
};

List Read(); /* details omitted */
void Print( List L ); /* details omitted */
List Reverse( List L );

int main()
{
    List L1, L2;
    L1 = Read();
    L2 = Reverse(L1);
    Print(L1);
    Print(L2);
    return 0;
}

/* Your function will be put here */
```

### 输入样例

```
5
1 3 4 5 2
```

### 输出样例

```
2 5 4 3 1
2 5 4 3 1
```

### 代码

```
List Reverse( List L )
{
    
    if(L==NULL||L->Next==NULL) return L;

    List node=(List)malloc(sizeof(struct Node));
    List tp=L->Next;
    List temp;

    node->Next=NULL;

    while(tp!=NULL){
        temp=tp->Next;
        tp->Next=node->Next;
        node->Next=tp;
        tp=temp;
    }

    List tail=L->Next;
    List head=L;
    head->Next=node->Next;
    tail->Next=NULL;

    return node;

}
```

## Evaluate Postfix Expression

### 题目概述

Write a program to evaluate a postfix expression. You only have to handle four kinds of operators: +, -, x, and /.

### 函数接口定义

```
ElementType EvalPostfix( char *expr );
```

where expr points to a string that stores the postfix expression. It is guaranteed that there is exactly one space between any two operators or operands. The function EvalPostfix is supposed to return the value of the expression. If it is not a legal postfix expression, EvalPostfix must return a special value Infinity which is defined by the judge program.

### 裁判测试程序样例

```
#include <stdio.h>
#include <stdlib.h>

typedef double ElementType;
#define Infinity 1e8
#define Max_Expr 30   /* max size of expression */

ElementType EvalPostfix( char *expr );

int main()
{
    ElementType v;
    char expr[Max_Expr];
    gets(expr);
    v = EvalPostfix( expr );
    if ( v < Infinity )
        printf("%f\n", v);
    else
        printf("ERROR\n");
    return 0;
}

/* Your function will be put here */
```

### 输入样例

```
11 -2 5.5 * + 23 7 / -
```

### 输出样例

```
-3.285714
```

### 输入样例2

```
11 -2 5.5 * + 23 0 / -
```

### 输出样例2

```
ERROR
```

### 输入样例3

```
11 -2 5.5 * + 23 7 / - *
```

### 输出样例3

```
ERROR
```

### 代码

```
ElementType EvalPostfix( char *expr ){
    ElementType stack[Max_Expr]={0};
    int i,s;
    int pflag=0; // 负数
    int sflag=0; // 小数
    int nflag=0; //
    ElementType base=10;
    s=0;
    i=0;

    while(expr[i]!='\0'){
        if(expr[i]>='0'&&expr[i]<='9'){
            if(nflag==1){
                s++;
                nflag=0;
            }
            if(sflag==1){
                stack[s]+=(ElementType)(expr[i]-'0')/(ElementType)base;
                if(pflag==1&(expr[i+1]=='\0'||expr[i+1]==' ')){
                    stack[s]*=-1;
                    pflag=0;
                }
                else base*=10;
            }
            else{
                stack[s]=10*stack[s]+expr[i]-'0';
                if(pflag==1&(expr[i+1]=='\0'||expr[i+1]==' ')){
                    stack[s]*=-1;
                    pflag=0;
                }
            }
        }
        else if(expr[i]=='.')
            sflag=1;
        else if(expr[i]==' '){
            sflag=0;
            nflag=1;
            base=10;
        }
        else if(expr[i]=='+'){
            if(s==0) return Infinity;
            stack[s-1]=stack[s-1]+stack[s];
            stack[s]=0;
            s--;
        }

        else if(expr[i]=='-'){
            if(expr[i+1]>='0'&&expr[i+1]<='9') pflag=1;
            else{
                if(s==0) return Infinity;
                stack[s-1]=stack[s-1]-stack[s];
                stack[s]=0;
                s--;
            }
        }
        else if(expr[i]=='*'){
            if(s==0) return Infinity;
            stack[s-1]=stack[s-1]*stack[s];
            stack[s]=0;
            s--;
        }

        else if(expr[i]=='/'){
            if(s==0) return Infinity;
            if(stack[s]==0) return Infinity;
            stack[s-1]=(ElementType)stack[s-1]/(ElementType)stack[s];
            stack[s]=0;
            s--;
        }

        i++;
    }

    if(s==0) return stack[0];
    else return Infinity;
}
```

## Two Stacks In One Array

### 题目概述

Write routines to implement two stacks using only one array. Your stack routines should not declare an overflow unless every slot in the array is used.

### 函数接口定义

```
Stack CreateStack( int MaxElements );
int IsEmpty( Stack S, int Stacknum );
int IsFull( Stack S );
int Push( ElementType X, Stack S, int Stacknum );
ElementType Top_Pop( Stack S, int Stacknum );
```

where `int Stacknum` is the index of a stack which is either 1 or 2; `int MaxElements` is the size of the `stack` array; and Stack is defined as the following:

```
typedef struct StackRecord *Stack;
struct StackRecord  {
    int Capacity;       /* maximum size of the stack array */
    int Top1;           /* top pointer for Stack 1 */
    int Top2;           /* top pointer for Stack 2 */
    ElementType *Array; /* space for the two stacks */
}
```

Note: `Push` is supposed to return 1 if the operation can be done successfully, or 0 if fails. If the stack is empty, `Top_Pop` must return `ERROR` which is defined by the judge program.

### 裁判测试程序样例

```
#include <stdio.h>
#include <stdlib.h>
#define ERROR 1e8
typedef int ElementType;
typedef enum { push, pop, end } Operation;

typedef struct StackRecord *Stack;
struct StackRecord  {
    int Capacity;       /* maximum size of the stack array */
    int Top1;           /* top pointer for Stack 1 */
    int Top2;           /* top pointer for Stack 2 */
    ElementType *Array; /* space for the two stacks */
};

Stack CreateStack( int MaxElements );
int IsEmpty( Stack S, int Stacknum );
int IsFull( Stack S );
int Push( ElementType X, Stack S, int Stacknum );
ElementType Top_Pop( Stack S, int Stacknum );

Operation GetOp();  /* details omitted */
void PrintStack( Stack S, int Stacknum ); /* details omitted */

int main()
{
    int N, Sn, X;
    Stack S;
    int done = 0;

    scanf("%d", &N);
    S = CreateStack(N);
    while ( !done ) {
        switch( GetOp() ) {
        case push: 
            scanf("%d %d", &Sn, &X);
            if (!Push(X, S, Sn)) printf("Stack %d is Full!\n", Sn);
            break;
        case pop:
            scanf("%d", &Sn);
            X = Top_Pop(S, Sn);
            if ( X==ERROR ) printf("Stack %d is Empty!\n", Sn);
            break;
        case end:
            PrintStack(S, 1);
            PrintStack(S, 2);
            done = 1;
            break;
        }
    }
    return 0;
}

/* Your function will be put here */
```

### 输入样例

```
5
Push 1 1
Pop 2
Push 2 11
Push 1 2
Push 2 12
Pop 1
Push 2 13
Push 2 14
Push 1 3
Pop 2
End
```

### 输出样例

```
Stack 2 is Empty!
Stack 1 is Full!
Pop from Stack 1: 1
Pop from Stack 2: 13 12 11
```

### 代码

```
Stack CreateStack( int MaxElements ){
    Stack s=(Stack)malloc(sizeof(struct StackRecord));
    s->Capacity=MaxElements;
    s->Top1=0;
    s->Top2=MaxElements-1;
    s->Array=(ElementType*)malloc(MaxElements*sizeof(ElementType));
    return s;
}

int IsEmpty( Stack S, int Stacknum ){
    if(Stacknum==1){
        if(S->Top1==0) return 1;
        else return 0;
    }
    else if(Stacknum==2){
        if(S->Top2==S->Capacity-1) return 1;
        else return 0;
    }
}

int IsFull( Stack S ){
    if(S->Top2+1==S->Top1) return 1;
    else return 0;
}

int Push( ElementType X, Stack S, int Stacknum ){
    if(IsFull(S)) return 0;
    if(Stacknum==1){
        S->Array[S->Top1++]=X;
    }
    else if(Stacknum==2){
        S->Array[S->Top2--]=X;
    }
    return 1;
}

ElementType Top_Pop( Stack S, int Stacknum ){
    if(IsEmpty(S,Stacknum)) return ERROR;
    if(Stacknum==1){
        return S->Array[--S->Top1];
    }
    else if(Stacknum==2){
        return S->Array[++S->Top2];
    }
}
```

## Deque

### 题目概述

A "deque" is a data structure consisting of a list of items, on which the following operations are possible:

```
Push(X,D): Insert item X on the front end of deque D.
Pop(D): Remove the front item from deque D and return it.
Inject(X,D): Insert item X on the rear end of deque D.
Eject(D): Remove the rear item from deque D and return it. Write routines to support the deque that take O(1) time per operation.
```

### 函数接口定义

```
Deque CreateDeque();
int Push( ElementType X, Deque D );
ElementType Pop( Deque D );
int Inject( ElementType X, Deque D );
ElementType Eject( Deque D );
```

where `Deque` is defined as the following:

```
typedef struct Node *PtrToNode;
struct Node {
    ElementType Element;
    PtrToNode Next, Last;
};
typedef struct DequeRecord *Deque;
struct DequeRecord {
    PtrToNode Front, Rear;
};
```

Here the deque is implemented by a doubly linked list with a header. `Front` and `Rear` point to the two ends of the deque respectively. `Front` always points to the header. The deque is empty when `Front` and `Rear` both point to the same dummy header. Note: `Push` and `Inject` are supposed to return 1 if the operations can be done successfully, or 0 if fail. If the deque is empty, `Pop` and `Eject` must return `ERROR` which is defined by the judge program.

### 裁判测试程序样例

```
#include <stdio.h>
#include <stdlib.h>

#define ElementType int
#define ERROR 1e5
typedef enum { push, pop, inject, eject, end } Operation;

typedef struct Node *PtrToNode;
struct Node {
    ElementType Element;
    PtrToNode Next, Last;
};
typedef struct DequeRecord *Deque;
struct DequeRecord {
    PtrToNode Front, Rear;
};
Deque CreateDeque();
int Push( ElementType X, Deque D );
ElementType Pop( Deque D );
int Inject( ElementType X, Deque D );
ElementType Eject( Deque D );

Operation GetOp();          /* details omitted */
void PrintDeque( Deque D ); /* details omitted */

int main()
{
    ElementType X;
    Deque D;
    int done = 0;

    D = CreateDeque();
    while (!done) {
        switch(GetOp()) {
        case push: 
            scanf("%d", &X);
            if (!Push(X, D)) printf("Memory is Full!\n");
            break;
        case pop:
            X = Pop(D);
            if ( X==ERROR ) printf("Deque is Empty!\n");
            break;
        case inject: 
            scanf("%d", &X);
            if (!Inject(X, D)) printf("Memory is Full!\n");
            break;
        case eject:
            X = Eject(D);
            if ( X==ERROR ) printf("Deque is Empty!\n");
            break;
        case end:
            PrintDeque(D);
            done = 1;
            break;
        }
    }
    return 0;
}

/* Your function will be put here */
```

### 输入样例

```
Pop
Inject 1
Pop
Eject
Push 1
Push 2
Eject
Inject 3
End
```

### 输出样例

```
Deque is Empty!
Deque is Empty!
Inside Deque: 2 3
```

### 代码

```

Deque CreateDeque(){
    Deque m=(Deque)malloc(sizeof(struct DequeRecord));
    PtrToNode tmp=(PtrToNode)malloc(sizeof(struct Node));
    tmp->Last=NULL;
    tmp->Next=NULL;
    m->Front=tmp;
    m->Rear=tmp;
    return m;
}

int Push( ElementType X, Deque D ){
    PtrToNode tmp=(PtrToNode)malloc(sizeof(struct Node));
    if(!tmp) return 0;
    tmp->Element=X;

    if(D->Front==D->Rear){
        tmp->Next=NULL;
        tmp->Last=D->Front;
        D->Front->Next=tmp;
        D->Rear=tmp;
    }
    else{
        tmp->Next=D->Front->Next;
        tmp->Last=D->Front;
        D->Front->Next->Last=tmp;
        D->Front->Next=tmp;
    }

    return 1;

}

ElementType Pop( Deque D ){
    if(D->Front==D->Rear) return ERROR;
    ElementType elem;
    elem=D->Front->Next->Element;

    PtrToNode next=D->Front->Next;

    if(next==D->Rear){
        D->Rear=D->Front;
        D->Rear->Next=NULL;
    }
    else{
        D->Front->Next=next->Next;
        next->Next->Last=D->Front;
    }

    free(next);
    return elem;
}

int Inject( ElementType X, Deque D ){
    PtrToNode tmp=(PtrToNode)malloc(sizeof(struct Node));
    if(!tmp) return 0;
    tmp->Element=X;


    if(D->Front==D->Rear){
        D->Front->Next=tmp;
        tmp->Last=D->Front;
        D->Rear=tmp;
    }
    else{
        D->Rear->Next=tmp;
        tmp->Next=NULL;
        tmp->Last=D->Rear;
        D->Rear=tmp;
    }
}

ElementType Eject( Deque D ){
    if(D->Front==D->Rear) return ERROR;
    ElementType elem;
    elem=D->Rear->Element;

    PtrToNode rear=D->Rear;

    if(D->Front->Next==rear){
        D->Rear=D->Front;
        D->Rear->Next=NULL;
    }
    else{
        D->Rear=D->Rear->Last;
        D->Rear->Next=NULL;
    }

    free(rear);
    return elem;
}
```

## Pop Sequence

### 题目概述

Given a stack which can keep M numbers at most. Push N numbers in the order of 1, 2, 3, ..., N and pop randomly. You are supposed to tell if a given sequence of numbers is a possible pop sequence of the stack. For example, if M is 5 and N is 7, we can obtain 1, 2, 3, 4, 5, 6, 7 from the stack, but not 3, 2, 1, 7, 5, 6, 4.

### 函数接口定义

```
Input Specification:
Each input file contains one test case. For each case, the first line contains 3 numbers (all no more than 1000): M (the maximum capacity of the stack), N (the length of push sequence), and K (the number of pop sequences to be checked). Then K lines follow, each contains a pop sequence of N numbers. All the numbers in a line are separated by a space.

Output Specification:
For each pop sequence, print in one line "YES" if it is indeed a possible pop sequence of the stack, or "NO" if not.
```

### 输入样例

```
5 7 5
1 2 3 4 5 6 7
3 2 1 7 5 6 4
7 6 5 4 3 2 1
5 6 4 3 7 2 1
1 7 6 5 4 3 2
```

### 输出样例

```
YES
NO
NO
YES
NO
```

### 代码

```
#include <iostream>
#include <stack>
#include <queue>
using namespace std;

int main(){
    int m,n,k;
    int i,j;
    int num;
    int tmp;
    queue<int> qu;
    stack<int> st;


    cin>>m>>n>>k;
    for(i=0;i<k;i++){
        for(j=0;j<n;j++){
            cin>>num;
            qu.push(num);
        }

        tmp=1;
        while(tmp<=n){
            st.push(tmp);
            if(st.size()==m){
                if(qu.front()!=st.top()) break;
            }

            while((!qu.empty())&&(!st.empty())&&qu.front()==st.top()) {
                qu.pop();
                st.pop();
            }
            tmp++;
        }

        if(qu.size()==0&&st.size()==0) cout<<"YES"<<endl;
        else cout<<"NO"<<endl;

        while(!qu.empty()) qu.pop();
        while(!st.empty()) st.pop();

    }
}
```

## Isomorphic

### 题目概述

Two trees, T1 and T2, are isomorphic if T1 can be transformed into T2 by swapping left and right children of (some of the) nodes in T1. For instance, the two trees in Figure 1 are isomorphic because they are the same if the children of A, B, and G, but not the other nodes, are swapped. Give a polynomial time algorithm to decide if two trees are isomorphic.

![](https://tva1.sinaimg.cn/large/0081Kckwly1gk9xihb9q8j30cd044q3z.jpg)

### 函数接口定义

```
int Isomorphic( Tree T1, Tree T2 );
```

where `Tree` is defined as the following:

```
typedef struct TreeNode *Tree;
struct TreeNode {
    ElementType Element;
    Tree  Left;
    Tree  Right;
};
```

The function is supposed to return 1 if T1 and T2 are indeed isomorphic, or 0 if not.

### 裁判测试程序样例

```
#include <stdio.h>
#include <stdlib.h>

typedef char ElementType;

typedef struct TreeNode *Tree;
struct TreeNode {
    ElementType Element;
    Tree  Left;
    Tree  Right;
};

Tree BuildTree(); /* details omitted */

int Isomorphic( Tree T1, Tree T2 );

int main()
{
    Tree T1, T2;
    T1 = BuildTree();
    T2 = BuildTree();
    printf(“%d\n”, Isomorphic(T1, T2));
    return 0;
}

/* Your function will be put here */
```

### 输入样例

![](https://tva1.sinaimg.cn/large/0081Kckwly1gk9yuf3lsgj305404274h.jpg)

### 输出样例

```
1
```

### 输入样例2

![](https://tva1.sinaimg.cn/large/0081Kckwly1gk9yutq54mj305z042jrl.jpg)

### 输出样例2

```
0
```

### 代码

```
int Isomorphic( Tree T1, Tree T2 ){
    if(T1==NULL||T2==NULL){
        if(T1==NULL&&T2==NULL) return 1;
        else return 0;
    }
    else{
        if(T1->Element!=T2->Element) return 0;
        else return (Isomorphic(T1->Left,T2->Left)&&Isomorphic(T1->Right,T2->Right)) || (Isomorphic(T1->Left,T2->Right)&&Isomorphic(T1->Right,T2->Left));
    }
}
```

## ZigZagging on a Tree

### 题目概述

Suppose that all the keys in a binary tree are distinct positive integers. A unique binary tree can be determined by a given pair of postorder and inorder traversal sequences. And it is a simple standard routine to print the numbers in level-order. However, if you think the problem is too simple, then you are too naive. This time you are supposed to print the numbers in "zigzagging order" -- that is, starting from the root, print the numbers level-by-level, alternating between left to right and right to left. For example, for the following tree you must output: 1 11 5 8 17 12 20 15.

![](https://tva1.sinaimg.cn/large/0081Kckwly1gk9yweu8vaj306j05oq43.jpg)

### 函数接口定义

```
Input Specification:
Each input file contains one test case. For each case, the first line gives a positive integer N (≤30), the total number of nodes in the binary tree. The second line gives the inorder sequence and the third line gives the postorder sequence. All the numbers in a line are separated by a space.

Output Specification:
For each test case, print the zigzagging sequence of the tree in a line. All the numbers in a line must be separated by exactly one space, and there must be no extra space at the end of the line.
```

### 输入样例

```
8
12 11 20 17 1 15 8 5
12 20 17 11 15 8 5 1
```

### 输出样例

```
1 11 5 8 17 12 20 15
```

### 代码

```
#include <stdio.h>
#include <stdlib.h>
#include <queue>
using namespace std;

const int MAXN=40;

typedef int ElementType;

typedef struct TreeNode *Tree;
struct TreeNode {
    ElementType Element;
    Tree  Left;
    Tree  Right;
    int level;
};

int FindInPos(int value);

void BuildTree(Tree& t, int post, int in, int len, int level);

void PrintTree(Tree t);

int postorder[MAXN], inorder[MAXN];
int n;

int main()
{
    int num;
    Tree T=NULL;

    scanf("%d", &n);
    for(int i=0;i<n;i++)
        scanf("%d", &inorder[i]);
    for(int i=0;i<n;i++)
        scanf("%d", &postorder[i]);

    BuildTree(T, n-1, 0, n, 0);
    PrintTree(T);

    return 0;
}

int FindInPos(int value){
    for(int i=0;i<n;i++)
        if(inorder[i]==value) return i;

    return -1;
}

void BuildTree(Tree& t, int post, int in, int len, int level){
    if(len<=0) return;

    t=(Tree)malloc(sizeof(struct TreeNode));
    t->Element=postorder[post];
    t->level=level;
    t->Left=NULL;
    t->Right=NULL;

    int pos=FindInPos(postorder[post]);

    int leftlen=pos-in;
    int rightlen=len-leftlen-1;

    BuildTree(t->Left,post-1-rightlen,in,leftlen,level+1);
    BuildTree(t->Right,post-1,pos+1,rightlen,level+1);
}

void PrintTree(Tree t){
    queue<Tree> q;
    ElementType lay[MAXN]={0};
    ElementType output[MAXN]={0};
    if(t!=NULL) q.push(t);
    int ltotal=0;
    int total=0;
    int level=0;
    int i;

    while(!q.empty()){
        while(!q.empty()&&q.front()->level==level){
            Tree tmp=q.front();
            lay[ltotal++]=tmp->Element;
            q.pop();
            if(tmp->Left!=NULL) q.push(tmp->Left);
            if(tmp->Right!=NULL) q.push(tmp->Right);
        }

        if(level%2==1){
            for(i=0;i<ltotal;i++)
                output[total++]=lay[i];
        }
        else{
            for(i=ltotal-1;i>=0;i--)
                output[total++]=lay[i];
        }

        level++;
        ltotal=0;

    }

    for(i=0;i<total;i++){
        if(i!=total-1) printf("%d ",output[i]);
        else  printf("%d\n",output[i]);
    }
}
```

## Percolate Up and Down

### 题目概述

Write the routines to do a "percolate up" and a "percolate down" in a binary min-heap.

### 函数接口定义

```
void PercolateUp( int p, PriorityQueue H );
void PercolateDown( int p, PriorityQueue H );
```

where `int p` is the position of the element, and `PriorityQueue` is defined as the following:

```
typedef struct HeapStruct *PriorityQueue;
struct HeapStruct {
    ElementType  *Elements;
    int Capacity;
    int Size;
};
```

### 裁判测试程序样例

```
#include <stdio.h>
#include <stdlib.h>

typedef int ElementType;
#define MinData -1

typedef struct HeapStruct *PriorityQueue;
struct HeapStruct {
    ElementType  *Elements;
    int Capacity;
    int Size;
};

PriorityQueue Initialize( int MaxElements ); /* details omitted */

void PercolateUp( int p, PriorityQueue H );
void PercolateDown( int p, PriorityQueue H );

void Insert( ElementType X, PriorityQueue H ) 
{
    int p = ++H->Size;
    H->Elements[p] = X;
    PercolateUp( p, H );
}

ElementType DeleteMin( PriorityQueue H ) 
{ 
    ElementType MinElement; 
    MinElement = H->Elements[1];
    H->Elements[1] = H->Elements[H->Size--];
    PercolateDown( 1, H );
    return MinElement; 
}

int main()
{
    int n, i, op, X;
    PriorityQueue H;

    scanf("%d", &n);
    H = Initialize(n);
    for ( i=0; i<n; i++ ) {
        scanf("%d", &op);
        switch( op ) {
        case 1:
            scanf("%d", &X);
            Insert(X, H);
            break;
        case 0:
            printf("%d ", DeleteMin(H));
            break;
        }
    }
    printf("\nInside H:");
    for ( i=1; i<=H->Size; i++ )
        printf(" %d", H->Elements[i]);
    return 0;
}

/* Your function will be put here */
```

### 输入样例

```
9
1 10
1 5
1 2
0
1 9
1 1
1 4
0
0
```

### 输出样例

```
2 1 4 
Inside H: 5 10 9
```

### 代码

```
void PercolateUp( int p, PriorityQueue H ){
    int flag=1;

    while(1){
        if(p<=1)
            break;

        if(H->Elements[p/2]>=H->Elements[p]) {
            int tmp = H->Elements[p];
            H->Elements[p] = H->Elements[p/2];
            H->Elements[p/2] = tmp;
            p=p/2;
        }
        else
            break;
    }
}

void PercolateDown( int p, PriorityQueue H ){
    int child;

    while(1){
        child=p;
        
        if(p*2<=H->Size){
            if(H->Elements[p*2]<=H->Elements[p]) {
                child=p*2;
            }
            
        }
        else if(p*2<H->Size){
            if(H->Elements[p*2+1]<=H->Elements[p*2]&&H->Elements[p*2+1]<=H->Elements[p]){
                child=p*2+1;
            }
            else if(H->Elements[p*2+1]>H->Elements[p*2]&&H->Elements[p*2]<=H->Elements[p]){
                child=p*2;
            }
        }


        if(child!=p){
            int tmp=H->Elements[child];
            H->Elements[child]=H->Elements[p];
            H->Elements[p]=tmp;
            p=child;
        } else
            break;
    }
}
```

## Complete Binary Search Tree

### 题目概述

A Binary Search Tree (BST) is recursively defined as a binary tree which has the following properties:

```
The left subtree of a node contains only nodes with keys less than the node's key.
The right subtree of a node contains only nodes with keys greater than or equal to the node's key.
Both the left and right subtrees must also be binary search trees.
```

A Complete Binary Tree (CBT) is a tree that is completely filled, with the possible exception of the bottom level, which is filled from left to right.

Now given a sequence of distinct non-negative integer keys, a unique BST can be constructed if it is required that the tree must also be a CBT. You are supposed to output the level order traversal sequence of this BST.

### 函数接口定义

```
Input Specification:
Each input file contains one test case. For each case, the first line contains a positive integer N (≤1000). Then N distinct non-negative integer keys are given in the next line. All the numbers in a line are separated by a space and are no greater than 2000.

Output Specification:
For each test case, print in one line the level order traversal sequence of the corresponding complete binary search tree. All the numbers in a line must be separated by a space, and there must be no extra space at the end of the line.
```

### 输入样例

```
10
1 2 3 4 5 6 7 8 9 0
```

### 输出样例

```
6 3 8 1 5 7 9 0 2 4
```

### 代码

```
#include <stdio.h>
#include <stdlib.h>
#include <math.h>

typedef struct Node *Pnode;
struct Node{
    int element;
    Pnode lchild;
    Pnode rchild;
};

Pnode que[2001];
int lp=0;
int rp=0;
int flag=1;

void push(Pnode head){
    que[rp++]=head;
}

Pnode pop(){
    return que[lp++];
}

int empty(){
    return lp==rp;
}

int rcal(int n){
    if(n==1||n==0)
        return 0;

    int i;
    i=1;
    while(2*(pow(2,i)-1)<n-1){
        i++;
    }

    if(2*(pow(2,i)-1)==n-1)
        return pow(2,i)-1;
    else if(2*(n+1-pow(2,i))<=pow(2,i))
        return pow(2,i-1)-1;
    else
        return n-pow(2,i);
}

Pnode BuildTree(Pnode parent,int num[],int i,int j){
    if(i==j) return NULL;

    Pnode now=(Pnode)malloc(sizeof(struct Node));
    int rc=rcal(j-i);

    now->element=num[j-rc-1];
    now->lchild=BuildTree(now,num,i,j-rc-1);
    now->rchild=BuildTree(now,num,j-rc,j);
    return now;
}

void LevelPrint(){
    while(!empty()){
        Pnode head=pop();
        if(flag){
            printf("%d",head->element);
            flag=0;
        }
        else
            printf(" %d",head->element);
        if(head->lchild) push(head->lchild);
        if(head->rchild) push(head->rchild);
    }
}

int main(){
    int i,j,k,n;
    int tmp;
    int num[2001];

    scanf("%d",&n);
    for(i=0;i<n;i++){
        scanf("%d",&num[i]);
    }

    for(i=0;i<n-1;i++) {
        k = i;
        for (j = i + 1; j < n; j++) {
            if (num[j] < num[k])
                k = j;
        }
        if (k != i) {
            tmp = num[k];
            num[k] = num[i];
            num[i] = tmp;
        }
    }

    Pnode head=BuildTree(NULL,num,0,n);
    push(head);
    LevelPrint();
}
```

## File Transfer

### 题目概述

We have a network of computers and a list of bi-directional connections. Each of these connections allows a file transfer from one computer to another. Is it possible to send a file from any computer on the network to any other?

### 函数接口定义

Input Specification:

Each input file contains one test case. For each test case, the first line contains N (2≤N≤10^4), the total number of computers in a network. Each computer in the network is then represented by a positive integer between 1 and N. Then in the following lines, the input is given in the format:

```
I c1 c2
```

where I stands for inputting a connection between c1 and c2; or

```
C c1 c2
```

where C stands for checking if it is possible to transfer files between c1 and c2; or

```
S
```

where S stands for stopping this case.

Output Specification:

For each C case, print in one line the word "yes" or "no" if it is possible or impossible to transfer files between c1 and c2, respectively. At the end of each case, print in one line "The network is connected." if there is a path between any pair of computers; or "There are k components." where k is the number of connected components in this network.

### 输入样例

```
5
C 3 2
I 3 2
C 1 5
I 4 5
I 2 4
C 3 5
S
```

### 输出样例

```
no
no
yes
There are 2 components.
```

### 输入样例2

```
5
C 3 2
I 3 2
C 1 5
I 4 5
I 2 4
C 3 5
I 1 3
C 1 5
S
```

### 输出样例2

```
no
no
yes
yes
The network is connected.
```

### 代码

```
#include <stdio.h>

int Find(int S[],int X){
    if(S[X]<0)
        return X;
    else
        return Find(S,S[X]);
}

void SetUnion(int *S,int c1,int c2){
    int root1,root2;
    root1=Find(S,c1-1);
    root2=Find(S,c2-1);

    if(root1==root2&&root1>=0&&root2>=0)
        return;

    if(S[root2]<S[root1]){
        S[root2]+=S[root1];
        S[root1]=root2;
    }
    else{
        S[root1]+=S[root2];
        S[root2]=root1;
    }
}

int main(){
    char c;
    int i,n,c1,c2;
    int S[10005];
    int num=0;

    scanf("%d",&n);

    for(i=0;i<n;i++){
        S[i]=-1;
    }

    scanf("%c",&c);
    while(c!='S'){
        scanf("%d%d",&c1,&c2);

        if(c=='I'){
            SetUnion(S,c1,c2);
        }
        else if(c=='C'){
            if(Find(S,c1-1)==Find(S,c2-1))
                printf("yes\n");
            else
                printf("no\n");
        }

        scanf("%c",&c);
    }

    for(i=0;i<n;i++){
        if(S[i]<0)
            num++;
    }

    if(num>1){
        printf("There are %d components.\n",num);
    }
    else{
        printf("The network is connected.\n");
    }
}
```

## Is Topological Order

### 题目概述

Write a program to test if a give sequence `Seq` is a topological order of a given graph `Graph`.

### 函数接口定义

```
bool IsTopSeq( LGraph Graph, Vertex Seq[] );
```

where LGraph is defined as the following:

```
typedef struct AdjVNode *PtrToAdjVNode; 
struct AdjVNode{
    Vertex AdjV;
    PtrToAdjVNode Next;
};

typedef struct Vnode{
    PtrToAdjVNode FirstEdge;
} AdjList[MaxVertexNum];

typedef struct GNode *PtrToGNode;
struct GNode{  
    int Nv;
    int Ne;
    AdjList G;
};
typedef PtrToGNode LGraph;
The function IsTopSeq must return true if Seq does correspond to a topological order; otherwise return false.

Note: Although the vertices are numbered from 1 to MaxVertexNum, they are indexed from 0 in the LGraph structure.
```

Sample program of judge:

```
#include <stdio.h>
#include <stdlib.h>

typedef enum {false, true} bool;
#define MaxVertexNum 10  /* maximum number of vertices */
typedef int Vertex;      /* vertices are numbered from 1 to MaxVertexNum */

typedef struct AdjVNode *PtrToAdjVNode; 
struct AdjVNode{
    Vertex AdjV;
    PtrToAdjVNode Next;
};

typedef struct Vnode{
    PtrToAdjVNode FirstEdge;
} AdjList[MaxVertexNum];

typedef struct GNode *PtrToGNode;
struct GNode{  
    int Nv;
    int Ne;
    AdjList G;
};
typedef PtrToGNode LGraph;

LGraph ReadG(); /* details omitted */

bool IsTopSeq( LGraph Graph, Vertex Seq[] );

int main()
{
    int i, j, N;
    Vertex Seq[MaxVertexNum];
    LGraph G = ReadG();
    scanf("%d", &N);
    for (i=0; i<N; i++) {
        for (j=0; j<G->Nv; j++)
            scanf("%d", &Seq[j]);
        if ( IsTopSeq(G, Seq)==true ) printf("yes\n");
        else printf("no\n");
    }

    return 0;
}

/* Your function will be put here */
```

### 输入样例

![](https://tva1.sinaimg.cn/large/0081Kckwly1gkz32rwoo7j306z03sglq.jpg)

```
6 8
1 2
1 3
5 2
5 4
2 3
2 6
3 4
6 4
5
1 5 2 3 6 4
5 1 2 6 3 4
5 1 2 3 6 4
5 2 1 6 3 4
1 2 3 4 5 6
```

### 输出样例

```
yes
yes
yes
no
no
```

### 代码

```
bool IsTopSeq( LGraph Graph, Vertex Seq[] ){
    int deg[2000]={0};
    int que[2000]={0};
    int i;
    int end=-1;
    PtrToAdjVNode p;

    for(i=0;i<Graph->Nv;i++){
        p=Graph->G[i].FirstEdge;
        while(p){
            deg[p->AdjV]++;
            p=p->Next;
        }
    }

    for(i=0;i<Graph->Nv;i++){
        que[++end]=Seq[i]-1;
    }

    i=-1;

    while(i!=end){
        i++;
        if(deg[que[i]]!=0)
            return false;

        p=Graph->G[que[i]].FirstEdge;
        while(p){
            deg[p->AdjV]--;
            if(deg[p->AdjV]<0)
                return false;
            p=p->Next;
        }
    }

    return true;
}
```

## Hamiltonian Cycle

### 题目概述

The "Hamilton cycle problem" is to find a simple cycle that contains every vertex in a graph. Such a cycle is called a "Hamiltonian cycle".

In this problem, you are supposed to tell if a given cycle is a Hamiltonian cycle.

### 函数接口定义

```
Input Specification:
Each input file contains one test case. For each case, the first line contains 2 positive integers N (2<N≤200), the number of vertices, and M, the number of edges in an undirected graph. Then M lines follow, each describes an edge in the format Vertex1 Vertex2, where the vertices are numbered from 1 to N. The next line gives a positive integer K which is the number of queries, followed by K lines of queries, each in the format:

n V1 V2 ... Vn
​​
where n is the number of vertices in the list, and Vi's are the vertices on a path.

Output Specification:
For each query, print in a line YES if the path does form a Hamiltonian cycle, or NO if not.
```

### 输入样例

```
6 10
6 2
3 4
1 5
2 5
3 1
4 1
1 6
6 3
1 2
4 5
6
7 5 1 4 3 6 2 5
6 5 1 4 3 6 2
9 6 2 1 6 3 4 5 2 6
4 1 2 5 1
7 6 1 3 4 5 2 6
7 6 1 2 5 4 3 1
```

### 输出样例

```
YES
NO
NO
NO
YES
NO
```

### 代码

```
#include <stdio.h>
#include <stdlib.h>

#define MAXN 210


int main()
{
    int i, j, k, p, m, n;
    int num, count;
    int v1, v2;
    int v[MAXN];
    int G[MAXN][MAXN]={0};
    int flag, rflag;

    scanf("%d%d",&n,&m);

    for(i=0;i<m;i++){
        scanf("%d%d",&v1,&v2);
        G[v1][v2]=1;
        G[v2][v1]=1;
    }

    scanf("%d",&k);
    for(i=0;i<k;i++){
        count=0;
        flag=1;

        scanf("%d",&num);

        for(j=0;j<num;j++){
            rflag=0;
            scanf("%d",&v[j]);

            for(p=0;p<j;p++){
                if(v[p]==v[j]){
                    rflag=1;
                    break;
                }
            }

            if(!rflag)
                count++;
        }

        for(j=1;j<num;j++){
            if(G[v[j-1]][v[j]]==0){
                flag=0;
                break;
            }
        }


        if(flag&&count==n&&v[0]==v[num-1]&&num-1==n){
            printf("YES\n");
        }
        else{
            printf("NO\n");
        }


    }

    return 0;
}
```

## 

### 题目概述

### 函数接口定义

### 输入样例

```

```

### 输出样例

```

```

### 代码

```

```

























# 浙大软院机试

## Standard Form of Polynomial

### 题目概述

The standard form of a polynomial of degree n is P(x)=an​​ x​n​​ +a​n−1xn−1+⋯+a1x+a0. Given that an=1 and all the n roots { r1, ..., rn} of P(x) are integers. Then P(x) can be written as (x−r1)(x−r2)⋯(x−rn). Your job is to write P(x) in its standard form with all roots given.

### 函数接口定义

```
Input Specification:
Each input file contains one test case. For each case, the first line gives a positive n (≤10) which is the degree of P(x). Then n integer roots are given in the next line, separated by spaces.

Output Specification:
For each case, output the coefficients ai(i=n−1,⋯,0) in a line. All the numbers must be separated by one space, and there must be no extra space at the beginning or the end of the line. It is guaranteed that all the calculation will be within the range of int.
```

### 输入样例

```
3
-1 4 -1
```

### 输出样例

```
-2 -7 -4
```

### 代码

```
#include <iostream>
using namespace std;

const int MAXN=100001;
int list[MAXN];

int cal(int a,int b,int n){
    int ans;
    ans=0;
    if(n==0) return 1;

    for(int i=a;i<=b;i++){
        ans+=list[i]*cal(i+1,b,n-1);
    }

    return ans;

}

int main() {
    int i,n;
    cin>>n;

    int tmp;

    for(i=0;i<n;i++) {
        cin >> tmp;
        list[i]=-tmp;
    }

    for(i=1;i<n;i++)
        cout<<cal(0,n-1,i)<<" ";

    cout<<cal(0,n-1,n)<<endl;
}

```

## Distance of Triples

### 题目概述

The distance of triples (三元组) (a,b,c) is defined as D(a,b,c)=∣a−b∣+∣b−c∣+∣c−a∣. Now given three non-empty integer sets S1, S2 and S3, you are supposed to find the minimum distance of all the possible triples (a,b,c) where a∈S1, b∈S2 and c∈S3.

### 函数接口定义

```
Input Specification:
Each input file contains one test case. For each case, the first line gives three positive integers N1, N2 and N3(all no more than 10^4), which are the sizes of S1, S2 and S3, respectively. Then the members of the three sets are given in the following three lines, respectively. All the numbers are integers in the range [−10^4,10^4], and they are distinct within each set. The numbers in a line are separated by spaces.

Output Specification:
For each case, print in a line MinD(a, b, c) = d, where (a, b, c) are the triples that has the minimum distance, and d is the corresponding distance. If the solution is not unique, output the largest triples.
```

### 输入样例

```
4 4 6
0 9 -1 11
10 -25 11 -10
9 2 41 17 12 30
```

### 输出样例

```
MinD(11, 11, 12) = 2
```

Hint:
Notice that there are two solutions. The other one is MinD(9, 10, 9) = 2. Since (11, 11, 12) is larger, this one must be printed out.

### 代码（未验证）

```
#include <iostream>
#include <cmath>
using namespace std;
const int MAXN=100001;

int main(){
    int i,j,k;
    int n1,n2,n3;

    int s1[MAXN],s2[MAXN],s3[MAXN];

    cin>>n1>>n2>>n3;

    for(i=0;i<n1;i++){
        cin>>s1[i];
    }

    for(i=0;i<n2;i++){
        cin>>s2[i];
    }

    for(i=0;i<n3;i++){
        cin>>s3[i];
    }

    int ansi=0;
    int ansj=0;
    int ansk=0;
    int ans=abs(s1[0]-s2[0])+abs(s1[0]-s3[0])+abs(s2[0]-s3[0]);

    for(i=0;i<n1;i++){
        for(j=0;j<n2;j++){
            for(k=0;k<n3;k++){
                if(abs(s1[i]-s2[j])+abs(s1[i]-s3[k])+abs(s2[j]-s3[k])<=ans){
                    ansi=i;
                    ansj=j;
                    ansk=k;
                    ans=abs(s1[i]-s2[j])+abs(s1[i]-s3[k])+abs(s2[j]-s3[k]);
                }
            }
        }
    }

    cout<<"MinD("<<s1[ansi]<<", "<<s2[ansj]<<", "<<s3[ansk]<<") = "<<ans<<endl;
    return 0;
}
```

## Partial School Ranking

### 题目概述

In a Group Programming Contest, each university is supposed to send n students who must compete independently. The universities are ranked according to the total score of their students. Your job is to generate the ranklist.

It sounds like a simple problem, but what makes it complicated is that we have lost all the teams' information! What we have is a list of only some of the students' scores, and some photos taken from the contest sites which shows several students with the same university badge. You just have to recover the ranklist as much as you can.

### 函数接口定义

```
Input Specification:
Each input file contains one test case. For each case, the first line gives a positive integer N (≤1000). Then N lines follow, each gives the infomation of a student in the format:

ID k teammate1⋯teammatek Score

where ID is a unique 4-digit identification number for a student; k (0≤k≤5) is the number of teammates of this student in a photo; teammate
​i's are the ID's of his/her teammate; and Score is this stdent's score which is in the range [0, 400].

It is guaranteed that each ID with a Score is given only once.

Output Specification:
For each case, first print in a line the number of universities (all the students that are related directly or indirectly as teammates are considered in the same university). Then output the partial school ranking in the format:

ID S Score total
​​
where ID is the smallest student ID in the university; S is the total number of its students; and Score
​total
​​  is the total score that can be recovered for that university. The universities must be given in descending order of their Score
​total
​​ , or in ascending order of S if there is a tie, or in ascending order of ID if there is still a tie.
```

### 输入样例

```
11
7456 3 7457 7458 7459 157
6666 3 5551 5552 7777 100
1234 3 5678 9012 0002 80
8888 0 340
2468 3 0001 0004 2222 110
7777 1 6666 57
3721 1 2333 30
9012 3 1236 1235 1234 10
1235 2 5678 9012 50
2222 4 1236 2468 6661 6662 16
2333 4 3721 6661 6662 6663 44
```

### 输出样例

```
4
8888 1 340
0001 15 340
5551 4 157
7456 4 157
```

### 代码（未验证）

```
#include <iostream>
#include <map>
#include <vector>
using namespace std;

const int MAXN=10001;

map<int,int> lscore;
int G[MAXN][MAXN]={0};

int total=0;

vector<int> listn[MAXN];
int flag[MAXN]={0};

void makelist(){
    for(int i=1;i<MAXN;i++){
        for(int j=i;j<MAXN;j++){
            if(G[i][j]==1){
                listn[i].push_back(j);
                listn[j].push_back(i);
            }
        }
    }
}

void DFS(int id){
    if(listn[id].empty()||flag[id]==1) return;

    for(int i=0;i<listn[id].size();i++){
        total++;
        flag[id]=1;
        if(flag[i]==0) DFS(listn[id][i]);
    }
}

int main(){
    int i,j,n;
    int id,maten,score;
    int tmpid;

    cin>>n;
    for(i=0;i<n;i++){
        cin>>id;
        tmpid=id;
        cin>>maten;
        for(j=0;j<maten;j++){
            cin>>id;
            G[tmpid][id]=1;
            G[id][tmpid]=1;
        }
        cin>>score;
        lscore[tmpid]=score;
    }

    makelist();

    while(i<MAXN){
        if(flag[i]==1){
            i++;
            continue;
        }
        DFS(i);
        if(total!=0) {
            cout<<total+1<<endl;
            total=0;
        }

        i++;
    }
}
```

## Coupons

### 题目概述

There are N items on your shopping list, and you have N kinds of coupon that can save you some money on buying any of these items. At the mean time, you have D dollars in your pocket. How can you buy as many items as you can? Here you may use any coupon many times and buy any item many times, but only one coupon may be used to get a reduction of payment for one item once -- that is, for example, you may use coupon A1 to buy item B1, and then use coupon A2 to buy item B1. At the mean time, coupon A1 can be used to buy item B2. But you may not use coupon A1 to buy item B1 AGAIN.

For example, suppose there are 4 items with prices of $10, $12, $15 and $20; and 4 kinds of coupons with values of $6, $7, $8 and $9. If you have $30 in your pocket, the best way to buy is the following:

buy $10 item for 4 times, with each coupon once, and hence pay $10x4 - $6 - $7 - $8 - $9 = $10 in total;
buy $12 item for 3 times, with coupons of $7, $8 and $9, and hence pay $12x3 - $7 - $8 - $9 = $12 in total;
buy $15 item with $9 coupon, and hence pay $6;
Now you have $2 left, not enough to buy any more items. Therefore the maximum number of items you can buy is 8.

### 函数接口定义

```
Input Specification:
Each input file contains one test case. For each case, the first line gives two positive integers: N (≤10^5), the number of items (and the coupons), and D (≤10^6), the amount of money you have.

Then N positive prices are given in the second line, and N positive coupon values are given in the third line. It is guaranteed that the highest value of coupons is less than the lowest price of items, and all the numbers are bounded above by 10^9. The numbers in a line are separated by spaces.

Output Specification:
Print in a line the maximum number of items you can buy, and the maximum amount of money left, separated by 1 space.
```

### 输入样例

```
4 30
12 20 15 10
9 6 8 7
```

### 输出样例

```
8 2
```

### 代码（未验证）

```
#include <iostream>
#include <algorithm>
using namespace std;

bool cmp(int a,int b){
    return a>b;
};

const int MAXN=100001;

int main(){
    int n,d;
    int i,j;
    int price;
    int total=0;
    int flag=0;

    int items[MAXN],coupons[MAXN];

    cin>>n>>d;
    for(i=0;i<n;i++){
        cin>>items[i];
    }

    sort(items,items+n);

    for(i=0;i<n;i++){
        cin>>coupons[i];
    }

    sort(coupons,coupons+n,cmp);

    while(flag==0){
        for(i=0;i<n;i++) {
            flag = 0;
            for (j = 0; j < n; j++) {
                price = items[i] - coupons[j];
                if (d >= price) {
                    d -= price;
                    total++;
                } else {
                    flag = 1;
                    break;
                }
            }

            if (flag == 1) break;
        }
    }

    cout<<total<<" "<<d<<endl;

}
```

# MSC2018趣味C挑战赛

## 救救孩子

### 题目概述

sbconscious最近沉迷于中美合拍的西游记，导致他在计算七个整数的平均数保留两位小数（四舍五入）的时候将最后一位算错了，请你帮他纠正过来。

对于30％的数据，n≤10^9。

对于100%的数据，n≤10^1000，输入数据保证有解。

### 函数接口定义

```
输入格式:
一行一个保留两位小数的正浮点数n（n≤10^1000）。

输出格式:
一行一个保留两位小数的正浮点数。
```

### 输入样例

```
0.13
```

### 输出样例

```
0.14
```

### 代码

答案和整数部分无关，只需要考虑小数部分。总共只有七种情况，判断一下它属于哪一个即可。

参考数据：

```
0/7 = 0.000000， 1/7 = 0.142857， 2/7 = 0.285714， 3/7 = 0.428571
4/7 = 0.571428，5/7 = 0.714285， 6/7 = 0.857142
```

这些数小数点后第一位都是不一样，根据数据修改最后一位即可，注意四舍五入。

## 庭庭的布偶

### 题目概述

婷婷的弟弟——庭庭，他拥有一些布偶。庭庭给每一只布偶都起了英文名字，这些名字是只由大写英文字母组成的长度为L的字符串（1<=L<=100）。庭庭想要通过自定义的字典序来给这些布偶排序。

但是偏心的庭庭最喜爱的是第K个布偶，他也许想把该布偶安排在任意一个位置。庭庭非常想要知道，对于布偶序列中每一个位置，是否存在一种自定义字典序，使得该规则下他能够将第K个布偶排在队列中的该位置。

最后，由于严格的婷婷不允许弟弟沉迷于太多的布偶游戏而玩物丧志，她只给庭庭留下了三只布偶，并请求你来解决此问题。

对于字典序，庭庭向你提供了以下信息：

在数学中，字典或词典顺序是基于字母顺序排列的单词按字母顺序排列的方法。这种泛化主要在于定义有序完全有序集合（通常称为字母表）的元素的序列（通常称为计算机科学中的单词）的总顺序。不同单词的先后关系是从左到右逐个比较对应的字母的先后来决定的。

特别地，当两个字符串完全相同时，它们的先后顺序是任意的。

### 函数接口定义

```
输入格式:
第一行输入三个长度相同只由大写英文字母组成的字符串，字符串之间用一个空格隔开。

第二行输入一个正整数K，表示庭庭偏爱的布偶编号。

输出格式:
请你输出三行答案。第i行若为“YES”，则说明庭庭偏爱的布偶能够在某种自定义字典序下排在三只玩偶中的第i位；否则便输出“NO”。（输出不含引号）
```

### 输入样例

```
BCB CAB CCC 
1
```

### 输出样例

```
YES
NO
YES
```

### 输入样例2

```
HTTNB HTTBB HTTNN
1
```

### 输出样例2

```
NO
YES
NO
```

### 代码

题目大意：
  
给定三个长度相等的字符串。指定其中一个串，分别询问它是否能够排在第一/二/三的位置 (可自定义字典序)。

解题思路：

考虑分类讨论。

首先找到三个字符串第一个出现分歧的位置，此时会有两种情况:1、三个字母各不相同 2、 其中一个字母不同于另外两个。

对于第一种情况，毋庸置疑输出三个“YES”，因为我们可以任意地调整三者的优先性。

对于第二种情况，不妨假设这三个字母分别为‘A’、‘B’、‘B’。如果第K个布偶该位置正好是A， 显然它没有办法排在中间，但一定可以排在两边，此时输出“YES”、“NO”、“YES”;如果第K个 布偶该位置是B，我们便需要对这两个B打头的后缀子串继续遍历，找到它们第一次分歧的位 置，此时，若第K个串出现一个‘B’而另一个串出现了‘A’，则说明庭庭偏爱的布偶无法排在中 间，若第K个串出现一个‘A’而另一个串恰好出现了‘B’，则说明庭庭偏爱的布偶无法排在两边， 剩下的情况均不会出现任何矛盾，讨论结束。

特别地，如果三个字符串完全相同，则说明任何排列方案都合法，直接输出三个“YES”。

```
#include<stdio.h>
#include<string.h>
int K,len;
char str[5][110],ss[110];

int main(){
    scanf("%s%s%s",str[1],str[2],str[3]);
    len=strlen(str[1]);
    scanf("%d",&K);

    for(int i=0;i<len;i++){
        if(str[1][i]==str[2][i] && str[2][i]==str[3][i])
            continue;
        if(str[1][i]!=str[2][i] && str[1][i]!=str[3][i] && str[2][i]!=str[3][i]){
            printf("YES\n");
            printf("YES\n");
            printf("YES\n");
            return 0;
        }
        int tot=0;
        for(int ii=1;ii<=3;ii++)
        if(str[ii][i]!=str[K][i])
            tot++;
        if(tot==2){
            printf("YES\n");
            printf("NO\n");
            printf("YES\n");
            return 0;
        }

        char ch1=str[K][i],ch2;
        int flag1=1,flag2=1,a;

        for(a=1;a<=3;a++)
            if(str[a][i]!=ch1)
                ch2=str[a][i];

        for(a=1;a<=3;a++)
            if(a!=K && str[a][i]==str[K][i]){
                for(int ii=i+1;ii<len;ii++){
                    if(str[a][ii]==str[K][ii])
                        continue;
                    if(str[a][ii]==ch1 && str[K][ii]==ch2)
                        flag1=0;
                    if(str[a][ii]==ch2 && str[K][ii]==ch1)
                        flag2=0;
                    break;
                }
            break;
        }

        if(flag1==1)
            printf("YES\n");
        else
            printf("NO\n");

        if(flag2==1)
            printf("YES\n");
        else
            printf("NO\n");

        if(flag1==1)
            printf("YES\n");
        else
            printf("NO\n");

        return 0;
    }

    printf("YES\n");
    printf("YES\n");
    printf("YES\n");
    return 0;
}
```

## FA♂

### 原题

**AAAFFAAFAAFAAFAAAAFAFAAAFAFAAAAFFFFFAAFFAFAAAAFFFAAFFAF**

AAFAFAFAAAFAAAFFAAFAFAAFFAFAFFFFAAA, FAFFAAAFAA AAAFFAAFAAAFFAFAFFFAFAAFFAAFAA AAAAAAFAFFAFFFFAAFFFAAAAAAAAAFAAFAAFAAFF AAAAA FAAFFAFFFA FFAAF FAFFAAFAAAFAAFFAAFFF FFAAFAAFAAFAAAFAFFFA FAAFFAFFFA FAAFFFAFFAAAFAAAFFAFFAAFFFFAAA-AAFAFAFAAAFAFAFAAFAA FAAAFAAFAAFAAFAAFFFFAAFAAAAAFAFAAFFAFAAAFAFAFAAFAAAFAFFFFAAA. FAAFFAAFFFAAFAAAFFAF AAFAAAAAAAAAAFAAAFFF AAAFAAFFFAAAAFFAAFAA AAAFAAAAAAAFFAF AAAAFAAFAA FAAAFAAFAAAFFFFFAAAFAAFAAFAAFAAAFAAAFFAFFAAFFAAFAAAAAFF AFAAAAFFAF AAAAFAFAAAAFFAFAAAAAFAAAFFFAAA AAFAFAFFFAFAAAFAFFAA FAFFAAFAAAFAAFFAAFFF AAFAFAFAAAFAFAFAAFAA AAAAFAFAAAFAAFFFAAFA. AAFAFAFAAAAFFAFAAAAAAFAFFAFAFFFFAAA FAFFAAAFAA FAAFFFAAAFAAAAAAFFAFFAAFAAAFAFAFFFAFAAAFAFFAA AFFFAAFFAFAAFAA FAAFFAFFFA AAFAF AAAAAAFFAFAAAFF FFAAFAAFAAFAAAFAFFFA FAAFFAFFFA AAAAA, AAFFAAAFAAFAAFF AAAAA AAFAFAAAAA FAAFAFAAFFFAAAFAFAAAAFFAFAAFFA. AAFFAAFAAAFAFAFAAFAAAFFAF AAAAA FAAFAFAAFFFAAAFAFAAAAFFAFAAFFA FAFFAAFAAAFAAFFAAFFF AFFFAAFFAFAFAFFFFAAA FAFAAAFFFFAFFFFAAFAAFAAAFAAAFAAAAAAFAAFAAAFAA AFAFFAAFAAFAAFFFAAFFAAFAAFAAAFFAAFA AAAAAAFFAFAAAFF FAAFAAFFFFAAAAAAAAFAAAFAAFAAFA, FAAFFFAAAFAAAAAAFFAFFAAFAAAFAFAFFFAFAAAFAFFAA AFAAAFAAFF FAAFFAFFFA AAAAA AAFAFAAAAA FAAFAFAAFFFAAAFAFAAAAFFAFAAFFA.

**AFAAAAFFAFAFFFFFAFAAFAAFF**

AFFFAAFFAFAAFAA AFAFFAFAAAAFFAFAAFAA AAAFAAFFFAAFFAFFAAFFAAAAAAFAAAAFFAFFAAFA AAAAA FAAFAFAAFFFAAAFAFAAAAFFAFAAFFA FAFFAAFAAAFAAFFAAFFF AFFFAAFFAFAFAFFFFAAA FAFAAAFFFFAFFFFAAFAAFAAAFAAAFAAAAAAFAAFAAAFAA AFAFFAAFAAFAAFFFAAFFAAFAAFAAAFFAAFA AAAAAAFFAFAAAFF FAAFAAFFFFAAAAAAAAFAAAFAAFAAFA.

FAAFFAAFFFAAFAA AFAFFAAFAAAFFAFAAFFAFAAFFAAFFF AFAAAFAAFA AFFAFAFFFA AFAFFAFFFAAFFAFAAFFAAAFAAFAAAF FAAFFAAFFFAAAAAAFFAF AFFFAAFFAFAAFAA AAFFFFAFAAAFFAFAAAFFFAAAFAAFAAAAAFF.

**AFFFAFAFAAFAAFFAFFFFFAFAAFAAFF**

AFFFAAFFAFAAFAA AFAFFAFAAAAFFAFAAFAA AAAFAAFFFAAFFAFFAAFFAAAAAAFAAAAFFAFFAAFA FAAFFAAFFFAAFAA AAFFAAAFAAAFFAFAAFAAFAAAFAAAAAFAAFFAAFAAAAAFF AAFAFAAAAA FAAFAFAAFFFAAAFAFAAAAFFAFAAFFA.

**FAAFAAAAAAAFFAAAFFFFAFAFFAAFAA AFAAAAFFAFAFFFFFAFAAFAAFF**

```
A A
```

**FAAFAAAAAAAFFAAAFFFFAFAFFAAFAA AFFFAFAFAAFAAFFAFFFFFAFAAFAAFF**

```
AAAAA AAAAA
```

### 解密方法

可以观察到后面两个副标题中的第一个单词都是FAAFAAAAAAAFFAAAFFFFAFAFFAAFAA， 鉴于这个标题对应的是输入输出样例，可以猜到这两个标题的含义是Sample Input和Sample Output

还可以关注到每两个空格之间的单词长度都是5的倍数，以五位为一个字母，每位可为A或F，相对于二进制的0和1，五位可表示2^5=32个字母。A=0，F=1，转换为序号即可。

```
FAAFA/AAAAA/AFFAA/AFFFF/AFAFF/AAFAA=Sample
```

### 题目概述

Firstly, we denote alphabet A to Z with zero to twenty-five respectively. Then each code can be represented in binary form with five bits. Finally we transform one to F and zero to A, get a FA string. Given a string with only uppercase letters and spaces, transform it to a FA string.

### 函数接口定义

```
Input
One line contains a string with only uppercase letters and spaces. The length is no longer than one hundred.
Output
One line contains the generated FA string.
```

### 输入样例

```
A A
```

### 输出样例

```
AAAAA AAAAA
```

### 代码

题目本身没什么问题，就是写个简单的 printf，照着题目给的例子写就是了，不过有一个 坑就是 C 语言参数传递时的自动类型提升。

char 会被 cast 成 int 再传过去，所以在拿的时候也得拿一个 int 。 这个信息可以在 pintia 给你的编译输出里看到的，所以不要随便忽略警告。

```
int printNum(int n) {
    return printf("%d", n);
}

int printFloat(double f) {
    return printf("%lf", f);
}

int printStr(char* str) { 
    return printf("%s", str);
}

int simple_printf(const char* fmt, ...) { 
    va_list args;
    va_start(args, fmt);
    int counter = 0;
    while (*fmt != '\0') {
        if (*fmt != '$') {
            putchar(*fmt);
            fmt++;
            counter++;
            continue;
        }
        fmt++;
        if (*fmt == 'd') { // integer
            int int_val = va_arg(args, int);
            counter += printNum(int_val);
            fmt++;
        } else if (*fmt == 'f') { // double
            double float_val = va_arg(args, double);
            counter += printFloat(float_val);
            fmt++;
        } else if (*fmt == 's') { // char*
            char* str = va_arg(args, char*);
            counter += printStr(str);
            fmt++;
        } else if (*fmt == 'c') { // char
            char cha_val = va_arg(args, int);
            counter += 1;
            putchar(cha_val);
            fmt++;
        } else if(*fmt == '$') {
            fmt++;
            putchar('$');
            counter += 1;
        }
    }
    va_end(args);
    return counter;
}
```

## Macro Overload

### 题目概述

宏是C语言中极为强大，也极具魅力的一个功能。宏为极其贴近机器语言的C提供了一定程度上的元编程能力。程序员们能够利用宏来减少代码中的重复，或是解决一些条件编译之类的问题。

宏、或者说C预处理器，实际上干的事情就是进行字符串拼接和处理，其中比较简单的就是普通的字符串替换：

```
#define abc efg
#define one 1
#define INT int
#define DENGYU
INT abc DENGYU one; // -> int efg = 1;
```

为了防止替换的过程无法结束，通过这种方式定义的宏在它的(和它的递归)展开内容中是不会再对其本身进行替换

```
#define abc abc efg
#define efg abc hij
#define hij efg abc
    abc // (所有东西都可以被展开)
->  abc efg // (abc 不会再被展开)
->  abc abc hij // (abc efg 不会再被展开)
->  abc abc efg abc
```

另一种则是宏函数，规则稍微复杂一些。宏函数首先会对它的参数进行宏展开，如果参数中存在`#`或者`##`(具体作用后面会提及)， 那么不再对这个参数进行递归展开，然后再按照宏的定义将参数进行拼接，得到这一次展开的结果；之后对结果进行下一次展开。这次展开和之前说的展开规则类似，不再会对生成的结果中相同的宏函数进行展开。

```
#define foo(a, b) a b
foo(1+, 2) // -> 1 + 2
#define foo2(a, b) foo(a) foo(b)
foo2(1+, 2) // -> foo(1+) foo(2) -> 1+ 1+ 2 2
```

C 预处理器提供了一些方法来生成特殊的字符串，例如`#`和`##`.

`##`能将它两边的内容直接连接起来，去除中间的空格，可以用来拼接新的宏或者标识符等

```
#define CONCAT(X, Y) X ## Y
int a = CONCAT(x, 1); // -> int a = x1;
#define ADD_PREFIX_AND_SUFFIX(X) pre ## X ## suf
int b = ADD_PREFIX_AND_SUFFIX(X); // int a = preXsuf;

#define IF(COND, PASS, FAIL) IF_ ## COND (PASS, FAIL)
#define IF_0(PASS, FAIL) FAIL
#define IF_1(PASS, FAIL) PASS

IF(0, A, B) // -> B
IF(1, A, B) // -> A
```

`#`能作用于它右边的宏参数，把它转化为一个 C 语言字符串字面量。而 C 语言也提供了“自动拼接相邻两个字符串字面量”的语法糖，使得我们可以在 编译期完成拼接字符串字面量的操作

```
#define TO_C_STR(X) #X

#define ADD_C_STR_SUFFIX(X, Y) X TO_C_STR(Y)

const char* c_str = ADD_C_STR_SUFFIX("abc", efg); // -> const char* c_str = "abc" "efg"; === const char* c_str = "abcefg";
```

宏也提供了可变参数的宏函数。简单的说，配合`__VA_ARGS__`它可以将传入的参数按原样替换到内容里。 相比与 C 语言的可变参数函数，宏函数不需要"至少有一个固定参数"。 在宏函数的替换内容中，可以使用`__VA_ARGS__`来代表它获得的可变参数. 可以使用`, ##__VA_ARGS__`来实现对 0 个可变参数的适配。可变参数`...`只能在参数列表中出现一次。 下面是一些例子：

```
#define PRINT(...) printf(__VA_ARGS__)
#define PRINTF(FMT, ...) printf(FMT, ##__VA_ARGS__)

PRINT("123"); // -> printf("123");
PRINT("%d", 1); // -> printf("%d", 1);
PRINTF("123"); // -> printf("123");
PRINTF("%d", 1); // -> printf("%d", 1);
```

实现一个可以接受可变参数的宏函数`MAX`, 它返回其中的最大值。

保证传入的参数为合法的 C 表达式，且类型为`int`，个数在 1 ~ 5 之间，中间用逗号分隔

保证传入的表达式无副作用

提示：

具体的规则最好自己实验一下， 使用`gcc -E test.c`可以打印出`test.c`经过预处理后的代码

宏函数的参数使用时在不影响语义的情况下尽量在两边加上`()`，一个常见的错误例子就是：

```
#define MUL(A, B) A * B
MUL(1 + 2, 3 + 4) // -> 1 + 2 * 3 + 4 
```

在预处理器进行处理的过程中, 可以产生一些临时参数，但它们很快就在下一次替换过程消失，而不会导致生成非法的表达式。

你可能需要一个形式类似于`#define FOO(_USELESS_PARAMETER, ...) BLAHBLAH`的宏

### 函数接口定义

```
#define MAX(...)
```

### 裁判测试程序样例

```
#include <stdio.h>

// your code here

int data[10];

int main() {
  int n;
  scanf("%d", &n);
  int x, y, z, w, u;
  switch(n) {
    case 1: scanf("%d", &x); printf("%d\n", MAX(x)); break;
    case 2: scanf("%d %d", &x, &y); printf("%d\n", MAX(x, y)); break;
    case 3: scanf("%d %d %d", &x, &y, &z); printf("%d\n", MAX(x, y, z)); break;
    case 4: scanf("%d %d %d %d", &x, &y, &z, &w); printf("%d\n", MAX(x, y, z, w)); break;
    case 5: scanf("%d %d %d %d %d", &x, &y, &z, &w, &u); printf("%d\n", MAX(x, y, z, w, u)); break;
    default: break;
  }
  return 0;
}
```

### 输入样例

```
4 1 3 2 1
```

### 输出样例

```
3
```

### 输入样例2

```
1 0
```

### 输出样例2

```
0
```

### 代码

这题做出来需要一些技巧。

1. 如何拿部分分? 由于函数题只能设置一种检查程序，所以只有编译通过才能获得部分 分。
很简单，直接把多于一个的参数都扔了就能过参数个数为 1 的那个点了，于是 20 分到手

```
#define MAX(X, ...) X
```

正解怎么写?

需要一丁点技巧，不过也没什么好说的，看了标程就懂了。大概就是利用参数个数不同使固 定位置上获取的参数不同从而实现基于参数个数的宏函数重载

```
#define MAX1(A) (A)
#define MAX2(A,B) (A)>(B)?(A):(B)
#define MAX3(A,B,C) MAX2(MAX2(A,B),(C))
#define MAX4(A,B,C,D) MAX2(MAX3(A,B,C),(D))
#define MAX5(A,B,C,D,E) MAX2(MAX4(A,B,C,D),(E))
#define GETMAX(A,B,C,D,E,MAXNAME,...) MAXNAME
#define MAX(...) GETMAX(__VA_ARGS__,MAX5,MAX4,MAX3,MAX2,MAX1)(__VA_ARGS__)
```

## Garbage Collection

### 题目概述

垃圾回收是大多数常见的语言中提供的内存管理模式。 C 语言在语言层面上并没有提供原生的垃圾回收支持，当然这是为了给程序员提供更加底层、更加可控的内存管理手段；但只要你使用过`malloc/free`这种手动获取/释放堆内存的方式，就不可避免的会遇到内存泄漏之类的问题，这时候你或许就会开始怀念写其它高级语言时可以随意使用的`new`。这道题需要你实现一个简单的垃圾回收机制(大概)。不过在此之前对此不了解也没有关系，下面我们会给出你完成这道题目所需要的所有信息。

垃圾回收可以抽象为下面这个问题：

程序变量和堆分配的记录构成了一个有向图，图中的结点为程序变量和堆上分配的内存；每一个程序变量是图中的一个根（gc root），堆上分配的内存和程序变量可能保存着指向另一块堆内存的指针；如果存在某种路径，使得能从一个根出发，沿着指针找到一块内存，那么称它为可到达的 (reachable)。

一个简单的例子：

```
void test() {
    int* ptr = (int*)malloc(sizeof(int)); // 1
    ptr = (int*)malloc(sizeof(int)); // 2
}
```

`ptr`最开始指向的内存是第一次调用`malloc`获得的，然而这个指针立即就被另一次赋值行为覆盖了，所以我们称第一块内存是不可到达的；而第二次分配时获取的指针仍然保存在`ptr`内部，所以它是可到达的。当调用的`test`函数推出之后，`ptr`中的值已经无法访问，也就是我们失去了这个根，所以此时分配的两块内存都变为不可到达的了。

当然需要提及的一点是，在堆上分配的每一块内存都需要记录自己的大小；这里假设我们能判断分配的内存中有哪些域是指针。

所以垃圾回收实际上就是找到有哪些在堆上分配的内存变为不可达了，并将其释放。常见的垃圾回收算法包括标记-清除、引用计数、复制、分代、增量式收集等，它们的区别也主要在于探测和释放的手段。

我们这里简要介绍一下其中的几种方法：

标记-清除算法

标记-清除算法通过深度优先搜索找到所有可达内存，并作上标记；对于没有标记的内存都一定是垃圾，应当回收。随后从头开始遍历堆，并将找到的未标记结点加入到空闲表中，等待下一次分配时使用。

标记-清除算法的伪代码如下：

```
function mark(x) {
    如果 x 为指向堆的指针 且 x 还未被标记 {
        标记 x
        对于 x 的每一个域, mark(x)
    }
}

function sweep() {
    p <- 堆中的第一个地址
    while p < 堆的最后一个地址 {
        如果 p 已经被标记 {
            去掉 p 的标记
        } 否则 {
            将 p 的第一个域指向当前的空闲表
            使空闲表等于 p
            将 p 移动到当前分配的内存块的末尾
        }
    }
}

function gc() {
    对于所有的根 x : mark(x)
    sweep()
}
```

复制式收集

标记-清除算法会带来很多内存碎片，这对于程序本身、内存分配和以后的垃圾回收性能会有很大影响，所以我们可以在寻找可达结点时将它们复制到新的内存区域中，而这种方法保证了得到的分配是连续的。

我们需要一块额外的一样大的堆内存。需要 gc 的内存我们成为 from-space ，另一块空闲内存成为 to-space 。我们采取类似标记-清扫中的标记的方法来遍历 from-space 中分配的内存。如果有可达结点所指向的内存在 from-space，我们则在 to-space 中分配一块同样大小的空间，并将其内容拷贝进去；重复此操作直到所有可达结点已经被探测完，然后将 to-space 作为下一次垃圾回收的 from-space。

具体的操作可以用下面的伪代码说明

```
function forward(p){
    如果 p 指向 from-space {
        如果 p 的第一个域指向 to-space {
            返回 p 的第一个域
        } 否则 {
            在 to-space 上分配一块和 p 相同大小的内存
            将 p 的每一个域复制进这块内存
            使 p 的第一个域指向这块内存
            返回 p 的第一个域
        }
    } 否则 {
        返回 p
    }
}

function gc() {
    清空 to-space
    使 scan 为 to-space 的开始
    对于每一个根 x : 使 x 等于 forward(x)
    while scan 不等于 to-space 分配的最后一块空间的末尾 {
        对于 scan 指向的内存的所有的域 f : 使 f 等于 forward(f)
        将 scan 移动到下一块 to-space 中分配的内存
    }
    交换 to-space 和 from-space
}
```

引用计数

每个对象保存一个存有持有自己的对象的个数的计数器，也即引用计数器。在其它对象获取其时引用计数器加一，反之则减一，当计数器为 0 时则认为它已经无法被访问到并释放资源。引用计数的实现非常简单但存在无法直接处理循环引用的问题。一种解决方案是引入弱引用，打破对象之间形成的环状引用关系。

那么这道题到底要做什么呢？我们需要你实现一个简单的垃圾回收算法，无论是参考上面所写的，还是自己实现的。这里的垃圾回收基于一个假设：分配的所有内存和根中都只保存指针(操作系统会有单独的区域保存它们的大小，但这部分并不参与统计)。它需要支持下面这些操作：

```
创建新的根
分配一块大小固定的内存
使根指向一块内存的头部
使一块内存中的某个域指向一块内存的头部
删除一个已有的根
触发一次垃圾回收
```

前 5 种操作都不需要输出，而在触发垃圾回收的时候，需要打印出当前可达内存总量为多大，有多少块可达内存。

具体的输入输出格式如下

输入第一行为一个整数 n ，第 2 行至第 n + 1 行 为如下形式的操作之一

```
new gcroot <id> 创建一个编号为 <id> 的根
new object <id> <size> 在堆中分配一块大小为 <size> （加上长度域一共 <size> +  1 ），编号为 <id> 的空间
link <object> <offset> <object> 使第一块 <object> 中的第 <offset> 个指针指向第二个 <object>
assign <gcroot> <object> 使编号为 <gcroot> 的根指向编号为 <object> 的内存
remove <id> 移除编号为 <id> 的根
```

在所有输入结束之后触发一次垃圾回收，此时输出两个由空格分割的整数，第一个数是当前的可达内存总量（只计算指针所占内存，不考虑长度域），第二个是可达的内存块个数，输出完两个数后换行.

数据范围：

n <= 1000 , 0 <= offset < size

所有的 id ( 包括根和内存 ) 都大于 0 小于 10000, 且互不相等;

0 < size <= 10000，分配的所有内存加起来不超过 100000

保证数据合法，且所有 id 不会在它们的 new 之前出现，被 remove 的根不会再次出现

补充说明：

assign 和 link 都是不可逆的操作

参考：

现代编译原理 C 语言描述（修订版）(虎书)

### 输入样例

```
7
new gcroot 1
new object 2 100
assign 1 2
new object 3 1
link 3 0 2
new object 4 3
assign 1 3
```

### 输出样例

```
101 2
```

### 输入样例2

```
5
new gcroot 1
new gcroot 2
new object 3 1
assign 1 3
remove 1
```

### 输出样例2

```
0 0
```

### 代码

题面很长，抄了很多东西，但题目很简单，因为并没有让你收垃圾，而且数据量很小。只需要从每个活跃的根去标记一下可达的内存数量和大小就可以了。

```
#include <stdio.h>
#include <assert.h>

#define MAXN 10000
#define MAX_ID 10000
#define MAX_OBJ 10000

typedef struct _Node {
    size_t count;
    int vis;
    struct _Node** array;
} Node;

Node* create_node(int n) {
    Node* result = (Node*)malloc(sizeof(Node));
    result->count = n;
    result->vis = 0;
    result->array = (Node**)malloc(n * sizeof(Node*));
    for (int i = 0; i < n; i++)
        result->array[i] = NULL;
    return result;
}

Node* root[MAXN];
int mapping[MAX_ID];

Node* objects[MAX_OBJ];

void new_object(int object_id, int size) {
    objects[object_id] = create_node(size);
}

void new_gc_root(int root_id) {
    // do nothing
}

void remove_root(int root_id) {
    root[root_id] = NULL;
}

void link_to_object(int from, int offset, int to) {
    objects[from]->array[offset] = objects[to];
}

void link_to_root(int root_id, int object_id) {
    root[root_id] = objects[object_id];
}

int vis[MAX_OBJ];
Node* stack[MAX_OBJ];
int top;

int main(int argc, char** argv) {
    // freopen(argv[1], "r", stdin); int n;
    scanf("%d", &n);
    char oper[100];
    char type[100];
    int id;
    int size;
    int sum = 0;
    int reach = 0;
    for (int t = 0;t < n; t++) {
        scanf("%s",oper);
        // new
        if (oper[0]== 'n') {
            scanf("%s", type);
            // create object
            if (type[0] == 'o') {
                scanf("%d %d", &id, &size);
                sum += size;
                new_object(id, size);
            } else {
                // new gc root
                scanf("%d", &id);
                new_gc_root(id);
            }
        } else if (oper[0] == 'l') {
            int k;
            scanf("%d %d %d", &id, &size, &k);
            link_to_object(id, size, k);
        } else if (oper[0] == 'a') {
            scanf("%d %d", &id, &size);
            link_to_root(id, size);
        } else if (oper[0] == 'r') {
            scanf("%d", &id);
            remove_root(id);
        } else {
            break;
        }
    }

    for(inti=0;i<MAX_ID;i++){
        if (root[i]) {
            stack[top++] = root[i];
        }
    }
    int cnt2 = 0;

    while (top) {
        Node* cur = stack[--top];
        if (cur->vis)
            continue;

        cur->vis = 1;
        reach += cur->count;
        cnt2 ++;
        for (int i = 0; i < cur->count; i++) {
            if (cur->array[i] && !cur->array[i]->vis) {
                stack[top++] = cur->array[i];
            }
        }
    }
    printf("%d %d\n", reach, cnt2);
}
```

## Math Strikes Back

### 题目概述

若正整数a符合a与a^2在6进制下没有相同的数字，称a为好数。

输出前K个好数。0≤K≤500

### 函数接口定义

```
输入格式:
输入一个数字K。

输出格式:
输出K行，第i行以6进制输出第i个好数，每行以回车结尾。
```

### 输入样例

```
4
```

### 输出样例

```
2
5
32
35
```

### 代码

题目大意

若正整数a符合在6进制下不存在某个数码(012345)既在a中出现、又在a2中出现，则称a为好数。

解题思路

可以发现确定a最小的k位，就能确定a2最小的k位。 从低到高枚举每一位，过程中如果不符合条件就return即可。

注意某些弱智(出题人)要跑满30位的所有情况。最后把表打出来即可。

参考数据

第500个是225255525525525522555555555255。

## 图灵测试

### 题目概述

浙江大学微软学生俱乐部出现了一个神秘的成员，他从不参加内训和例会，却无时无刻不活跃在QQ群中，和MSC的成员们进行激烈的交流。这让MSC的首席计算机科学家htt感到很疑惑，她怀疑这个人可能是微软人工智能实验室的最新研究成果——一个拥有人类智慧的机器人！为了辨别这个人的真实身份，htt想到了计科面试中教授问到的关于图灵测试的问题，她决定用这个方法来判断对方是人类还是机器。

图灵测试（英语：Turing test，又译图灵试验）是图灵于1950年提出的一个关于判断机器是否能够思考的著名试验，测试某机器是否能表现出与人等价或无法区分的智能。

测试者与被测试者（一个人和一台机器）隔开的情况下，通过一些装置（如键盘）向被测试者随意提问。

进行多次测试后，如果测试者不能确定出被测试者是人还是机器，那么这台机器就通过了测试，并被认为具有人类智能。

由于测试需要大量对话数据支持，htt在和神秘成员彻夜长谈后睡着了！你能通过她留下的宝贵聊天记录判断对方是人类还是机器吗？

### 函数接口定义

```
输入格式
第一行给出一个整数T，（T≤10）表示接下来有T组测试数据

对于每组测试数据第一行给出一个整数N（4≤N≤100，且N为偶数）。

接下来N行为科学家与神秘人的对话，第2k行为科学家的发言，第2k+1行为神秘人的发言。

每一句发言以"Scientist: "或"Stranger: "开头，（冒号后面有一个空格），接下来是他们交流的内容，由英文字母、标点符号和空格组成。内容结束后有一个换行符。每行的字符总数不超过100。

输出格式
输出共T行，对于每个测试数据，若你的程序判断它为机器，则输出“Robot”。若判断他是一个人类，输出“Human-being”。
```

### 输入样例

```
2
4
Scientist: httnb!
Stranger: httnb!
Scientist: Licking dog won't get house.
Stranger: httnb!
4
Scientist: Php is the best language in the world!
Stranger: reject!
Scientist: Python is the best language in the world!
Stranger: reject!
```

### 输出样例

```
Robot
Robot
```

### 代码

```
#include <stdio.h>
#include <string.h>
char s1[1005], s2[1005], tmp[1005];
int main(){
    int cas, n, i; scanf("%d", &cas);
    while(cas--){
        scanf("%d", &n);
        int flag = 1;
        for(i = 1; i <= n / 2; ++i){
            scanf("%s", tmp); gets(s1);
            scanf("%s", tmp); gets(s2);
            if(strcmp(s1, s2) != 0) flag = 0;
        }
        if(flag) puts("Human-being");
        else puts("Robot");
    }
    return 0;
}
```

### 代码2

```
#include <stdio.h> 
#include <stdlib.h>

#define FORP(i,a,b) for(i=(a);i<=(b);i++) 
#define maxn 100005 
/*==================split line==================*/ 

char s[3][maxn];
int cnt[3];
int main(){ 
    int cas; scanf("%d",&cas);
    while (cas--){
        int n,i,j; scanf("%d\n",&n);
        int ff = 1;
        FORP(j, 1, n / 2){
            cnt[1] = 0, cnt[2] = 0;
            FORP(i, 1, 2) {
                while ((s[i][++cnt[i]] = getchar()) != '\n'); cnt[i]--;
            }
            if (cnt[1] - 1 != cnt[2]) ff = 0;
            FORP(i, 10, cnt[1]) if (s[1][i] != s[2][i - 1]) ff = 0;
        }
        if (ff) puts("Human-being");
        else puts("Robot");
    }
}
```

## 约翰的翳病

### 题目概述

去年约翰的翳病使他只能看到单词的前一半，而今年他的玩了星际后情况更糟了。

他在看到两个连续字母的时候有可能会将其中一个认错，但他绝不会连续认错两个字母。

善于甩锅的约翰决定将这个任务交给你，你愿意帮约翰找出他可能混淆的两个单词吗？

（对于善于脑补的约翰而言，单个字母的单词总是一样的）

### 函数接口定义

```
Input:
第一行一个整数n 代表测试组数 0<n<100;
下面有 n 每组包含两行代表两个单词。单词长度0<l<100

Output:
一个表示共有多少组有可能让John看错的整数
```

### 输入样例

```
4
hate
date
EDD
ADS
LMG
EBR
o
p
```

### 输出样例

```
3
```

### 代码

```
#include<stdio.h>
#include<string.h>
int check(char *str1,char* str2){
    if (strlen(str1) != strlen(str2)) return 0;
    if (strlen(str1) == 1)  return 1;
    for(int i = 1; str1[i] != 0; i++){
        if (str1[i] != str2[i] &&
                 str1[i-1] != str2[i-1])
                     return 0;
    }
    return 1;
}

int n;
char str1[10000],str2[10000];
int sum;
int main(){
    scanf("%d", &n);
    for(int i = 0; i < n; i++){
        scanf("%s", str1);
        scanf("%s", str2);
        if (check(str1, str2)) sum++; 
    }
    printf("%d\n", sum);
}
```

## 玩游戏

### 题目概述

小Z是以撒的结合（The Binding of Issac）的终极粉丝。可这个游戏新出的dlc却难倒了他。刚出生的以撒有一颗魂心，当它失去所有魂心时游戏结束。游戏的地图由一排n个相连的房间构成。房间一共有一下三个类型：

```
牺牲房（Sacrifice Room）。当你走进这个房间时你会失去一颗魂心。
宝藏房（Treasure Room）。当你走进这个房间时你会捡到一枚金币。
天使房（Angel Room）。当你走进这个房间时你会获得一颗魂心。
```

你可以从任意一个房间进入，开始游戏。你只能向右一个一个房间走过去直到前方没有房间或以撒死亡。请问你最多能捡到多少个金币？

数据范围：

对于10%的数据，字符串只含字符T。

对于10%的数据，字符串不含字符T。

对于40%的数据，n≤1000

对于70%的数据，n≤10^5

对于100%的数据，n≤10^6

### 函数接口定义

```
输入格式
第一行一个整数n（n≤10^6)，表示有n个房间。

第二行有一个长为n的字符串，由字母'S'、'T'、'A'构成，其中S表示牺牲房，T表示宝藏房，A表示天使房。

输出格式
输出一行一个整数，表示收集到的最大金币数量。
```

### 输入样例

```
7
TSATSTS
```

### 输出样例

```
2
```

小Z选择从第三个房间进入游戏，经历了“ATSTS”五个房间，最终在第7个房间死去。共获得两个金币。

### 代码

题目大意:

n个连在一起的房子，走进某个房子可能会加血掉血或获得一枚金币，求在血量始终大于0地走过一段房子能够获得的最大金币数量

解题思路:

首先对于部分分，没有T和只有T都是可以直接输答案的
$$n \leq 1000$$的情况 可以验证每一段能够获得的金币数进行比较。
设置$$n \leq 10^5$$ 和 $$n \leq 10^6$$是为了防止同学用$$O(n^2)$$带一些奇怪的优化卡过去，并鼓励大家思考优美的做法...

这道题应该有很多做法，但是其实这个问题最简单的做法是从头到尾按照题意模拟，一旦血量下降为0时在这个点上重置血量和当前所获得的金币数，走到尾即可。

为什么这么做是对的呢？我们假设最优方案的起点不是上述路径中的某一个起点，那么它必然被包含在上述做法求出的某一条路径中。由于游戏要求血量大于1时才能继续进行，所以原路径肯定不会比当前路径劣，因此上述做法是正确的

```
#include <stdio.h>
#include <stdlib.h>
int max(int a, int b){return a > b ? a : b;}
int main(){
    int n; scanf("%d\n",&n);
    int i, ans = 0;
    int hp = 1, sum = 0;
    for (i = 1; i <= n; i++){
        char c = getchar();
        switch(c){
            case 'A': hp++; break;
            case 'T': sum++; break;
            case 'S': hp--; break;
        }
        if (hp == 0){
            ans = max(ans, sum); hp = 1, sum = 0;
        }
    }
    ans = max(ans, sum);
    printf("%d\n", ans);
}
```

## 婷婷的桃子

### 题目概述

婷婷得到了n堆桃子，第 i 堆桃子有 a[i] 个。婷婷想要把每堆桃子分成尽可能多的相同等份。但毋庸置疑，在桃子数量参差不齐的时候，这无疑是十分困难的，所以婷婷决定去掉其中一堆桃子。

现在她请求你来帮助她选择去掉哪一堆桃子，使得最后剩下的每堆桃子可以分成 K 等份，且使 K 达到最大。请你输出能够达到的最大整数K。

### 函数接口定义

```
输入格式:
输入第一行T，表示数据组数。
接下来输入T组数据，每组数据给定一个正整数n(n>1)，表示有几堆桃子。接下来一行n个int范围内的整数表示每堆桃子的个数。

输出格式:
请你输出T行正整数。表示每组数据的答案K。

数据范围:
100%: n<=100000
```

### 输入样例

```
3
3
1 1 1
5
2 2 2 3 2
4
1 2 4 8
```

### 输出样例

```
1
2
2
```

### 代码

题目大意

给定n个正整数，求去掉一个数后，剩下数的gcd的最大值。

解题思路

```
40分做法：枚举去掉哪一堆桃子，每次对剩下的桃子用辗转相除法计算最大公因数，答案取Max。
100分做法：考虑线性做法。我们知道计算gcd的顺序对最终的结果是没有任何影响的，因此采用前缀/后缀思想。left[i]表示前i个数的最大公因数，right[i]表示第i个数到第n个数的最大公因数。所以只需要一次遍历，枚举去掉哪一堆桃子，通过 gcd(left[i-1],right[i+1]) 便可以快速计算出剩下桃子的最大公因数。
```

```
#include<stdio.h>
#define mid ((l+r)>>1)
#define ll long long
#define maxn 100010
int a[maxn],left[maxn],right[maxn];
int gcd(int a,int b){
    if(b==0)return a;
    return gcd(b,a%b);
}
int max(int a,int b){
    if(a>b)return a;else return b;
}
int main(){
    int n,cas;
    scanf("%d",&cas);
    while(cas--){
        scanf("%d",&n);
        for(int i=1;i<=n;i++){
            scanf("%d",&a[i]);
        }
        left[1]=a[1];
        right[n]=a[n];
        for(int i=2;i<=n;i++)left[i]=gcd(left[i-1],a[i]);
        for(int i=n-1;i>=1;i--)right[i]=gcd(right[i+1],a[i]);
        int ans=max(right[2],left[n-1]);
        for(int i=2;i<n;i++){ans=max(ans,gcd(left[i-1],right[i+1]));}
        printf("%d\n",ans);
    }
}
```

## 是秘密啊

### 题目概述

给定一个日期，输出对应的星座。

### 代码

```
#include <stdio.h>
const char ch[12][50] =
{"Capricorn","Aquarius","Pisces","Aries","Taurus","Gemini",
"Cancer","Leo","Virgo","Libra","Scorpio","Sagittarius"};
const int d[13] = {0,19,18,20,19,20,21,22,22,22,23,22,21};
int n, m;

void solve() {
    scanf("%d%d", &n, &m);
    int ans = 0;
    if ((n==1 && m<=19) || (n==12 && m>=22)) return puts(ch[0]), (void)0;
    for (int _=1;_<=12;_++) if (n>_ || (n==_ && m>d[_]) ) ans++;
    puts(ch[ans]);
}

int main() {
    int T; scanf("%d",&T);
    while (T--) solve();
    return 0;
}
```

## 玩球

### 题目概述

桌面上有编号为$1$,$2$,$3$的三个盒子，每个盒子里面有一个球。
$1$号盒子里的球是红球，$2$、$3$号盒子里的球是白球。
现在有$M$个操作，每次从$x_i$​ 盒子中取任意一个球放入$y_i$​​盒子。
现在询问每个盒子在所有操作结束后里面是否有可能有红球。

### 代码

我们称可能有红球的盒子是红色的。对于一次操作

```
1、$y_i$盒子颜色的变动
如果$x_i$不是红色的，那么这次操作不影响$y_i$的颜色。
如果$x_i$是红色的，那么$y_i$盒子也是红色。
2、$x_i$盒子颜色的变动
如果$x_i$盒子在操作后没有球了，那么该盒子一定不可能是红色。
如果$x_i$盒子在操作后还有球，则该盒子颜色不变。
```

```
#include <stdio.h>
#include <string.h>
inline int rd() {int r;scanf("%d",&r);return r;}
int a[5], b[5], n, T;
int main() {
    for (int T=rd();T;T--) {
        n = rd();
        a[1] = a[2] = a[3] = 1;
        b[1] = 1, b[2] = b[3] = 0;
        for (int i=1;i<=n;i++) {
            int x = rd(), y = rd();
            b[x] ? b[y] = 1 :0;
            --a[x]; ++a[y];
            !a[x] ? a[x] = b[x] = 0 : 0;
        }
        
        for (int i=1;i<=3;i++) puts(b[i] ? "Yes" : "No");
    }
    
    return 0;
}
```

## 计算题

### 题目概述

给定正整数n，对n进行k次开平方根操作，输出结果（保留3位小数）。

### 代码

在k比较大的时候程序会超时。
$$\lim_{k\to +\infty}n^{\frac{1}{2^k}}=1$$
在$k$较大的时候答案会迅速向$1$靠近，在$k$比较大的时候输出$1.000$即可。（阈值设置成$16$时即可通过此题，即$k$大于$16$输出$1.000$）
使用其他方法，减少$k$较大时的计算，如二分答案再乘回去在数字比较大的时候退出，或者使用pow函数先计算指数都可以通过此题。

```
#include <stdio.h>
#include <math.h>
inline int rd() {int r;scanf("%d",&r);return r;}
inline int min(int a,int b) {return a<b ? a : b;}
int main() {
    int n = rd(), k = min(rd(), 16);
    double ans = n;
    for (int _=1;_<=k;_++) ans = sqrt(ans);
    printf("%.3f\n", ans);
    return 0;
} 
```

## Data structure alignment

### 题目概述

给定以下c/c++结构体内存对齐的规则，要求根据一给定结构体的成员定义顺序计算最终结构体所占空间字节数。

```
1. **对于结构体的各个成员，第一个成员的偏移量是0，排列在后面的成员其当前偏移量必须是当前成员类型的整数倍**
2. **结构体内所有数据成员各自内存对齐后，结构体本身还要进行一次内存对齐，保证整个结构体占用内存大小是结构体内最大数据成员的最小整数倍**
3. **如程序中有`#pragma pack(n)`预编译指令，则规则1中每个数据成员的对齐按照`#pragma pack`指定的对齐参数n和这个数据成员自身长度中比较小的那个进行；规则2中不再考虑最大结构体内类型，直接以该对齐参数进行对齐，且参数n的取值只能为1，2，4，8，16**
```

### 代码

按照题意模拟即可。

数据点0为样例；数据点1为对齐参数为1的情况；数据点2为无对其参数的情况；数据点3为所有可能情况。

模拟时只要记录当前偏移字节数，按照需要整除的对齐参数/成员大小进行调整，最后以最大的成员所占字节或给定的对齐参数对结构体进行最终的大小调整。

```
#include <stdio.h>
#include <string.h>
#include <assert.h>
int a[10001];
int main()
{
  int T;

  scanf("%d", &T);
  assert(T >= 1 && T <= 100);
  while (T--)
  {
    int N, Len, i;
    int ans = 0;
    scanf("%d", &N);
    scanf("%d", &Len);
    assert(N >= 1 && N <= 100);
    assert(Len == -1 || Len == 1 || Len == 2 || Len == 4 || Len == 8 || Len == 16);
    for (int i = 1; i <= N; i++)
    {
      char str[10];
      scanf("%s", str);
      if (strcmp("BYTE", str) == 0)
        a[i] = 1;
      else if (strcmp("WORD", str) == 0)
        a[i] = 2;
      else if (strcmp("DWORD", str) == 0)
        a[i] = 4;
      else if (strcmp("QWORD", str) == 0)
        a[i] = 8;
      else
      {
        printf("%s\n", str);
        assert(0);
      }
    }
    if (Len == -1)
    {
      for (int i = 1; i <= N; i++)
        Len = Len < a[i] ? a[i] : Len;
    }
    for (int i = 1; i <= N; i++)
    {
      int l = Len < a[i] ? Len : a[i];
      if (ans % l != 0)
        ans = ans + (l - (ans % l));
      ans += a[i];
    }
    if (ans % Len != 0)
      ans = ans + (Len - (ans % Len));
    printf("%d\n", ans);
  }
}
```


## Variadic Function

### 题目概述

想必学习过 C 语言的各位一定都使用过 printf 来格式化输出过一些东西，那么有没有了解过 printf 之类的变参函数是如何实现的呢？事实上在 C 调用约定下我们可以使用 va 系列宏来轻松的实现一些变参函数，例如：

```
#include <stdarg.h>
int sum(int count, ...) {
    va_list args;
    va_start(args, count);
    int res = 0;
    for (int i = 0; i < count; i++) {
        int val = va_arg(args, int);
        res += val;
    }
    va_end(args);
    return res;
}
```

代码本身很简单，这里稍微解释一下

`va_list args;`定义一个指向可变参数列表的指针

`va_start(args, count);`使参数列表指针指向函数参数 count

`va_arg(args, int)`把参数列表指针当前所指位置以类型 int 读取出来并移动参数列表指针

`va_end(args)`清空参数列表。va_start 和 va_end 必须一一对应 。”每次调用va_start() / va_copy()后，必须得有相应的va_end()与之匹配。参数指针可以在参数列表中随意地来回移动，但必须在va_start() … va_end()之内“

当然大家可以发现这种形式的可变参数函数是非常危险的，va_arg 基本就是强制类型转换，而且读取的参数个数也只能通过用户输入来确认，很容易出现访问参数越界的情况。同时 va_arg 取出的参数类型和实际传入的类型不一致，或是访问最后一个参数之后的参数是未定义行为，在不同平台、不同编译器实现下的结果可能不完全相同，所以在使用这些函数的时候也请大家务必注意参数的个数和类型问题。

这道题需要你实现一个简单的 printf ，接受一个格式化字符串和若干参数，将结果打印至标准输出。

满足：

使用 `'$'` 作为转义字符，在需要输出 `'$'` 字符的时候重复一次 `'$'` , 其它情况下视为输出对应类型的参数
支持 `$d` 输出 32 位整数(int)， `$s` 输出字符串(char*)直至 `'\0'` ，`$f`输出双精度浮点数 (double), 保留 6 位小数 , `$c` 输出单个字符 (char)，保证输入合法
返回其中打印的字符个数
保证 `'$'` 后不会出现上述提及情况以外的字符

### 函数接口定义

```
int simple_printf(const char* fmt, ...);
```

其中`fmt`为传入的格式化字符串，`...` 为传入的其它参数，返回输出的字符串长度

### 裁判测试程序样例

保证单次调用输出的长度小于 4096，输出的浮点数保留 6 位小数

```
#include <stdio.h>
#include <stdarg.h>

int simple_printf(const char* fmt, ...);

int main() {
    printf("%d\n", simple_printf("123 "));
    printf("%d\n", simple_printf("$$ "));
    printf("%d\n", simple_printf("$d ", 123));
    printf("%d\n", simple_printf("$c ", '1'));
    printf("%d\n", simple_printf("$s ", "123"));
    printf("%d\n", simple_printf("$f ", 123.4));
    return 0;
}

/**
*   your code here
**/
```

### 输出样例

```
123 4
$ 2
123 4
1 2
123 4
123.400000 11
```

### 代码（未完成）

# PAT甲级

## 1001 A+B Format

### 题目概述

Calculate a+b and output the sum in standard format -- that is, the digits must be separated into groups of three by commas (unless there are less than four digits).

### 函数接口定义

```
Input Specification:
Each input file contains one test case. Each case contains a pair of integers a and b where −10^6≤a,b≤10^6. The numbers are separated by a space.

Output Specification:
For each test case, you should output the sum of a and b in one line. The sum must be written in the standard format.
```

### 输入样例

```
-1000000 9
```

### 输出样例

```
-999,991
```

### 代码

```
#include <iostream>
using namespace std;

int main(){
    int a,b;
    int ans,bak;
    char str[100010]={0};

    cin>>a>>b;
    ans=a+b;

    int i,j,flag;
    i=0;
    bak=ans;
    if(ans<0) ans=-ans;
    while(ans!=0){
        flag=0;
        for(j=0;j<3;j++){
            if(ans==0){
                flag=1;
                break;
            }
            str[i++]=ans%10+'0';
            ans/=10;
        }
        if(ans==0 || flag==1) break;
        str[i++]=',';
    }
    if(bak<0) str[i++]='-';

    if(bak==0) cout<<"0";
    else
        for(j=i-1;j>=0;j--)
            printf("%c",str[j]);
}
```

# 诚信守则

```
本人有义务保护好自己的帐号和密码。
I have a responsibility to protect my account number and password.
答案：是

对于号称帐号被盗、非本人提交的作弊代码，除非有技术手段查到非本人提交的证据，否则本人承担责任，被判作弊。
In the event another student submits plagiarized class work in my name, I assume responsibility for cheating, unless there exists evidence to the contrary.
答案：是

本课程的课后作业必须独立完成。
I must complete all homework assignments independently.
答案：是

在作业提交截止、自己的得分确定之前，从任何资源找到他人的作业答案阅读、参考、提交，都是作弊行为。
Before the submission deadline and release of grades, it is cheating to seek out from any source and to read/refer to/submit the assignments of others.
答案：是

提交的作业中包含他人的作品（包括文字、代码、图片、视频等），或对他人的原句、原文段落、具体思路有所引用，但未注明出处或致谢，此种行为属于抄袭。
If your submitted work contains pieces of others' work (such as the text, code, figures, video, etc.), or cites other people's original sentences, paragraphs, or solution ideas without clearly referencing or acknowledging its source, it constitutes plagiarism.
答案：是

伪造实验数据属于作弊。
The forgery or falsification of experimental data constitutes cheating.
答案：是

提交作品参加同行双盲互评时，提交的任何文件的任何地方不能出现足以暴露作者身份的信息，包括作者姓名、学号、昵称、组号、以及文件属性中包含的作者信息等。违者本次作业应被判零分。
If the work submitted for peer review reveals the identity of the authors in any way (name(s), student ID's, nickname(s), group number(s), file properties, etc), the work will be assigned a score of zero.
答案：是

【真实案例】学生A有一个函数不会写，找学生B教他。B教A怎样写这个函数，并且把自己的代码给A看。A和B都应被判作弊。
【Real Case】Student A had a function he could not figure out how to write, and then went to student B to ask for help. Student B taught student A how to write the function, and showed student A his own code. Both student A and B had participated in cheating.
答案：是

【真实案例】学生A有一个函数不会写，找学生B教他。B给A口头讲解了这个函数的思路，没有给A看自己的代码。A和B都不应被判作弊。
【Real Case】Student A had a function he could not figure out how to write, and then went to student B to ask for help. Student B gave student A a verbal explanation of the function, without showing student A any code. Neither student A nor student B had participated in cheating.
答案：是

【真实案例】学生A通过贴广告求职做家教，结果招来了同班同学B请他做家教，辅导课程作业。A应该避嫌，不接受B的邀请。
【Real Case】Student A advertised tutoring services, and student B in the same class asked student A to be her tutor and to help completing course work. Student A should not help.
答案：是

【真实案例】学生A和B提交了极为相似的作业，这份作业是两人分工完成的，每人都做了一半的工作量。A和B不应被判作弊。
【Real Case】Student A and B both submitted very similar assignments. One half was completed by student A, another by student B. Neither student should be punished for cheating.
答案：否

【真实案例】学生A和B提交了极为相似的作业。学生A表示自己是原创，并不知道被抄袭。学生B表示自己是从网上抄的。A和B都应被判作弊。
【Real Case】Student A and B both submitted very similar assignments. Student A claimed his work is original, and was unaware it had been copied. Student B admitted he copied it from the Internet. Both student A and student B should be convicted of cheating.
答案：是

【真实案例】学生A和B提交了极为相似的作业。学生A表示自己是原创，并不知道被抄袭。学生B承认自己盗窃了A的作业。A应被警告，B应被判作弊。
【Real Case】Student A and B both submitted very similar assignments. Student A claimed his work is original, and was unaware it had been copied. Student B admitted to copying student A's homework. Student A should be given a warning, and student B should be punished for cheating.
答案：是

【真实案例】学生A独立完成了一份作业，并把代码给了别人。A应被判作弊。
【Real Case】Student A completed his assignment independently, and then distributed his code to other students. Student A should be punished for cheating. 
答案：是

【真实案例】学生A把自己以前独立完成的课程作业上传到某论坛。A不应被判作弊。
【Real Case】Student A completed his assignment independently, and then posted his work on a forum. Student A should not be convicted of cheating.
答案：否

【真实案例】以前上过某课的学生A把自己的作业给了本学期正在上该课的学生B参考，A和B都应被判作弊。
【Real Case】Student A, who had previously taken this course, gave his homework to student B, who was currently enrolled in the course. Both student A and student B should be punished for cheating.
答案：是

【真实案例】某年学生A将作业答案发给学生B。第二年老师布置了同样的题目，学生B将答案放到QQ空间，后被转载到校内论坛。数十位学生参考了该答案。大部分学生自己也进行了解答，但参考了该答案后对自己的解答进行了调整。学生A、B和所有参考了该答案的学生都应被判作弊。
【Real Case】Student A sent his homework answers to student B. The next year, a Professor assigned a similar question. Student B uploaded the zip file of these answers on QQ, and then the file was forwarded on the school forum. Dozens of students referenced these answers to complete their homework. Most students first completed the question by themselves, but altered their work after seeing the answers. Student A, student B, and all students who used the answers to complete their assignment should be punished for cheating.
答案：是

【真实案例】学生A把自己独立完成的作业代码放到开源社区，第二年被学生B抄袭。A和B都应被判作弊。
【Real Case】Student A completed his homework independently, and released his code as open source. The next year, student B plagiarized said code. Both student A and student B should be punished for cheating.
答案：是

【案例】学生A把自己在实际工作中的代码放到开源社区，其中一部分代码被学生B抄袭。B应被判作弊，A无任何责任。
【Hypothetical Case】Student A releases the code they have produced during industry working as open source. Student B copies a section of that code to complete their assignment. Student B should be punished for cheating, but student A assumes no responsibility.
答案：是

【真实案例】学生A在作业提交截止前，从网上找了一份答案提交，后又提交了自己独立完成的、并未满分的答案，并主动向老师坦白自己的答案并未满分。A不应被判作弊。
【Real Case】Prior to an assignment deadline, student A found a correct answer on the Internet and submitted it as his own work. Later, he submitted his own work, which was done independently and was not fully correct. He took the initiative to admit to the Professor that his work should not get full marks. Student A should not be punished for cheating.
答案：否

【真实案例】学生A在作业提交截止、自己的得分确定后，请老师重新开放作业集，从网上找到答案学习，并且提交了其他人的答案和自己修改后的答案。A应被判作弊。
【Real Case】After an assignment deadline had passed, and the grading determined, student A asked the Professor to allow her to resubmit the assignment. Student A used the Internet, and the answers of other students, to revise her work. Student A should be punished for cheating.
答案：否

【真实案例】甲校学生A并未在乙校选课，但旁听乙校课程时把自己独立完成的作业代码放到开源社区，被乙校学生B抄袭。A应被跨校警告，责令立刻消除协助作弊的影响；B应被判作弊。 
【Real Case】Student A was from another university, but was sitting in a course at this university. Student A released his assignment code as open source, and student B in this university plagiarized said code. Student A should be given a cross-school warning, ordering him to immediately eliminate the impact of cheating; student B should be punished for cheating.
答案：是

【案例】学生A在网上的开放题库中自主练习，并且在博客中发布了解题报告和代码。后该题目被老师作为作业发布，该生博客中的代码被学生B抄袭。B应被判作弊，A无任何责任。
【Hypothetical Case】Student A practiced on an open online judge (OJ) independently, and posted code and solutions to their blog. Later, a Professor assigned one of the problems on that OJ as homework, and student B plagiarized said code. Student B should be punished for cheating, but student A is not responsible.
答案：是


本课程的课程设计团队作业需要本团队独立完成。
The team projects for this class should be completed independently by each group.
答案：是

团队提交的作业中包含团队成员以外的他人的作品（包括文字、代码、图片、视频等），或对他人的原句、原文段落、具体思路有所引用，但未明确注明出处或致谢，此种行为属于抄袭。
If a team submitted work contains pieces of work (such as the text, code, figures, video, etc.) of someone other than this team's members, or cites other people's original sentences, paragraphs, or solution ideas without clearly referencing or acknowledging its source, it constitutes plagiarism.
答案：是

在团队任务中未完成本人分工的任务，却在报告中给出虚假分工和承诺，此种行为属于作弊。
A false promise or misrepresented contribution in one's team report constitutes cheating.
答案：是

在团队任务中未能阻止、或举报组员抄袭行为，属于协助作弊，应与直接作弊者一同处罚。
If a group becomes aware of plagiarism within their team, and fails to report or otherwise prevent it, they are complicit in cheating and should all be punished accordingly.
答案：是

学生须携带照片清晰的身份证或学生证于考试开始前10分钟到达考场。
Students are required to bring a legible citizenship ID card / passport or their student ID card with a clear photo 10 minutes before the start of an examination.
答案：是

携带考试规定以外的物品进入考场并且未放在指定位置属于考试违纪。
Bringing items other than those allowed to be carried into the examination room, or failed to place them at the specific position, constitutes a violation of the examination regulations.
答案：是

未在规定的座位参加考试不属于考试违纪。
Failing to write the exam in your assigned seat is not a violation of the examination regulations.
答案：否

考试开始信号发出前答题或者考试结束信号发出后继续答题不属于考试违纪。
Writing the exam before it has been indicated to begin, or continuing to write the exam after it ends, are not violations of the examination regulations.
答案：否

在考试过程中旁窥、交头接耳、互打暗号或者手势属于考试违纪。
Peeping, whispering, gesturing, or otherwise sharing answers during the exam are all violations of the examination regulations.
答案：是

在考场禁止的范围内喧哗、影响考场秩序属于考试违纪。
Making excessive noise within a prohibited area near the examination room or be otherwise disruptive in the examination room are violations of the examination regulations.
答案：是

未经考试工作人员同意在考试过程中擅自离开考场不属于考试违纪。
Leaving the examination room without the permission of the invigilator(s) is not a violation of the examination regulations.
答案：否

将试卷、答题纸、草稿纸等考试用纸带出考场属于考试违纪。
It is a violation of examination regulations to remove any examination materials from the examination room (including, but not limited to, the examination paper, answer sheet, and draft paper).
答案：是

在闭卷考试中，携带与考试课程内容相关的文字材料或者存储有与考试内容相关资料的电子设备等参加考试，属于考试作弊。
Bringing written or electronic materials related to course content into a closed book exam is cheating.
答案：是

在考试用桌上或者身体上涂写任何与考试课程内容有关的文字和符号的，属于考试作弊。
Writing testable material on the desk in the exam room, or on one's body, is cheating.
答案：是

违规使用电子工具或通讯工具属于考试作弊。
Using banned electronic tools or other means of communication during an exam is cheating.
答案：是

抄袭他人试卷或者与考试内容相关的材料属于考试作弊。
The plagiarism of another student's work or the use of related course materials to complete the exam is cheating.
答案：是

故意将自己试卷或者与考试内容相关的资料让他人抄袭属于考试作弊。
Intentionally allowing others to copy one's own test paper or other examination content, is cheating.
答案：是

报对答案及传递纸条、试卷、答卷、草稿纸属于考试作弊。
Passing around answers, test papers, the answer key, or draft paper for an exam are all acts of cheating.
答案：是

抢夺、窃取他人试卷、答卷或者强迫他人为自己抄袭提供方便，属于考试作弊。
Robbing or stealing another student's test answers, or forcing them to disclose their answers for the purpose of plagiarism, is cheating.
答案：是

借故暂离考场以得到答案属于考试作弊。
Leaving the examination room during the exam, in order to obtain answers to the exam, is cheating.
答案：是

使用通讯设备及其他工具发送、接收考试相关内容，属于考试作弊，情节严重可开除学籍。
The use of communication equipment to send or receive answers during an exam is is cheating, and may result in expulsion in a serious case.
答案：是

替他人参加考试或由他人代替考试属于考试作弊，情节严重可开除学籍。
Either allowing others to sit your exam, or sitting an exam for others are both cheating, and may result in expulsion in a serious case.
答案：是

组织作弊者，情节严重可开除学籍。
Conspiring to cheat -- either with others or as an individual -- is a serious offence that may result in expulsion.
答案：是

窃取试卷者，情节严重可开除学籍。
Stealing test papers is a serious offence that may result in expulsion.
答案：是

篡改分数者，情节严重可开除学籍。
Tampering with scores is a serious offence that may result in expulsion.
答案：是

在上机考试中，只允许使用“PTA客户端”（桌面快捷方式“oms-client”）访问PTA。
The "PTA client" browser (desktop shortcut "oms-client") is the only tool
allowed for accessing PTA during an onine exam.
答案：是

在上机考试中，只能用自己的账号登录。企图用任何其他账号登录的，属于考试作弊。
During an online exam, you may only log in using your own credentials. Any attempt to use credentials other than your own is cheating.
答案：是

在上机考试中，启动“PTA客户端”以外的任何其他浏览器都属于考试作弊。
During an online exam, activating any browsers other than the "PTA client" is cheating.
答案：是

在上机考试中，未经监考老师允许不得关闭“PTA客户端”窗口，更不得以任何方式登出。违者属于考试作弊。
During an online exam, DO NOT close the "PTA client" or log out without the permission of the exam invigilator(s). Otherwise it constitutes cheating.
答案：是

在上机考试中，即使提前退出考试，也不可以自己关闭“PTA客户端”，必须等监考老师来处理。
During an online exam, even if you finish the exam early, you may not close the "PTA client". You must wait for the help of an exam invigilator.
答案：是

在上机考试中，严禁访问除PTA外的其它网站，严禁使用任何即时通讯工具。违者属于考试作弊。
During an online exam, it is strictly prohibited to access any websites other than PTA, especially any instant messaging services. Any attempt to do so is cheating. 
答案：是

在开卷考试中，携带禁止的资料或者工具属于考试违纪。
Bringing prohibited materials or tools into an open book exam is a violation of the examination regulations.
答案：是

在开卷考试中，与其他考生共享允许携带的参考资料（如教材等），不属于考试作弊。
During an open book exam, it is not cheating to share reference materials (such as textbook) with other students. 
答案：否

【真实案例】期末考试交一篇论文，某学生选取某知名企业为研究对象。他查看了该企业主页上的人物介绍并直接拷贝到论文中。这种行为属于抄袭，判考试作弊。
【Real Case】For a final project, a student chose a well-known company as a subject of study. He looked up some information on the company homepage, and copied sections of it directly into his own work. This is plagiarism, and constitutes cheating.
答案：是

本人明确理解并同意：（1）此诚信测试不达满分则无资格参加本课程期末考试；（2）本学期只要出现一次抄袭或作弊行为则无资格参加本课程期末考试，且本课程成绩为零分。
I fully understand and agree that: (1) a failure to score full marks on this Academic Honesty test disqualifies me from writing the final exam; (2) Only one act of cheating or plagiarism during this term disqualifies me from writing the final exam, and automatically assigns a grade of zero in this course.
答案：是

本人明确理解并同意：如果对作弊或抄袭判罚有异议，须在接到判罚通知一周内向任课教师提出。过期不再受理。
I fully understand and agree that: if I object to a plagiarism or cheating penalty, I must submit my complaint to the Professor within one week of receiving the penalty. After one week has passed, the complaint will no longer be accepted.

答案：是

本团队明确理解并同意：本学期的团队作业只要出现一次抄袭或作弊行为，则全体成员无资格参加本课程期末考试，且本课程成绩为零分。
As a team member, I fully understand and agree that: only one act of cheating or plagiarism by my team during the term will result in the whole team being disqualified from writing the final exam, and automatically assigned a grade of zero in the course.

答案：是
```

# 参考教程

## 如何看待浙江大学数据结构课上工高班串通舞弊的行为？

```
https://www.zhihu.com/question/68235594/answer/261395370
```
