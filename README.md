# 👱‍♂️Local-Binary-Patterns-Face-Recognition

<p float="left">
  <img src="output/lbp-faces/face-5.png" width="400" />
  <img src="output/lbp-faces/face-14.png" width="400" />
  <img src="output/lbp-faces/face-13.png" width="400" />
  <img src="output/lbp-faces/face-9.png" width="400" />
</p>

### 📄Description
---
One of the main problems in the computer vision field is face recognition. If we take a look back, we can find that building and deploying an accurate face recognition algorithm was very challenging and time-consuming. Since then, many face recognition algorithms have been made and implemented, such as: `Eigenfaces/Eigenvector`, `Local Binary Patterns` or `LBPs` in short, and even deep learning-based face recognition algorithms were introduced, such as `Siamese Networks` with triplet loss function. In this repository, our purpose is to implement not a state-of-the-art model but a fairly accurate face recognition algorithm based on the local binary patterns methodology.

### ⚡Face Recognition vs Face Detection
---
First of all, it has to be mentioned that `face detection` and `face recognition` are completely two different terminologies. With face detection, as its name suggests, we can detect and localize available face(s) in an image. The face detection algorithm tells you where is the face exactly in the image. On the other hand, a face recognition algorithm is a different algorithm. face recognizer gets the ROI of the image where the face is exactly located in that region, performs some actions on the ROI, and then identifies the person that this face belongs to.

We have various methods to detect and extract face(s) in the image, some of them are `Haar Cascades`, `OpenCV's deep learning-based face detector`, `HOG + Linear SVM` etc. We choose the `Haar Cascade` algorithm over other techniques because: 1. It has a very small size (< 1 MB). 2. it needs low computational power and  3. It is very fast (even if it can be used for real-time projects). This algorithm like many other algorithms, has some drawbacks. One of them is that it may (in some specific situations) produce some false positives. The other drawback is that this model has many parameters to tune. Therefore, you have to `finetune` its parameters to produce minimum false positives. The figure below shows one of the false positives that this model produced during the project.

<p align="center">
  <img width="400" src="output/lbp-faces/face-0.png">
</p>

### 🔄Local Binary Patterns Process
---
Local Binary Patterns, or `LBP` in short, is a texture descriptor. LBPs compute a local representation of texture. This local representation is constructed by comparing each pixel with its surrounding neighborhood of pixels. To implement the LBP text descriptor, first, we have to convert the input image into a grayscale image, then, for each pixel, we consider a neighborhood of size `r` surrounding the center pixel.

To calculate LBP for the neighborhood pixels, first, we have to consider their values and compare them to the center pixel value. If the intensity value of the neighborhood pixel is equal to or greater than the intensity value of the center pixel, then we set the neighborhood pixel value to `1`; Otherwise, we set it to `0`.

<p align="center">
  <img width="450" height="250" src="imgs/img-1.png">
</p>

To start, we can start by any neighborhood pixels. We can choose neighborhood pixels in `clock-wise` order or vice versa. Keep in mind that you have to keep the same order for every image ROI and every image in the dataset.

<p align="center">
  <img width="450" height="250" src="imgs/img-2.png">
</p>

After calculating the neighborhood pixel values in terms of the center pixel, we have to perform a binary test. The results of the binary tests will be saved in an 8-bit array(for the `3x3` neighborhood), and then we convert these array values into decimal format. After calculating the sum of decimals, we replace this value with the center pixel intensity value. After doing this process and computing the sum of decimals and replacing it with the center pixel for all of the pixels available in the image, we can get the LBP output image.

<p align="center">
  <img width="450" height="250" src="imgs/img-3.png">
</p>

The picture down below shows the final image after converting it into LBP format. The histogram next to it shows the intensity value distribution for all of the available pixels in the image.

<p align="center">
  <img width="250" height="350" src="output/extraction-results/extracted-face.png">
  <img src="output/extraction-results/image-hist.PNG" width="400" />
</p>

LBP algorithm, instead of looking into the entire image, first divides the whole image into a `SxS` grid. Then, this algorithm converts each grid into the `LBP` format and obtains the intensity distribution of that particular grid. After doing this process, this model produces an `S^2` histogram. After doing that, the model concatenates all of the produced histograms. Then we perform the `KNN` algorithm (with `K=1`), and by `chi-2` distance, we find the closest face in the dataset and display the corresponding person name in the output.

<p align="center">
  <img width="500" height="300" src="imgs/img-4.png">
</p>

### 📐Model Performance
---
This project was implemented by the built-in `cv2.face.LBPHFaceRecognizer_create()` OpenCV library. The `9x9` grid with the `radius` of 5 and `neighbors` of 16 were used. For this project, instead of training the model on random training and test examples, we do this five times (`cv=5`). By doing this, we reduce the `uncertainty score` of our models. In other words, by getting five different f1-scores and obtaining the mean scores, we know how this model is performing, instead of considering just one random f1-score.

The histogram below shows the distribution of 5 calculated f1-scores. 

<p align="center">
  <img width="500" height="300" src="output/model-performance/f-scores-dist.png">
</p>

### 💻Installation
---
The Code is written in Python 3.7.5. If you don't have Python installed, you can find it [here](https://www.python.org/downloads/). If you are using a lower version of Python, you can upgrade using the pip package, ensuring you have the latest version of pip. To install the required packages and libraries, run this command in the project directory after cloning the repository:
```
git clone git@github.com:Kasra1377/lbp-face-recognition.git
```
or
```
git clone https://github.com/Kasra1377/lbp-face-recognition.git
```
To install required libraries just type:
```
pip install -r requirements.txt
```

### ⚙Technologies Used
---
![](https://forthebadge.com/images/badges/made-with-python.svg)

[<img target="_blank" src="https://scikit-learn.org/stable/_static/scikit-learn-logo-small.png" width=200>](https://scikit-learn.org/stable/) [<img target="_blank" src="https://numpy.org/images/logos/numpy.svg" width=200>](https://numpy.org/) [<img target="_blank" src="https://upload.wikimedia.org/wikipedia/commons/thumb/5/53/OpenCV_Logo_with_text.png/487px-OpenCV_Logo_with_text.png" width=200>](https://docs.opencv.org/) 

### ❌Bugs & Issues
---
If you ever encounter any bugs or technical issues in this project you can report it to the `issues` section of this repository, or you can contact me by my email address.


### 👥Contributers
---
Kasra1377

### 🔻References
---
[Face Recognition with Local Binary Patterns (LBPs) and OpenCV](https://www.pyimagesearch.com/2021/05/03/face-recognition-with-local-binary-patterns-lbps-and-opencv/)

[Local Binary Patterns with Python & OpenCV](https://www.pyimagesearch.com/2015/12/07/local-binary-patterns-with-python-opencv/)

[What is face recognition?](https://www.pyimagesearch.com/2021/05/01/what-is-face-recognition/)

[Face Recognition: Understanding LBPH Algorithm](https://towardsdatascience.com/face-recognition-how-lbph-works-90ec258c3d6b)
