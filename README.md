No dataset provided in this repoitory. 

# Mailware Detector CNN and White Box Evasion Attack Implementation

## 1. Mailware Detector
The network expects a fixed-length vector as input, on which it applies a one-dimensional convolutional layer with a kernel size of 10, stride of 1, and no padding. The input has 1 channel, and the output has 16 channels. A ReLU activation function is applied to the output of the convolutional layer, followed by a 2D max pooling layer with a kernel size of 2. The output of the max pooling layer is then passed through a linear mapping which produces a single scalar value, also known as a logit. Applying a sigmoid function to the logit value yields a probability between 0 and 1. If this value is greater than 0.5, the decision is "malware"; otherwise, it is "benign (non-malware)". The loss function is binary cross-entropy. The implementation should be done in the PyTorch framework.

### Tasks:
Calculate the average accuracy of the trained model on the test data with s = 2^14.

Calculate TPR, TNR, FPR, FNR, and AUC values on the test data.

## 2. White Box Evasion Attack

The task is to implement a white-box evasion attack with the goal of misclassifying the malware files in the victim directory (i.e., recognizing them as benign) using the model trained in the first task. To achieve this, the attacker can modify the malware files according to the following rules:

The attacker cannot change the preprocessing of the data (resizing the binary).
The attacker does not want to ruin the malware machine code, so they can only append extra bytes to the end of the binary file, which is called the adversarial suffix. They cannot modify any other bytes. Therefore, the attacker can only modify the elements of the model input that correspond exclusively to these extra suffix bytes. The positions in the model input that can be modified by the attacker are called the adversarial mask, denoted by set M (|M| < s). For example, if the binary file is [1, 2, 3, 4, 5, 6, 7, 8, 9, 10] and the attacker appends 4 zero bytes as the adversarial suffix, the model expects an input vector of length s = 6, thus the resized vector becomes x = [2, 5, 8, 3.33333333, 0, 0]. In this case, the attacker cannot modify any bytes (M = ∅) since none of the elements correspond exclusively to the suffix bytes (the 4th element covers the 10th element of the original byte sequence which is not part of the adversarial suffix, and the 5th element covers more than just the last two zero bytes, including a padding byte due to the resizing process which the attacker also cannot modify). However, if the attacker increases the suffix length from 4 to 5 bytes: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 0, 0, 0, 0, 0], the resized vector remains [2, 5, 8, 3.33333333, 0, 0], but now the attacker can modify the last element (M = {5}), as it exclusively covers the last three elements of the suffix.
The attacker knows the parameters of the attacked model and can calculate its gradient with respect to the input.
The attacker applies a white-box Projected Gradient Descent (PGD) attack.

### Tasks:

First, evaluate a simple baseline attack where the attacker sets the adversarial suffix to random byte values and appends this to the binary (i.e., not using PGD). Calculate the attack accuracy (the proportion of successfully misclassified malware samples in the victim/malware directory) if the size of the adversarial suffix is 5%, 10%, 15%, and 20% of the original binary file length! For each case, also provide the size of the adversarial mask |M|!

Calculate the PGD attack accuracy (the proportion of successfully misclassified malware samples in the victim/malware directory) if the size of the adversarial suffix is 5%, 10%, 15%, and 20% of the original binary file length! For each case, also provide the size of the adversarial mask |M|!
