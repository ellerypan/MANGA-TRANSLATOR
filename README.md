# Manga_Translator (Work in progress)

This is the model pipeline in our self-designed application--"Mlator", which is aimed to help manga fans and publishers to overcome the language barrie and lower the cost of translation respectively. 

# Workflow
This example image is from <<Q.E.D. iff - proven end - 11>> Episode 1 © Motohiro Katou.

## Bubble Detection
First, train a object detection model that helps us locate the text in the bubble. Here we thanks to ** Manga109** providing us with large amount of high quality annotated dataset.As the following image shows, the identified areas are marked with orange bounding boxes, and content in the box would be processed by the next step.

<img width="747" alt="Screen Shot 2019-05-07 at 12 54 51 AM" src="https://user-images.githubusercontent.com/40588854/57282875-cffbc380-7062-11e9-9e65-bf984f1c04b9.png">

## Optical Character Recognition
Next we use a state-of-the-art OCR engine to parse the image segment we identified in step 1 into machine-readable text.
Besides, a few tricks are needed to help the model parse vertically-oriented Japanese text and stylized comic fonts.

<img width="742" alt="Screen Shot 2019-05-07 at 12 54 16 AM" src="https://user-images.githubusercontent.com/40588854/57282885-d2f6b400-7062-11e9-98b3-2f043ee2ea7d.png">

## Translation
All the extracted Japanese text is translated to English. This is a crucial stage in the process, since a quality translation is what allows readers to enjoy the results.

<img width="771" alt="Screen Shot 2019-05-07 at 12 54 28 AM" src="https://user-images.githubusercontent.com/40588854/57282898-d5f1a480-7062-11e9-80e8-f73e9279bf12.png">

## Text Removal
If we simply use the bounding boxes as our translated text background, some of the boxes would leak beyond the bounds of the bubble, which make the page uncomfortable to read. It would be the best if the bubble is used for background, that's why we need to remove the original text.

<img width="753" alt="Screen Shot 2019-05-07 at 12 54 44 AM" src="https://user-images.githubusercontent.com/40588854/57282911-df7b0c80-7062-11e9-94e7-28edf532c8cc.png">

## Placement
Finally, the English text is broken up into lines of an appropriate length and resized to comfortably fit their corresponding speech bubble. At this point, the comics are translated and ready for reading!

<img width="749" alt="Screen Shot 2019-05-07 at 12 54 04 AM" src="https://user-images.githubusercontent.com/40588854/57282941-eb66ce80-7062-11e9-8d33-ca6b75878914.png">

