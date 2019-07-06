# Manga Translator

This is the model pipeline in our self-designed application--"Mlator", which aims to help manga fans and publishers to overcome the language barrier and lower the cost of translation respectively. 

## Website
http://mlator.com/

**Two Main Functions:**

I) Drag your page to the DEMO region at the bottom and the result will show on the right side

<p align="center">
<img width="600" alt="Screen Shot 2019-07-05 at 6 45 54 PM" src="https://user-images.githubusercontent.com/40588854/60749857-27c88500-9f55-11e9-9a24-08dc8fe04169.png">
</p>

II) After you register and log in, you can translate multiple pages and download the translations at the same time

<p align="center">
<img width="600" alt="Screen Shot 2019-07-05 at 6 43 44 PM" src="https://user-images.githubusercontent.com/40588854/60749852-09628980-9f55-11e9-8c1c-1ddeb4c24ed9.png">
</p>

## Source Code
https://github.com/MSDS698/product-analytics-group7

## Demo
https://youtu.be/SuyPbsMslxs

## Workflow
This example image is from <<Q.E.D.iff-proven end-11>> Episode 1 © Motohiro Katou.

### Bubble Detection
First, train an object detection model that helps us locate the text in the bubble. Here thanks to **Manga109** providing us with a large amount of high quality annotated dataset. As the following image shows, the identified areas are marked with orange bounding boxes, and content in the box would be processed by the next step.

<p align="center">
<img width="500" alt="Screen Shot 2019-05-07 at 12 54 04 AM" src="https://user-images.githubusercontent.com/40588854/57282941-eb66ce80-7062-11e9-8d33-ca6b75878914.png">
</p>

### Optical Character Recognition
Next, we use a state-of-the-art OCR engine to parse the image segment we identified in step 1 into machine-readable text.
Besides, a few tricks are needed to help the model parse vertically-oriented Japanese text and stylized comic fonts.

<p align="center">
<img width="500" alt="Screen Shot 2019-05-07 at 12 54 16 AM" src="https://user-images.githubusercontent.com/40588854/57282885-d2f6b400-7062-11e9-98b3-2f043ee2ea7d.png">
</p>

### Translation
All the extracted Japanese text is translated into English. This is a crucial stage in the process, since a quality translation is what allows readers to enjoy the results.

<p align="center">
<img width="500" alt="Screen Shot 2019-05-07 at 12 54 28 AM" src="https://user-images.githubusercontent.com/40588854/57282898-d5f1a480-7062-11e9-80e8-f73e9279bf12.png">
</p>

### Text Removal
If we simply use the bounding boxes as our translated text background, some of the boxes would leak beyond the bounds of the bubble, which make the page uncomfortable to read. It would be best if the bubble is used for background, that's why we need to remove the original text.

<p align="center">
<img width="500" alt="Screen Shot 2019-05-07 at 12 54 44 AM" src="https://user-images.githubusercontent.com/40588854/57282911-df7b0c80-7062-11e9-94e7-28edf532c8cc.png">
</p>

### Placement
Finally, the English text is broken up into lines of an appropriate length and resized to comfortably fit their corresponding speech bubble. At this point, the comics are translated and ready for reading!

<p align="center">
<img width="500" alt="Screen Shot 2019-05-07 at 12 54 51 AM" src="https://user-images.githubusercontent.com/40588854/57282875-cffbc380-7062-11e9-9e65-bf984f1c04b9.png">
</p>

## Deploy on Website


**Instance Type:** Ubuntu Server 18.04 LTS (HVM), SSD Volume Type

**Size:** >= t2.medium

**Models:** https://drive.google.com/drive/folders/1mEvrweffTBs7-wb2WyNQ8wOjoTKVWxCT?usp=sharing

**Command Lines:**


1. install conda
```
wget https://repo.continuum.io/archive/Anaconda3-4.2.0-Linux-x86_64.sh
bash Anaconda3-4.2.0-Linux-x86_64.sh
export PATH=/home/ubuntu/anaconda3/bin:$PATH
conda update coda
```

2. install git and git clone the files
```
sudo yum install git
git config —global credential.helper store
git clone https://github.com/MSDS698/product-analytics-group7.git
```

3. setup environment
```
cd product-analytics-group7
conda env create -f environment.yml
source activate MSDS603
```

4. setup packages
```
pip install scikit-image
pip install opencv-python
sudo apt update && sudo apt install -y libsm6 libxext6
pip install pytesseract
pip install —upgrade google-cloud-translate
pip install Keras==2.1.4
pip install tensorflow
```

5. setup tesseract
```
sudo apt-get update
sudo apt install tesseract-ocr
sudo apt install libtesseract-dev
sudo apt install tesseract-ocr-chi-tra-vert
sudo apt install tesseract-ocr-jpn-vert
```
6. transfer well-trained models: SSD, U-Net and OCR to certain paths
```
cd product-analytics-group7/server/
mkdir checkpoint
scp -i your.pem ssd300_all.h5 ubuntu@ec2-xx-xxx-x-xxx.us-west-2.compute.amazonaws.com:product-analytics-group7/server/checkpoint/

scp -i your.pem unet_8.hdf5 ubuntu@ec2-xx-xxx-x-xxx.us-west-2.compute.amazonaws.com:product-analytics-group7/server/checkpoint/

# if permission denied when you run the following cmd, scp to the Desktop then sudo mv
scp -i your.pem jpn_vbest.traineddata ubuntu@ec2-xx-xxx-x-xxx.us-west-2.compute.amazonaws.com:/usr/share/tesseract-ocr/4.00/tessdata
```

7. run the server
```
cd ~
python product-analytics-group7/server/server.py
```
