🖼️ CIFAR-10 Transfer Learning with EfficientNetB0

This repository demonstrates an advanced approach to image classification on the popular CIFAR-10 dataset utilizing transfer learning. By leveraging the powerful pre-trained EfficientNetB0 model, this project successfully scales tiny 32x32 images and classifies them into 10 distinct categories with high accuracy.

🧠 Model Architecture

The model architecture is built using a custom TensorFlow/Keras Sequential pipeline that seamlessly integrates the pre-trained EfficientNet base with a custom classification head:

Input Layer: Expects the native CIFAR-10 image size of (32, 32, 3).

Resizing Layer: Natively upscales the images to (96, 96) inside the model so the EfficientNet base can properly extract features.

EfficientNetB0 Base: Uses pre-trained ImageNet weights (with the top/head excluded).

Custom Classification Head:

GlobalAveragePooling2D() to flatten the feature maps.

Dropout(0.5) to aggressively prevent memorization/overfitting.

Dense(256) with a ReLU activation function.

Dropout(0.3) for further regularization.

Dense(10) with a Softmax activation function to output the final class probabilities.

⚙️ Training Strategy (Two-Phase Approach)

To maximize performance and preserve the valuable pre-trained weights, the model is trained in two distinct phases:

Phase 1: Feature Extraction

Frozen Base: The EfficientNetB0 base is completely frozen.

Objective: We train only the new custom classification head so it learns to interpret the features extracted by the base model.

Details: Uses the Adam optimizer for 5 epochs.

Phase 2: Fine-Tuning

Unfrozen Base: The entire model (including the EfficientNet base) is unfrozen.

Objective: Make microscopic adjustments to the entire network to specialize it for CIFAR-10.

Details: Uses a very low learning rate (1e-5) for 15 epochs to prevent catastrophic forgetting.

Callbacks:

EarlyStopping (restores best weights to prevent overfitting).

ReduceLROnPlateau (halves the learning rate if validation loss stops improving).

📊 Results

Following the two-phase training process, the model achieves highly competitive performance on the test set:

Final Test Accuracy: 92.47% 🎉

(Note: Logs indicate slight kernel delays during execution, but this does not impact the mathematical integrity or final accuracy of the model.)

💻 Requirements

To run this project locally, you will need the following installed:

Python 3.x

TensorFlow (tensorflow)

Matplotlib (matplotlib)

You can install the required packages using pip:

pip install tensorflow matplotlib


🚀 Usage

Clone this repository:

git clone https://github.com/yourusername/cifar10-efficientnet.git
cd cifar10-efficientnet


Run the script:

python train_model.py


(Assuming your python script is named train_model.py. Modify as necessary.)

Monitor the Output: The script will automatically download the CIFAR-10 dataset, download the EfficientNetB0 weights, execute Phase 1 and Phase 2 training, and print the final evaluation accuracy in the terminal.

📄 License

This project is open-source and available under the MIT License.
