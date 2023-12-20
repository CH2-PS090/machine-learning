# machine-learning

In this project we developed ML modules for the machine learning pipeline in the Equifit app. We trained all models using Google Colab.

<p align="center">
  <img align="center" width="400" src="https://github.com/CH2-PS090/machine-learning/assets/100210178/6d564617-7f36-4d9c-a5cd-d199c0ae6abb" />
</p>

Refer to [this code](https://github.com/CH2-PS090/predict-api/blob/main/predict.py) for the implementation of this pipeline.

### Explanation of the model pipeline:

This pipeline takes image, gender, weight, height, and age as input and output 15 measurements (ankle, arm-length, bicep, calf, chest, forearm, neck, hip, leg-length, shoulder-breadth, shoulder-to-crotch, thigh, waist, wrist, body fat).

The first module is [DeepLabV3](https://github.com/CH2-PS090/machine-learning/tree/main/DeepLab_v3). We use a pre-trained [DeepLabV3](https://github.com/tensorflow/models/tree/master/research/deeplab) model to process the raw image into a silhouette image.

The second module ([Measurement model](https://github.com/CH2-PS090/machine-learning/tree/main/Measurements_Model)) utilizes pre-trained [MobileNetV3Large](https://www.tensorflow.org/api_docs/python/tf/keras/applications/MobileNetV3Large) for feature extraction. It takes the silhouette image from the previous module and GHW (gender, height, weight) input and predicts 14 body measurements (ankle, arm-length, bicep, calf, chest, forearm, height, hip, leg-length, shoulder-breadth, shoulder-to-crotch, thigh, waist, wrist). We trained this model for 39 epochs using the [BodyM dataset](https://aws.amazon.com/marketplace/pp/prodview-w3762gcflmhzk?sr=0-1&ref_=beagle&applicationId=AWSMPContessa).

The third module is [Neck&BodyFat](https://github.com/CH2-PS090/machine-learning/tree/main/BodyFat_Prediction) predictor. This module comprises a Neck_Prediction model and a function to predict body fat. The Neck_Prediction model takes 8 measurements from the previous module (chest, waist, hip, thigh, ankle, bicep, forearm, wrist), age input, weight input, and height input to predict neck circumference. We trained the model using the [Body Fat Prediction dataset](https://www.kaggle.com/datasets/fedesoriano/body-fat-prediction-dataset). This predicted neck is then used to predict body fat percentage (BFP) in predict_bodyfat function with height input, gender input, waist and hip as additional input.

The output from the model are 13 measurements from the Measurement model, and neck measurement and BFP from the Neck&BodyFat predictor.

### Resources:
- [DeepLabV3](https://github.com/tensorflow/models/tree/master/research/deeplab)
- [MobileNetV3Large](https://www.tensorflow.org/api_docs/python/tf/keras/applications/MobileNetV3Large)
- [BodyM dataset](https://aws.amazon.com/marketplace/pp/prodview-w3762gcflmhzk?sr=0-1&ref_=beagle&applicationId=AWSMPContessa)
- [Body Fat Prediction dataset](https://www.kaggle.com/datasets/fedesoriano/body-fat-prediction-dataset)
- [U.S. Navy Body Fat formula](https://colab.research.google.com/corgiredirector?site=https%3A%2F%2Fwww.omnicalculator.com%2Fhealth%2Fnavy-body-fat)
