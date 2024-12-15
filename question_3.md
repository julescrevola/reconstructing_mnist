### Methods to Rebuild a Corrupted Dataset

#### 1. **Inverted Images**
   - **Solution:**
     - Apply color inversion using image processing libraries such as OpenCV or PIL in Python.
     - Color inversion flips pixel values to their opposite (e.g., black to white) to restore the original appearance.
     - Verify the image matches the expected dataset style after applying inversion.
   - **Implementation Example:**
     ```python
     from PIL import ImageOps
     inverted_image = ImageOps.invert(image)
     inverted_image.show()
     ```
- Soruce: https://stackoverflow.com/questions/2498875/how-to-invert-colors-of-image-with-pil-python-imaging  

#### 2. **Rubbish or Noisy Data**
   - **Solution:**
     - Identify the type and intensity of noise (Gaussian, salt-and-pepper, etc.).
     - Use noise reduction techniques:
       - **Gaussian Filtering**: Smoothens the image to reduce noise.
       - **Median Filtering**: Particularly effective against salt-and-pepper noise.
     - For heavy noise, use **denoising autoencoders** to reconstruct the clean image by learning the noise pattern.
   - **Implementation Example:**
     ```python
     import cv2
     denoised_image = cv2.GaussianBlur(image, (5, 5), 0)
     cv2.imshow('Denoised', denoised_image)
     ```
- Sources: https://stackoverflow.com/questions/62042172/how-to-remove-noise-in-image-opencv-python
https://stackoverflow.com/questions/66395494/how-to-reduce-the-noise-of-an-image-on-python
https://www.omdena.com/blog/denoising-autoencoders
#### 3. **Blurred Images**
   - **Solution:**
     - **Deblurring Techniques**:
       - **Wiener Filtering**: Estimate the original signal by minimizing the mean square error.
       Pros: Computationally efficient and easy to implement
       Cons:Requires precise knowledge of the blur kernel and noise characteristics, these are not always known like for this case.  
       - **Blind Deconvolution**: Estimate both the blur kernel and the deblurred image.
       Pros: Suitable when the blur kernel is unknown, making it flexible for real-world scenarios
       Cons:Computationally expensive and slow, particularly for high-resolution images  
       - **Deep Learning Models**: Use pre-trained models like DeblurGAN for complex cases.
     - Ensure the result preserves critical features of the original image without introducing
     artifacts.
     Pros: Can generalize well to various types of blur when trained on a large and diverse dataset
     Cons: Performance depends on the quality and diversity of the training data and expensive. 
   - **Implementation Example:**
     ```python
     import cv2
     kernel = cv2.getGaussianKernel(ksize=5, sigma=1)
     deblurred_image = cv2.filter2D(image, -1, kernel)
     cv2.imshow('Deblurred', deblurred_image)
     ```
- Sources: Call with AI expert from my company that explianed to me what all of this meant. His name is Mahmoud Bidry, PhD Student in AI.

#### 4. **Pixel Loss**
   - **Solution:**
     - **Interpolation Techniques**:
       - **Bilinear or Bicubic Interpolation**: Estimate missing pixels using neighboring ones.
       Pros: Quick and computationally efficient for small regions of missing pixels
       Cons: Limited effectiveness for large-scale pixel loss. 
     - **Inpainting Methods**:
       - Use neural networks designed for inpainting, such as DeepFill, to predict and fill missing regions.
       - Use OpenCV's `inpaint` function for small missing regions.
     - Validate the reconstructed image against the expected dataset pattern to avoid artifacts.
     Pros: Capable of restoring larger missing regions.
     Cons: May require training on a dataset similar to the corrupted one for optimal results, also very expensive. 
   - **Implementation Example:**
     ```python
     import cv2
     inpainted_image = cv2.inpaint(image, mask, inpaintRadius=3, flags=cv2.INPAINT_TELEA)
     cv2.imshow('Inpainted', inpainted_image)
     ```
- Source: https://www.youtube.com/watch?v=FAwA6LzFVFw&ab_channel=DSPNITAP
https://www.geeksforgeeks.org/python-opencv-bicubic-interpolation-for-resizing-image/
This amazing code on github: https://github.com/aGIToz/PyInpaint

Disclaimer: we used GPT to help with the synthesis of this markdown.