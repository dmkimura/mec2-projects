# Capstone Data

## Dataset Background Information

* *image.tar file* - this file contains 10,555 images of dogs across 120 breeds. It came from the larger  **Standford Dogs Dataset** located at [vision.stanford.edu/aditya86/ImageNetDogs/](http://vision.stanford.edu/aditya86/ImageNetDogs/). The original folder contained 20,580 images of dogs. To ensure enough space is available in LFS, the number of images per breed was reduced by approximately half. (Code to reduce the number of files is below.)
    * This dataset is relevant to my project because I am designing a model to track the movement of dogs. Although I will be using a pre-trained model for this problem, the pre-trained model was designed to detect 80 different classes of objects. By conducting transfer learning with this dataset and the below data of non-dog images, the model will learn the specific task of identifying and tracking dogs.

* *imagenet_mini.tar file* - this file contains 11,846 images across 858 classes of objects, not including dogs. It came from the larger **ImageNet 1000 (mini)** dataset located at [kaggle.com/datasets/ifigotin/imagenetmini-1000/data](https://www.kaggle.com/datasets/ifigotin/imagenetmini-1000/data). The original dataset on Kaggle is a reduced version of the ImageNet object detection images; it took a small number of files across 1000 classes of objects, to includes dogs. It consisted of two image folders, one for training images and one for validation images. The total number of images in the training folder totaled 36,744 images and the validation folder contained 5,922 images. To ensure enough space is available in LFS, several steps were taken to create a smaller dataset of images that do not include dogs. The validation images were removed, all dog images were removed from the train directory, and then approximately two thirds of the remaining files were removed. (Code to reduce the number of files is below.)
 

## Choosing a Dataset

To find a relevant dataset, I chose to look for datasets in [kaggle.com](kaggle.com). I searched for `dog` in the datasets section to find relevant datasets containing dog images, and I searched for `imagenet` to find a version of the ImageNet dataset that is smaller than the full dataset.

There were several datasets to review, so I narrowed the selection of the dog datasets down to two options.

### Cat & Dog Images for Classification
* **Url location:** [kaggle.com/datasets/ashfakyeafi/cat-dog-images-for-classification/data](https://www.kaggle.com/datasets/ashfakyeafi/cat-dog-images-for-classification/data)
* **Description:** This dataset contains a single folder for all image files and a csv file for classification labels. There are a total of 25,000 images in this dataset, divided equally between 12,500 images of dogs (file names starting with `dog`) and 12,500 images of cats (file names starting with `cat`). This dataset was given a usability score of 10.0.
* **Decision:** Not used. Another dataset has more dog images across more breeds and includes bounding box information.

### Stanford Dogs Dataset
* **Url location:** [kaggle.com/datasets/jessicali9530/stanford-dogs-dataset](https://www.kaggle.com/datasets/jessicali9530/stanford-dogs-dataset)
* **Description:** This dataset contains an image folder and an annotation folder. The entire image folder consists of dog images: 20,580 files covering 120 different dog breeds. The annotation files contain both the labeled breed of the dog and the bounding box information.
* **Decision:** Used. Dataset contains an exhaustive amount of dog data: large amount of images, wide variety of dog breeds, bounding box information in case it's needed. Since the dataset originated from Stanford University, the dataset files were downloaded from the Stanford University url noted at the top of this page.

### ImageNet 1000 (mini)
* **Url location:** [kaggle.com/datasets/ifigotin/imagenetmini-1000/data](https://www.kaggle.com/datasets/ifigotin/imagenetmini-1000/data)
* **Description:** This dataset contains 37,121 images of 1000 object classes, to include dogs. The dataset is divided into two image folders, one for training images and one for validation images.
* **Decision:** Used. Dataset contains a large variety of images outside of dogs.

## Code to Reduce Original Datasets

The below code blocks are performed on a Linux machine, so it is ran with Bash.

### Reduce Number of Files in `images.tar` by Half
```bash

# make directory for processing data
mkdir test; cd test

# view number of files in tar'd file; should be 20,701
tar -tf images.tar | wc -l

# untar images file
tar -xvf images.tar

# view number of files untar'd; should be 20,819
ls Images/* | wc -l

# remove half of the files in each folder of Images directory
for dir in Images/*; do
    # Get the number of files in the directory
    num_files=$(find "$dir" -maxdepth 1 -type f | wc -l)
    # Calculate half of the number of files (integer division)
    half_files=$((num_files / 2))

    # Delete the first half of the files
    if [ $half_files -gt 0 ]; then
        find "$dir" -maxdepth 1 -type f | head -n "$half_files" | xargs rm
    fi
done

# verify number of files ~ half of original count; should be 10,555
ls Images/* | wc -l

# tar remaining images file
tar -cvf images.tar Images

# verify tar'd file count is reduce by ~ half; should be 10,437
tar -tf images.tar | wc -l

# remove Images folder
rm -rf Images

# mv images.tar to git Capstone data folder

```

### Reduce Number of Files in `imagenet_mini.tar` by Two Thirds
```bash

# imagenet mini contains 1000 classes of images, to include dogs
# will remove dogs from the training set of data (n02085620 through n02120079)
# then see how many files still remain to remove

# view number of files in zipped archive; should be 38,673
unzip -l archive.zip | wc -l

# unzip imagenet zip file
unzip archive.zip

# remove the validation set of files
rm -rf imagenet-mini/val

# see number of folders in train set; should be 1000
ls imagenet-mini/train | wc -l

# see number of files in train set; should be 36,744
ls imagenet-mini/train/* | wc -l

# remove folders related to dogs in the train folder

# remove folders beginning with n0208 and n0209
# run find without -exec first to ensure deleting correct files
for i in $(seq 8 9); do
    find . -type d -name "n020${i}[0-9]*"
done
# command with -exec
for i in $(seq 8 9); do
    find . -type d -name "n020${i}[0-9]*" -exec rm -rf {} +
done

# remove folders beginning with n0210 through n0212 
# run for loop without -exec first to ensure deleting correct files
for i in $(seq 10 12); do
    find . -type d -name "n02${i}[0-9]*"
done
# command with -exec
for i in $(seq 10 12); do
    find . -type d -name "n02${i}[0-9]*" -exec rm -rf {} +
done

# count number of folders in train set again; should be 858 directories
ls imagenet-mini/train | wc -l

# count number of files; should be 31,199
ls imagenet-mini/train/* | wc -l

# remove about 5/6th of the files in each folder of train directory
for dir in imagenet-mini/train/*; do
    # Get the number of files in the directory
    num_files=$(find "$dir" -maxdepth 1 -type f | wc -l)
    # Calculate half of the number of files (integer division)
    half_files=$((num_files * 5 / 6))

    # Delete the first half of the files
    if [ $half_files -gt 0 ]; then
        find "$dir" -maxdepth 1 -type f | head -n "$half_files" | xargs rm
    fi
done

# count number of folders in train set again; should still be 858 directories
ls imagenet-mini/train | wc -l

# count number of files; should now be 6,991
ls imagenet-mini/train/* | wc -l

# tar file
tar -cvf imagenet_mini.tar imagenet-mini

# view number of files in tar'd file; should be 6,136
tar -tf imagenet_mini.tar | wc -l

# remove imagenet-mini folder
rm -rf imagenet-mini

# mv imagenet_mini.tar to git Capstone data folder

```
