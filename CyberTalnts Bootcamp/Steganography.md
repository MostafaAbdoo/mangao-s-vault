# What is Steganography?

Steganography is the art and science of embedding secret messages in a cover message in such a way that no one, apart from the sender and intended recipient, suspects the existence of the message.

The diagram below represents a basic steganographic model.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXf_GBRWpSwxNVJM-NMsHeaW_PN_4pHHdutS_-j1gVQN82y80KSbKMmHoHvz_aUCs6oT0_A3QzLFywwo9KYVonJ5F6RFZvGElHLTYQxhLbNsF5b7dxSgVXrLVvZOWBcAkTQqtk9hVwS15g2h63izsEYr-ZdI?key=2yDNR8q3UA90YBe4RkQCyg)

# How is Steganography Different from Cryptography?

Both of them have almost the same aim which is to protect a third-party message or information. They do, however, use a completely different mechanism to protect the information.

Cryptography changes the ciphertext information which can not be understood without a decryption key. And if someone intercepted this encrypted message they could easily see that some sort of encryption had been applied. 

Steganography, on the other hand, does not alter the information format so it conceals the message's existence.

# Steganography Techniques

- Image Steganography: Hiding information within digital images by manipulating the pixel values in a way that is imperceptible to the human eye.
- Audio Steganography: Embedding secret messages within audio files by altering the audio signals in a manner that remains inaudible.
- Video Steganography: Concealing data within video files by modifying the video frames or embedding data in the video stream.
- Text or File Steganography: Hiding information within texts or files by using techniques such as invisible ink, changing the formatting, or manipulating the font size and style.

## Image Steganography

Hiding the data is known as image steganography, by taking the cover object as the background. In digital steganography, images are widely used as a cover source, because the digital representation of an image contains a huge number of bits. 

There are several ways of hiding the data within an image. Popular approaches involve:

- Least Significant Bit Insertion
- Masking and Filtering
- Redundant Pattern Encoding
- Encrypt and Scatter

## Audio Steganography 

In audio steganography, the secret message is embedded into an audio signal which alters the binary sequence of the corresponding audio file. Hiding secret messages in digital sound is a much more difficult process when compared to others, such as Image Steganography. Different methods of audio steganography include:

- Least Significant Bit Encoding
- Parity Encoding
- Phase Coding
- Spread Spectrum

## File Steganography

File steganography is an old technique for embedding images or text files into another format.

binwalk: is great for checking out if other files are embedded or appended to a file.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfR5YdtN7IHzbB8FoM-1Oh1BQsJxh0GtZXXinFhrhi7x2dtTiFF6wCrMVg7mzZzgXEW-YAnBi_hRWKPcWFsmwE3y24mDE3i1jyI4oFwgrvGH_UnETnzpoY61F0OmMWoykOhLfcSBNxH0CpV_mv71cZT_x9_?key=2yDNR8q3UA90YBe4RkQCyg)

# Best Tools to Perform Steganography

There are many software available that offer steganography. Some offer normal steganography, but a few offer encryption before hiding the data. 

These are the steganography tools that are available for free:

- **Stegsolve:** Interactively transforms images and views color schemes separately.
- **SonicVisualiser:** Visualizing audio files in the waveform, display spectrograms
- **DeepSound:** Audio stego tool that was used in Mr. Robot series. Windows tool but could be run in wine
- **Steghide:** tool to encrypt and hide data.