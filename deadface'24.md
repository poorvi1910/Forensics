## Tri Harder

On inspecting the email, there was whitespace under the signature block in each email

- NEW THING : They copy pasted whitespace and converted it to hexadecimal and you get 3 distinc uniicode charactes for whitespace
  U+200A, U+2002, and U+2009

 In the challenge base 3 numberng system wa smentioned. Hence replacing the unicode characters with 0,1 and 2 respectively, and then using a ternary
   to binary converter scipt and then converting the binary to plaintext, the flag is revealed

#### NOTE: Not adding a zero in the front didnt give the flag
