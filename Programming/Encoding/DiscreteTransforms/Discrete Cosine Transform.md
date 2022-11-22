Discrete Cosine Transform
---
Discrete Cosine Transform let's one represent the frequency with which an information change occurs.
DCT used in jpeg compression describes a frequency of alternating vertical and horizontal information.

Together with an coefficient it can further describe other properties of the image such as color, and lightness, the DCT of the different frequency domains describes the image.

Because the information change is more frequent "horizontally" or "vertically", by moving vertically or horizontally within the DCT coefficients table.
The lower right corner describes the highest frequency information.

Through quantization, we are reducing the need for the high frequency information, the most freuqunt information is in the corners of the DCT table, we of course need to compensate a bit for the information lost, hence we don't just zero out the coefficients of high frequency information without adding onto lesser frequency information.

Through zigzagging we are ensuring that the information in the end can be encoded with an RLE of how many zeros are at the end.

---