`file` command to check the type of a file

# Binwalk
**Binwalk** can be used to extract files hidden within other files, including text files hidden in images or other types of steganographic embeddings

`binwalk image.png`
- This will list all detected embedded files and their offsets.
`binwalk -e image.png`
- This creates a directory (e.g., `_image.png.extracted`) containing the extracted hidden files.
`binwalk -Me image.png`
-   This extracts all layers of hidden files, ensuring deeply nested files are also uncovered
`strings image.png`
- Use `strings` to search for readable text

- If the hidden content is steganographically encoded (e.g., bits embedded within image pixels), Binwalk might not detect it.
- Tools like **Steghide**, **zsteg**, or specialized steganography tools are better suited for these cases.

# Steghide
tool used to **hide or extract data within files** like images, audio files, or other multimedia. It works by embedding the data in a way that is imperceptible to human senses, making it a popular choice for securely concealing information.
1. **Supports Multiple File Formats**:
    - Works with common media formats, such as:
        - Images: JPEG, BMP
        - Audio: WAV, AU
2. **Encryption and Compression**:
    - Encrypts hidden data using strong encryption algorithms (e.g., AES-128) to secure its contents.
    - Compresses data before embedding to minimize the size impact.
3. **Passphrase Protection**:
    - Requires a passphrase to extract hidden data, ensuring an additional layer of security.
4. **Preserves File Integrity**:
    - Maintains the original quality and appearance of the carrier file (image/audio).
5. **Metadata Hiding**:
    - Does not leave obvious traces in the file's metadata that could indicate hidden data.

Steghide uses **least significant bit (LSB)** steganography to embed data into the carrier file. For example:
- In images, it modifies the least significant bits of pixel values.
- In audio, it modifies the least significant bits of sound waveforms.

**Embed Data in a File**:
`steghide embed -cf carrier_file -ef data_file`
- You’ll be prompted for a passphrase. Enter a strong, secure passphrase.
**Extract Hidden Data**:
`steghide extract -sf carrier_file`
**Check File Information**:
`steghide info carrier_file`
**Specify Encryption Algorithm**:
`steghide embed -cf image.jpg -ef secret.txt -e rijndael-128`
**Inspect Metadata**: Use `exiftool` to analyze file metadata:
`exiftool <file_name>`
**Check for Strings**: Look for readable content:
`strings <file_name>`
**Check File Type**:
`file <file_name>`
**Check if Data is Embedded**:
`steghide info <file_name>`
- This will tell you if hidden data exists and whether a passphrase is required.
`stegcracker image.jpg
`stegcracker image.jpg custom_wordlist.txt
![[Pasted image 20241225181635.png]]

