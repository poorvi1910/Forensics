I was looking up some forensics concepts and i came across this hashing which I think we can potentially use for a forensics challenege. I am not sure if this has been explored in nite before. 

### Perceptual Hashes (pHashes)
This family of algorithms are designed to not change much when an image undergoes minor modifications such as compression, color correction and brightness. 
pHashes allows the comparison of two images by looking at the number of different bits between the input and the image it is being compared against. This difference is known as the Hamming distance.
Cryptographic hashing algorithms like the MD5 or SHA256 are designed to generate an unpredictable result. In order to do this, they are optimized to change as much as possible for similar inputs. 
Perceptual Hashes are the opposite —they are optimized to change as little as possible for similar inputs.

They are heavily used in Forensics and Malware detecion / classification

![image](https://github.com/user-attachments/assets/f4e0896a-d8d7-4bc3-9042-aaee0aabbb6f)

A ctf challenge that used this concept : https://github.com/eternaleclipse/ctf-writeups/blob/main/PlaidCTF2022/I_C_U.md

The program checks if there is a valid PNG or JPEG magic header at the start of the file. The hashes of the images should be equal to ERsrE6nTHhI= (all the bits of the 2 files need to be equal)
The recognized text (case-insensitive) of the first file should be "sudo please" and of the second file should be "give me the flag" wherein Tesseract is used to extract English text from the images.



