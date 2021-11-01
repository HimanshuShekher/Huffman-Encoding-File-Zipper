# Web App link
**https://huffman-encoding-file-zipper.herokuapp.com/**


# About

Huffman Algorithm is an efficient way for file Compression and Decompression. This program exactly follows huffman algorithm. It reads frequent characters from input file and replace it with shorter binary codeword. The original file can be produced again without loosing any bit.

# Usage

Compression:

    ./encode <file to compress>

Output file named .hzip will be produced. Decompression:

    ./decode <file to uncompress>


The main logic for the program is in ./huffman.js

The huffman algorithm encodes the input as a string of bits. The script included takes a file as input and writes the encoded(compressed) result to a file as well. We cannot write individual bits to a file using node. It would also be unwise to write the sequence of 1's and 0's as characters to a file because each number would be represented by a byte on disk which would inflate the file size rather than compress it. We will have to use a different representation to store the encoded sequence.

The code included takes each sequence of 8 bits (a byte) and turns it into a decimal number. The number is then used to create an extended ascii character (0-255) using String.fromCharCode. When we later decode the sequence we can perform this process in reverse to arrive at the original string of bits.

Consider a case where the first 8 bits of the encoded string was 0011 1001

turn 0011 1001 to a number with parseInt -> 57
turn 57 into a char with String.fromCharCode -> 9
store char 9 in the file
At the end of the sequence we make sure the left over bits become a byte by right padding with 0's. So if we had 3 bits left over we would add 5 0's to the end of the sequence. This will be handled accordingly when we decode.

When we decode the file we go in the opposite direction for each char (byte)

char to character code 9 -> 57
character code to binary 57 -> 111001
left pad until we have 8 bits -> 00111001
use the sequence to walk the huffman tree
Since we may have extra zeros from the right pad in the encoding we make sure we only decode the same number of characters we had in the input originally. Essentially we stop when we have the correct number of characters.

In order to reconstruct the same huffman tree when we decode the minified text we store the frequency table and length of the original input. We can parse this when we read the encoded file and then proceed with the decoding process described above.
# Huffman-Encoding-File-Zipper
