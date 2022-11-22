Huffman Encoding
================

Huffman Encoding;
Overloads a symbol

With huffman encoding, you have to choose the smalest size of the encoded word, the encoded word describes a unique item;

I never tested whether there's a difference;

// "11" becomes the keyword for "taking 3 bits"
//   00 3 // 2^2-1
//   01
//   10
//  110 1 // 1
// 1110 2 // 2
// 1111

// 2^(N)-1 == number of max possible bits;
// 2^1 == max number of 2nd stage bits;
// 2^0 == Every additional bit encoded ends up here;

Huffman table creation can be derrived from a huffman tree type;

Huffman tables can take many forms:
(Symbol count, Word Size\*SymbolCount, Words\*SymbolCount)
The table can describe a preset/pre-baked huffman symbol table;