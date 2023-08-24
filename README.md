# Pokemon-Type-Classifier


Pokémon has become an internationally recognized and beloved media franchise. As of 2023, there are over 1000 different Pokémon, each having a specific primary type and some even having a secondary type. Understanding a Pokemon’s type is a crucial aspect of the game because many attributes, such as the advantage a Pokémon may have against others, are governed by its type. Currently, there are 18 different types, leading to 324 different possible type combinations. This may be daunting for newcomers to explore, since it is not always easy to discern a Pokemon’s type.
Using deep learning was the optimal choice due to the complexity of the problem. There are many Pokémon with different appearances and characteristics and it is difficult for patterns to be found. But by extending our search into higher dimensions using deep learning, we are able to detect underlying patterns that would be hard to determine otherwise. 

# Diagram
![pokemon illustration](https://github.com/GoldFishGod/Pokemon-Type-Classifier/assets/113151722/3ef43f51-c5a1-4e9e-9ce3-a97dba0cf3ec)

# Data Processing
Pokemon images from eight generations were from three different online databases: 
[https://www.kaggle.com/datasets/vishalsubbiah/Pokemon-images-and-types], [https://www.kaggle.com/datasets/hlrhegemony/Pokemon-image-dataset] and [https://github.com/jackw-ai/CNN-Pokemon-Classifier]. 
 9885 images were organized into the proper classes from these three sources and were then split into train, validation and test datasets in a ratio of 70%-15%-15% respectively. This was done once for primary types and once for secondary types. Since around half of all existing Pokemon do not have a secondary type, they were categorized into the ‘None’ folder. 
 These images were then resized to 120x120 pixels and converted to 3-channels for RGB, ignoring any alpha channels. In addition, to reduce the chances for overfitting, data augmentation, including flipping, cropping, colour jitter, and rotations were implemented. We also extended our dataset by using AlexNet for feature extraction. The images were resized to 3x224x224 and extracted features to produce the same amount of 256x6x6 sized tensors.
 
 ![pokemon data augmentation](https://github.com/GoldFishGod/Pokemon-Type-Classifier/assets/113151722/572dc203-31e2-4174-b05b-b9309f215db2)

# Model
![pokemon model](https://github.com/GoldFishGod/Pokemon-Type-Classifier/assets/113151722/6a19e47f-603c-451c-8dac-49433426225e)


First, AlexNet is used to extract features from our training, testing and validation data. Next, the training tensors derived from those features are fed into our CNN model with two convolutional layers for hierarchical representation, followed by two fully connected layers used for classifying Pokemon into their respective types. 

Both convolutional layers have a kernel size of 4, a stride of 1, and padding of 1. The first convolutional layer takes 256 input channels and produces 380 output channels, while the second convolutional layer takes 380 input channels and generates 512 output channels. Next, the first fully connected layer maps the 512x4x4 input features to 1024 output features. Finally, the second fully connected layer uses these 1024 features to classify the input picture into one of 18 or 19 classes, depending on whether it is categorizing primary or secondary types, respectively. Notably, secondary Pokemon types have an extra class, as Pokemon without a secondary type are organized into the 'None' category.

The model operated on 13810 samples in the primary type training dataset using a batch size of 256 and a learning rate of 0.01, and 7682 samples in the secondary type training dataset using a batch size of 512 and a learning rate of 0.005. 

# Results
We have chosen to perform the quantitative analysis of our model in comparison to the baseline, random forest model, to see how well it is performing. To begin, we review the training and validation of accuracies of both models. The baseline model, trained with the same dataset had the following accuracies: 
Training: primary 29.7% & secondary 30%.
Validation: primary 29.8% primary & 6.9 secondary

Due to the nature of the random forest model, an average of all the different training accuracies was taken, and these numbers are where the model plateaued at. In comparison, our model, which used AlexNet’s transfer learning and additional training managed to achieve the following accuracies: 
Training: primary 98.7% & secondary 99.4%
Validation: primary 63.3% & secondary 66.7%

The more detailed evolution by epoch number is presented with figure 1 and 2, illustrating that our model is able to perform accuracies that are more than three times better than the baseline. However, training and validation accuracies aren’t enough to confidently conclude better performance. Thus, we also examine the test accuracies of both models for comparison: 

Baseline Test: primary 30.7% & secondary 28.8%
Our Model Test: primary 60.6% & secondary & 66.6%
