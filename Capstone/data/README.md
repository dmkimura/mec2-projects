# Capstone Data

## Dataset Background Information

The *image.tar* and *annotation.tar* files were downloaded from the **Standford Dogs Dataset** located at [vision.stanford.edu/aditya86/ImageNetDogs/](http://vision.stanford.edu/aditya86/ImageNetDogs/). This dataset is relevant to my project because I am designing a model to track the moving of dogs. Although I will be using a pre-trained model for this problem, the pre-trained model was designed to classify among 80 different objects. These images will be used to train the model on the specific task of identifying and tracking dogs.

The *image.tar* file contains 20,580 images of dogs across 120 different dog breeds. 

The *annotation.tar* file provides both the labeled breed of the dog as well as its bounding box.

## Choosing a Dataset

To find a relevant dataset, I chose to look for datasets in [kaggle.com](kaggle.com). I simply searched for `dog` in the datasets section of the website.

There were several datasets to review, so I narrowed the selection down to three datasets based on number of images (greater than 10,000).

### Kaggle: Cat vs Dog Dataset
* **Url location:** [kaggle.com/datasets/karakaggle/kaggle-cat-vs-dog-dataset](https://www.kaggle.com/datasets/karakaggle/kaggle-cat-vs-dog-dataset)
* **Description:** This dataset uses data originally generated through a collaboration between Petfinder.com and Microsoft. The PetImages folder of the dataset contains 24,961 total images, divided between two folders: `Cat` and `Dog`. The `Dog` folder contains 12,470 images. The dataset also contains a file named `MSR-LA - 3467.docx`. The Kaggle project page did not provide a description or preview of the docx file, so the contents are not known. This dataset was given a usability score of 6.3 on Kaggle.
* **Decision:** Not used. Another dataset has more dog images across more breeds and includes bounding box information. 

### Cat & Dog Images for Classification
* **Url location:** [kaggle.com/datasets/ashfakyeafi/cat-dog-images-for-classification/data](https://www.kaggle.com/datasets/ashfakyeafi/cat-dog-images-for-classification/data)
* **Description:** This dataset contains a single folder for all image files and a csv file for classification labels. There are a total of 25,000 images in this dataset, divided equally between 12,500 images of dogs (file names starting with `dog`) and 12,500 images of cats (file names starting with `cat`). This dataset was given a usability score of 10.0.
* **Decision:** Not used. Another dataset has more dog images across more breeds and includes bounding box information.

### Stanford Dogs Dataset
* **Url location:** [kaggle.com/datasets/jessicali9530/stanford-dogs-dataset](https://www.kaggle.com/datasets/jessicali9530/stanford-dogs-dataset)
* **Description:** This dataset contains an image folder and an annotation folder. The entire image folder consists of dog images: 20,580 files covering 120 different dog breeds. The annotation files contain both the labeled breed of the dog and the bounding box information.
* **Decision:** Used. Dataset contains an exhaustive amount of dog data: large amount of images, wide variety of dog breeds, bounding box information in case it's needed. Since the dataset originated from Stanford University, the dataset files were downloaded from the Stanford University url noted at the top of this page.
