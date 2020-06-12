# cyberstuff

Because I have no idea what I'm doing.

Shell is denoted by starting with `$ `, or `# ` if escalated.

### netcat

TCP by default (which is a good default); `-u`/`--udp` to change to UDP

Connect to IP `192.0.2.0` on port `12000`
```bash
$ nc 192.0.2.0 12000
```

Listen on port `12000`
```bash
$ nc -l -p port
```

### xxd

Convert binary to hex and conversely

Binary to hex; `-p` removes additional indexes and readable characters to leave only plain hexadecimal
(creates or replaces `file.hex` with the hexadecimal representation of `file.bin` contents)
```bash
$ xxd -p file.bin file.hex
```

Hex to binary
```
$ xxd -r file.hex file.bin
```

### Tools as useful or even more, but not worth explaining here:

* `file`: detect filetype
* `binwalk`: detect actual files contained in a given file and extract them
* `foremost`: same purpose than `binwalk`
* `7z`: extract various types of archives
* `exiftool`: gather information about a given file, depending of its type
* [`CyberChef`](https://gchq.github.io/CyberChef) helps with efficiently decoding stuff, with notably a tool, Magic, good at guessing

### RSA

Assymetric system

* `p` and `q` are two relatively big prime numbers
* `e` an integer, **almost always `65537`** as it's `0b10000000000000001` which is great for modular exponentiation

* `n` := `p × q`
* `phi` := `(p - 1)(q - 1)`
* `d` is the inverse of `e` (mod `phi`)

The public key is comprised of `n` and `e`
The private key is comprised of `n` and `d`

      The actual nature (definition) of the keys might vary ; but these are what each person needs to
      encrypt and decrypt:

If you name `P` the plain text of the message you intend to encrypt, and `C` its encrypted counterpart:

* `C` = `P`^`e` (mod `n`)
* `P` = `C`^`d` (mod `n`)

Summary / example in Python, using `gmpy2` which is a fast (for Python) library for some mathematical
operations (not supposed to be a valid *program*):

```python
import gmpy2

p = ...
q = ...

e = ...

n = p * q
phi = (p - 1) * (q - 1)

d = gmpy2.invert(e, phi)

C = pow(P, e, n) # modular exponentiation
P = pow(C, d, n)

```

### Base64

Base64 is commonly used to encode data in ASCII. It uses the alphabet in uppercase,
in lowercase, digits from `0` to `9`, and the `+` and `/` characters ; additionally it appends
`=` for padding (maintaining 24-bit alignment).

Base32 is sometimes encountered. Similarly, it only uses the uppercase alphabet and digits from
`2` to `7` ; and again the `=` for the same reasons (40-bit alignment).

A base64 character represents 6 bits ; A base32 character represents 5 bits.

### Automated tools for 'ancient crypto'

* [Pretty accurate tool to determine the cipher](https://bionsgadgets.appspot.com/gadget_forms/refscore_extended.html)
* [Other one, providing some auto-solvers](https://www.boxentriq.com/code-breaking/cipher-identifier)
* [Index of coincidence and probable key length](https://www.dcode.fr/index-coincidence)
* [Frequency analysis](https://www.dcode.fr/frequency-analysis)
* [Best tool to crack 'em (Windows only)](https://sites.google.com/site/cryptocrackprogram/)
* [Website for solving (or creating) by same author](http://www.cryptoprograms.com/)

Index of coincidence is helpful to determine the type of cipher:
if it is near the language's one, or higher, this means the letters from the text
are closed to those of a plain text written in this language, 
and thus it is probably a transposition cipher (which is in the end a permutation of the plaintext)
or a monoalphabetic substitution.

The further it is from this value the more likely it is to actually be a polyalphabetic cipher,
with more alphabets being used as the IC decreases.

The IC of English is `0.0667` (others are on the webpage). The same page uses this IC test
at periodic places to help determine the probable length of the key.

Frequency analysis is another helpful tool notably for substitution ciphers.

### Binaries

ELF is a linux binary, PE is a Windows binary

* `strings`: display all readable strings in a file
* `ltrace <file>`: show library calls done while executing the file.
* `strace <file>`: show system calls done while executing the file.
* `gdb`: debug binary
* [`peda`](https://github.com/longld/peda): might improve gdb friendliness, especially to show state, though it might be slow