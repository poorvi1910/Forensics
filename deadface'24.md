## Tri Harder

On inspecting the email, there was whitespace under the signature block in each email

- NEW THING : They copy pasted whitespace and converted it to hexadecimal and you get 3 distinc uniicode charactes for whitespace
  U+200A, U+2002, and U+2009

 In the challenge base 3 numberng system wa smentioned. Hence replacing the unicode characters with 0,1 and 2 respectively, and then using a ternary
   to binary converter scipt and then converting the binary to plaintext, the flag is revealed

#### NOTE: Not adding a zero in the front didnt give the flag

## Descnded from wolves

In this chall writep they mentioned that if the preceding bytes in the chunk change, so too does the CRC(Cyclic Redundancy Check) need to change. Hence when they are changing the height the crc must change too. Never came across this fact before for some reason.

Also python has a library ofr it
```
import zlib

ihdr_data = bytes.fromhex('4948445200000400000005000803000000')
crc = zlib.crc32(ihdr_data) & 0xffffffff
print(f'{crc:08X}')
```
This script does it where bytes.fromhex functiontakes the entire IHDR. Note that this IHDR includes the new 00 00 05 00 height.
