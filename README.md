## 4D Chaotic Memristive Model: Image, Audio and Video Encryption and Decryption

Image, audio and video encryption and decryption using the 4D memristive chaotic system with a hyperbolic tangent nonlinear term. The implementation uses fourth-order Runge–Kutta (RK4, \(\delta=1\)) to generate chaotic sequences, followed by permutation and two-pass diffusion for multimedia encryption and decryption.

**Platform:** Python · Google Colab · Jupyter Notebook

## How to Run

1. Open the required `.ipynb` notebook in Google Colab (image, audio, video, or the combined 12-tests notebook).
2. Select **Runtime → Run all**.
3. Upload the matching test file when prompted (RGB image, PCM WAV audio, or video).
4. Run the encryption and decryption cells.
5. View the original, encrypted and decrypted outputs, recovery verification, histograms and statistical measurements.

For the video experiment, use the complete-video configuration to process the original video without resizing, frame dropping or duration trimming. The full test video contains 385 frames at 1920 × 1080 resolution and 30 FPS, with its complete stereo audio track.

The video implementation processes frames sequentially to control memory usage. The encrypted video is saved in a lossless MKV container, and decryption is performed from the saved encrypted video.

## Repository Contents

| File | Purpose |
|---|---|
| `4D_Chaotic_Model_Image_Encryption_Decryption.ipynb` | Image encryption and decryption |
| `4D_Chaotic_Model_Audio_Encryption_Decryption.ipynb` | Audio (PCM WAV) encryption and decryption |
| `4D_Chaotic_Model_Video_Encryption_Decryption.ipynb` | Complete video frame and audio encryption and decryption |
| `4D_Chaotic_Model_12_Tests_Image_Audio_Video.ipynb` | 12 statistical tests for each modality |
| `reports/` | Image, audio, video and combined statistical reports |
| `Standard Testing Data/` | Sample image, audio and video used in experiments |

## 4D Chaotic Model

The system consists of four state variables:

φ1,φ2,φ3,φ4

The equations used in the implementation are:

```text
dφ1/dt = -p(φ1 - φ2) + φ1φ2φ3
         + k(α + β tanh(φ4))φ2

dφ2/dt = q(φ1 + φ2) - φ1φ3²

dφ3/dt = -rφ3 + sφ2 + φ1φ2φ3

dφ4/dt = φ2
```

### Model Parameters

| Parameter | Value |
|---|---:|
| p | 43 |
| q | 13 |
| r | 13 |
| s | 6 |
| k | 1 |
| α | 1 |
| β | 0.5 |
| Initial state | [1, 1, 1, 1] |
| Numerical method | RK4 |
| Step size (h) | 0.0001 |
| Fractional order | δ = 1 (classical case) |

The fourth state variable, φ4, is used in the hyperbolic tangent memristive term. It adds nonlinear feedback to the system.

## Encryption and Decryption Process

The 4D chaotic system generates sequences that are used to control the multimedia encryption process.

- **Image:** Pixel permutation followed by forward and backward diffusion of image data.
- **Audio:** Encryption of PCM WAV data using chaotic sequences, byte permutation and diffusion.
- **Video:** Individual RGB frame encryption combined with separate audio encryption. Frames are processed sequentially, and the chaotic encryption process is applied to each frame.

Decryption reverses the diffusion and permutation operations using the correct key and corresponding initialization parameters.

SHA-256 hashes are used to verify exact recovery of the original decoded image, audio or video data.

## Statistical Testing

The combined testing notebook contains 12 tests for each modality:

1. Information entropy
2. Adjacent-pixel/sample correlation
3. Number of Pixels Change Rate (NPCR)
4. Unified Average Changing Intensity (UACI)
5. Time-series boundedness
6. Phase portraits
7. Lyapunov exponent analysis
8. Initial-condition sensitivity
9. Histogram chi-square test
10. Wrong-key sensitivity
11. Effective key-space assessment
12. Encryption/decryption execution time and throughput

The tests evaluate encryption behaviour, statistical properties, sensitivity and recovery performance for image, audio and video.

## Important Notes

- The implementation uses the 4D chaotic system from the supplied research article. The multimedia permutation and diffusion algorithms are project-developed applications of the model, not encryption algorithms directly provided by the article.
- The paper contains a sign inconsistency in its printed first differential equation and its subsequent reported stability results. The implementation uses the sign convention consistent with the paper's reported eigenvalues and bounded dynamics. This distinction should be considered when comparing the code with the printed equation.
- The numerical solver is classical fourth-order Runge–Kutta (RK4), corresponding to the integer-order case δ = 1. The paper's fractional-order Caputo, Caputo–Fabrizio and Atangana–Baleanu schemes are not implemented.
- Audio and video encryption are extensions of the 4D chaotic model.
- Complete-video testing preserves all 385 original frames at 1920 × 1080 and 30 FPS, together with the complete audio track. Exact recovery refers to decoded RGB frames and decoded PCM audio samples, not the original compressed MP4 file bytes.
- Statistical test results must be interpreted individually. High entropy, low correlation and exact recovery do not independently establish cryptographic security.
- This project is an experimental research prototype and is not recommended for protecting confidential or sensitive data.
