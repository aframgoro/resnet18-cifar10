# How Much Data Does a CNN Need? ResNet-18 on CIFAR-10

An experiment that measures how the size of the training set affects the accuracy of a convolutional neural network for image classification.

ResNet-18 was trained **from scratch** (no pretrained weights) on 10%, 25%, 50% and 100% of the CIFAR-10 training set. Everything else was kept identical, so the only variable is the amount of data.

## Results

Test accuracy, mean of 3 runs per data size:

| Training data | Images | Test accuracy |
|---|---|---|
| 10% | 5,000 | 64.2% |
| 25% | 12,500 | 73.6% |
| 50% | 25,000 | 79.4% |
| 100% | 50,000 | **84.3%** |

<img width="622" height="432" alt="accuracy" src="https://github.com/user-attachments/assets/c62f14c0-65cb-4e42-ba99-185801790b51" />


**Takeaway:** more data always helped, but with diminishing returns. Going from 10% to 25% added 9.4 percentage points, while doubling from 50% to 100% added only 4.9. Past a certain point, better augmentation or architecture may be worth more than collecting more data.

## Setup

| Setting | Value |
|---|---|
| Model | ResNet-18 (torchvision), final layer replaced for 10 classes |
| Dataset | CIFAR-10: 60,000 color images, 32×32 pixels, 10 classes |
| Augmentation | Padding + random crop, random horizontal flip, normalization |
| Optimizer | Adam, learning rate 0.001 |
| Loss | Cross-entropy |
| Batch size | 128 |
| Epochs | 50 per run |
| Runs | 3 per data size, each with a freshly initialized model |

## How to run

Open `resnet18_data_size.ipynb` in Google Colab with a GPU runtime (Runtime → Change runtime type → GPU) and run all cells. CIFAR-10 downloads automatically.

Training all 12 runs takes a few hours on a Colab GPU.

## What I learned

- How to design a controlled experiment where only one variable changes
- Why test data must never influence training decisions: an early version of our pipeline used the test set during training, which we caught and fixed before re-running everything
- How data augmentation helps a model generalize from limited data
- That the relationship between data size and accuracy is not linear

## Possible improvements

- Use the CIFAR-style ResNet-18 (3×3 first convolution, no max pooling), which keeps more detail in the small 32×32 images and usually reaches above 90%
- Pick the training subsets randomly with a fixed seed and stratified by class
- Hold out a separate validation set from the training data for model selection and early stopping

## Author

Afram Goro
