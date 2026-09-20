# Coin-Detection-Using-OpenCV

## Name: Joshna.M
## Reg. No: 212225230118

## Aim
To detect and count the total number of coins in an image using thresholding, morphological operations, Blob Detection, and Contour Detection using Python and OpenCV.

## Requirements
1. Python
2. OpenCV
3. NumPy
4. Matplotlib
5. Jupyter Notebook
   
## Algorithm
1. Import the required Python libraries.
2. Read the input coin image.
3. Convert the image from BGR to grayscale.
4. Split the image into Blue, Green, and Red channels.
5. Apply binary inverse thresholding to separate the coins from the background.
6. Create a structuring element and apply morphological dilation.
7. Apply erosion to refine the detected regions.
8. Create an OpenCV SimpleBlobDetector with circularity, convexity, and inertia parameters.
9. Detect and count the coins using Blob Detection.
10. Detect the coin boundaries using Contour Detection.
11. Count the detected contours and compare the results of both methods.
12. Display the intermediate and final results.

**Step 1: Read Image**

The input coin image is read using OpenCV's imread() function.
```
image = cv2.imread("coin.jpg")
```
**Step 2: Convert Image to Grayscale**

The image is converted from BGR to grayscale.
```
imageGray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
```
**Observation:**
The grayscale image contains intensity information in a single channel, making it easier to perform thresholding and segmentation.

**Step 3: Split Image into R, G, B Channels**
```
imageB, imageG, imageR = cv2.split(image)
```
**Observation:**
The image is separated into Blue, Green, and Red channels. The grayscale image is used for further processing because it simplifies the segmentation process.

**Step 4: Thresholding**

Binary inverse thresholding with a threshold value of 120 is applied.
```
_, imageThreshold = cv2.threshold(
    imageGray, 120, 255, cv2.THRESH_BINARY_INV
)
```
**Observation:**
Thresholding converts the grayscale image into a binary image and separates the coin regions from the background.

Effect of threshold value

## Different threshold values can produce different results:

A lower threshold may include unwanted background regions.
A higher threshold may remove parts of the coins.
A suitable threshold provides clearer coin regions.
Step 5: Morphological Operations

A 5 × 5 kernel is initially used.

kernel = np.ones((5, 5), np.uint8)

**Dilation is applied:**
```
imageDilated = cv2.dilate(
    imageThreshold, kernel, iterations=1
)
```
**A second dilation is performed:**
```
imageDilated2 = cv2.dilate(
    imageDilated, kernel, iterations=1
)
```
**An elliptical structuring element is then used:**
```
kernel = cv2.getStructuringElement(
    cv2.MORPH_ELLIPSE, (5, 5)
)
```
**Finally, erosion is applied:**
```
imageEroded = cv2.erode(
    imageDilated2, kernel, iterations=1
)
```
**Observation:**
Dilation expands the foreground regions, while erosion reduces and refines the regions. The elliptical kernel is suitable for the approximately circular shape of coins.

**Step 6: Blob Detection**

A SimpleBlobDetector is created using parameters for:

Circularity
Convexity
Inertia

**The processed image is given to the detector:**
```
keypoints = detector.detect(imageEroded)
```
**The number of detected coins is obtained using:**
```
len(keypoints)
```
## Result:

Number of coins detected using Blob Detection: 9

## Step 7: Contour Detection

**Contours are detected using:**
```
contours, _ = cv2.findContours(
    imageEroded,
    cv2.RETR_EXTERNAL,
    cv2.CHAIN_APPROX_SIMPLE
)
```
Contours with an area greater than 500 are counted.

**Observation:**
Contour Detection identifies the boundaries of the connected coin regions in the processed binary image.

Comparison
Method	Detection principle	Number of coins
Blob Detection	Circularity, convexity and inertia	9
Contour Detection	Boundaries of connected regions	Use the number printed by your notebook

Important: Enter the actual Contour Detection number shown by your notebook rather than guessing it.

## Result

The coin image was successfully processed using grayscale conversion, thresholding, morphological operations, Blob Detection, and Contour Detection. 9 coins were detected using Blob Detection. The Contour Detection result was obtained from the processed image and compared with the Blob Detection result.

## Conclusion

Coin detection was successfully implemented using Python and OpenCV. Thresholding was used to separate the coin regions from the background, while morphological operations helped refine the detected regions. Blob Detection and Contour Detection were then used to identify the coins. The experiment demonstrates that proper thresholding and morphological preprocessing are important for obtaining clear coin-detection results.
