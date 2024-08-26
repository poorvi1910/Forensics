A writeup on an old ctf chall which used parity bit of lsb to hide data.

The file given wasnt actually a jpeg
```
$ file hank.jpeg
hank.jpeg: PNG image data, 1600 x 1197, 8-bit/color RGBA, non-interlaced
```
Nothing stood out in the different colour channels with stegsolve
However, the solver noticed that the LSB of the alpha channel was noisy too, whereas the higher order bits were always set which indicated something was going on with lsbs.

First,they converted the png to a raw RGBA bitmap, for easier processing with python:
```
$ convert hank.jpeg raw.rgba
```
Since hint mentioned parity, they wrote this simple python script to calculate the parity bit of the LSBs for each pixel, and write them to a file:
```
data = open("raw.rgba", "rb").read()

bitcount = 0
bits = 0
out = []

for i in range(0, len(data), 4):
    parity = data[i] ^ data[i+1] ^ data[i+2] ^ data[i+3]
    bits |= (parity&1) << bitcount
    bitcount += 1
    if bitcount == 8:
        out.append(bits)
        bitcount = 0
        bits = 0

f = open("output", "wb")
f.write(bytes(out))
f.close()

```
This resulted in a zip, file, which they extracted:
```
$ file output
output: Zip archive data, at least v2.0 to extract
$ unzip output
Archive:  output
  inflating: mod.iqm
```
Tool for viewing IQM files: http://www.moddb.com/mods/r-reinhard/addons/iqebrowser-v110
And opening it the flag was there
