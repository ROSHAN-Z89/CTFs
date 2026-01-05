First Downloaded the image scanned it through exiftool 
Found a comment : "c3RlZ2hpZGU6Y0VGNmVuZHZjbVE9"
Used cyebrchef.io and decoded the cipher using Base64 decoder: "steghide:cEF6endvcmQ="
again decoded "cEF6endvcmQ=" and got: "pAzzword"

steghide is a tool to extract data from images.
Used cmd: "steghide extract -sf img.jpg" and then asked from passphrase: pAzzword
Then flag.txt was created under the same folder 
"cat flag.txt" : "picoCTF{h1dd3n_1n_1m4g3_5d4cba73}"
