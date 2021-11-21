
# Detecting Diabetic Retinopathy 

> Diabetic retinopathy (DR), also known as diabetic eye disease, is a medical condition in which damage occurs to the retina due to diabetes mellitus. It is a leading cause of
> blindness. Diabetic retinopathy affects up to 80 percent of those who have had diabetes for 20 years or more. Diabetic retinopathy often has no early warning signs. Retinal 
> (fundus) photography with manual interpretation is a widely accepted screening tool for diabetic retinopathy, with performance that can exceed that of in-person dilated eye 
> examinations.

The below figure shows an example of a healthy patient and a patient with diabetic retinopathy as viewed by fundus photography

<p align = "center">
<img align="center" src="https://user-images.githubusercontent.com/63184114/142764783-d787759a-6e19-4543-bf4c-6bc115e00c28.png" alt="Retinopathy GIF"/>
</p>

### The motivations for this project are twofold:

- Image classification has been a personal interest for years, in addition to classification on a large scale data set.

- Time is lost between patients getting their eyes scanned (shown below), having their images analyzed by doctors, and scheduling a follow-up appointment. By processing images in real-time, EyeNet would allow people to seek & schedule treatment the same day.
## Table of content

1. [Data](#data)
2. [ Data Analysis](#data-analysis)
3. [Preprocessing](#preprocessing)
4. [Training](#training)
5. [Resizing](#resizing)
6. [Results](#results)
7. [Future works](#future-works)
8. [References](#references)

## Data 
>The data originates from  [here](https://www.kaggle.com/c/diabetic-retinopathy-detection/data). 
>
>All images are taken of different people, using different cameras, and of different sizes.
>
> Pertaining to the preprocessing section, this data is extremely noisy, and requires multiple preprocessing steps to get all images to a useable format for training a model.
> 
>The training data is comprised of 35,126 images, which are augmented during preprocessing.
>
>we created an automated analysis system capable of assigning a score based on this scale.
>

## Data Analysis
we utilized transfer learning, oversampling, and progressive resizing on this small, imbalanced dataset. 
> Open the dataset with pandas, check distribution of labels, and oversample to reduce imbalance. The dataset is highly imbalanced, with many samples for level 0, and very >little for the rest of the levels.
> 
>A clinician has rated the presence of diabetic retinopathy in each image on a scale of 0 to 4, according to the following scale:
>
>0 - No DR
>
>1 - Mild
>
>2 - Moderate
>
>3 - Severe
>
>4 - Proliferative DR
>

## Preprocessing
> The images were actually quite big. We  resize to a much smaller size and try to use progressive resizing to our advantage when dealing with such a small dataset.  
> 
> Then loaded the dataset into the ImageItemList class provided by `fastai` .
> 
> The fastai library also implements various transforms for data augmentation to improve training.While there are some defaults that I leave intact we add vertical flipping >(do_flip=True) and 360 deg. max_rotate=360 as those >have been commonly used for this particular problem.

## Training
>Cohen's quadratically weighted kappa  is a better metric when dealing with imbalanced datasets like this one, and for measuring inter-rater agreement for categorical >classification (the raters being the human-labeled dataset and the neural network predictions).
>
> Implementation is based on the scikit-learn's implementation, but converted to >a pytorch tensor, as that is what fastai uses.
> 
>We use transfer learning, where we retrain the last layers of a pretrained neural network. also used the **ResNet50** architecture trained on the ImageNet dataset, which has >been commonly used for pre-training applications in computer vision. Fastai makes it quite simple to create a model and train


## Resizing
>Progressive resizing is a technique developed by Jeremy Howard as part of the fast.ai class . The idea is that we train with smaller images at the beginning, and retrain with larger images, which will have more information to learn from.
>
> This could be very helpful for dealing with small datasets, and we decided to give this a try. we only resized once due to kernel time limitations, but theoretically, we could continue to resize and see if that improves accuracy, especially since the retinal images are so big in size.

<p align = "center">
<img align="center" src="https://user-images.githubusercontent.com/63184114/142775839-1f82cc04-ee9d-4607-b233-a340ed9a9d38.png" alt="Retinopathy GIF"/>
</p>

## Results
We look at our predictions and make a confusion matrix.
<p align = "center">
<img align="center" src="https://user-images.githubusercontent.com/63184114/142775739-1eb595bb-5161-4a7e-91f3-18f7ac66b316.png" alt="Retinopathy GIF"/>
</p>
Quite impressively, with only 1000 images, oversampling, and transfer learning, we acheive decent agreement with the correct labels.

## Future works
>There are other methods for dealing with imbalanced datasets. The most common alternative is to use a weighted loss function, with the class weights dependent on the >distribution of the dataset.
>
>Some possible future experiments include:
>
>Training the dataset without the healthy (0) class, then retrain with the healthy class.
>
>Possibly using mixup as a form of data augmentation. It would be interesting to see how well it would perform with medical data.
>
>Utilize rectangular images instead of cropping. Most of the data has an aspect ratio of ~1.5, and by default, the fastai library center-crops squares to pass into the network. >Also for this dataset, it may not provide significant advantage as the circular retinal images are already padded, and cropping them removes much of the padding.

## References

- [Diabetic retinopathy Information](https://www.nei.nih.gov/learn-about-eye-health/eye-conditions-and-diseases/diabetic-retinopathy)
- [RESNET5-0]( https://iq.opengenus.org/resnet50-architecture/#:~:text=ResNet50%20is%20a%20variant%20of,explored%20ResNet50%20architecture%20in%20depth.)
- [fastai](https://www.fast.ai/)
