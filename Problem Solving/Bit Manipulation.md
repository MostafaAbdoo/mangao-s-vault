& - | - ^ - ~
and - or - xor - not
**XOR**: same number > zero, Different number > one

- if C = A ^ B  then B = A ^ C and A = C ^ B
# Shifting
**Left**:
- Like Multiplying by 2
- <<
- 4<<1 = 8  = 4 * 2^1
**Right**:
- Like divided by 2


الفرق بين الرقم و الرقم اللي قبليه هو اول بيت منورة هتبقا مطفية و اللي قبلها كلهم هيبقوا منورين و الاند بتاعهم بيطفي البيت دي
- num = 10100
- num-1=10011
**Set Bit**: x | (1<<bit)
**Clear Bit**: x & ~(1<<bit-1)
**Change Bit**: x ^ (1<<bit-1)
**Check Bit**: (x>>bit-1)&1
or: x&(1<<bit-1)

