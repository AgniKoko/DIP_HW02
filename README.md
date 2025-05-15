# Digital Image Processing — 2nd Course Assignment  

![image](https://github.com/user-attachments/assets/9e1577f7-af71-44ef-be52-3ba219421a2f)


## Description
This repository hosts the Python implementation for the **second** laboratory assignment of the *Digital Image Processing* course.  
The goal is to build a **JPEG-style compressor** for a colour image of your choice:

1. **8 × 8 Block DCT & Quantisation** (per Y, Cb, Cr channel).  
2. **DC coefficient coding**  
   * Predict each block’s DC value from its causal neighbours.  
   * Test three predictors — **Left**, **Upper**, **Average(Left, Upper)** — and **select the one with the lowest residual entropy**.  
   * Huffman-encode the chosen residuals.
3. **AC coefficient coding**  
   * Zig-zag scan each block.  
   * Huffman-encode the resulting (RUNLENGTH, SIZE) symbols.
4. **Bit-budget accounting**  
   * Add up the encoded stream **plus** the Huffman tables themselves.  
   * Report the **overall compression ratio** against the raw 24-bit RGB image.

## Contents

```plaintext
├── data/
│   ├── lena.png                  # Example RGB test image (dimensions = multiples of 8)
│   └── uv_mapping.png            # Example test image with difficult edges (dimensions = multiples of 8)
├── DIP_HW02.ipynb                # Jupyter Notebook with the full pipeline of lena.png
├── DIP_HW02_uv_mapping.ipynb     # Jupyter Notebook with the full pipeline of uv_mapping.png
└── README.md
```

## Setup

### Requirements
- Python 3.9 or later
- pip

## Tasks

The assignment consists of the following steps, implemented sequentially in the Jupyter Notebook:

1. **Image I/O & Pre-processing**
   - Load `data/lena_color.png` (or any RGB image).
   - Verify dimensions are divisible by 8; otherwise, pad symmetrically.

2. **Forward DCT (8 × 8)**
   - Convert RGB → YCbCr (ITU-R BT.601).
   - Apply 2D DCT to each channel, block-wise.

3. **Quantisation**
   - Use standard JPEG luminance/chrominance tables or custom ones (justify choice).
   - Divide DCT coefficients by quant matrix and round to integers.

4. **DC Differential Coding**
   - For every block, compute residuals using Left, Upper, and Average predictors.
   - Compute entropy of each residual set, choose predictor with minimum entropy.
   - Huffman-encode the chosen residuals; store predictor indices for decoding.

5. **AC Run-length & Size Coding**
   - Zig-zag scan each block.
   - Generate (RUNLENGTH, SIZE) symbol histogram, build Huffman table.
   - Encode the AC stream.

6. **Bit-stream Packaging**
   - Concatenate in order:
     1. File header (image size, quant tables, predictor flags)
     2. Huffman DC table
     3. Huffman AC table
     4. Encoded DC residuals
     5. Encoded AC data
   - Save to `output/encoded.bin`.

7. **Decoding & Verification**
   - Decode bit-stream, reconstruct YCbCr blocks.
   - Apply inverse quantisation and inverse DCT.
   - Convert back to RGB.
   - Compute **PSNR** and **SSIM** against the original image.

8. **Compression-ratio Report**
   - Compute

   $$
     \mathrm{CR} = \frac{\text{Uncompressed size (bytes)}}%
                    {\text{Encoded bit-stream + tables (bytes)}}
   $$

   - Print the result and, if multiple quality factors were tested, save a *rate–distortion* plot.

## Running the Notebook
Open **DIP_HW02.ipynb** or **DIP_HW02_uv_mapping.ipynb** and execute all cells in sequence.

---

## Notes

This assignment is part of the *Digital Image Processing* course at **Democritus University of Thrace (DUTH)**.
The original task description is the intellectual property of the course instructor and is reproduced here strictly for educational purposes.
