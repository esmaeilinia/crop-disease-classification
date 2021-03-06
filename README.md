# Automated classification of tomato plant disease
Deep learning based automated tomato plant disease classification covering over 40 disease classes and 4 healthy classes. 

## Get Started

### Instal python virtualenv
sudo apt install virtualenv
virtualenv --system-site-packages -p python3 py35

### Activate the python virtualenv (run from root)
source py35/bin/activate

### Install dependencies
sh requirements.sh

Note: <br />
1. To setup nginx properly, follow the setup tutorial here: https://www.matthealy.com.au/blog/post/deploying-flask-to-amazon-web-services-ec2/

2. Delete the default nginx page <br />
sudo rm /etc/nginx/sites-enabled/default

3. Restart the nginx server <br />
sudo service nginx restart

## Project tree

 * [Kisan_app](./kisan_app)
   * [model](./kisan_app/model): directory containing model files for API
   * [templates](./kisan_app/templates)
   * [static](./kisan_app/static)
   
 * [tf_files](./tf_files)
   * [models](./tf_files/models): directory containing model files for training
   * [bottlenecks](./tf_files/bottlenecks): feature files for each image for each
 * [data_dir]: get data_dir @{insert link to S3 bucket}
 * [scripts](./scripts): contains python files for training the models, evaluation and quantization
 * [train.sh](./train.sh): bash script to run the training for inceptionV3 model
 * [train_mobilenet.sh](./train_mobilenet.sh): bash script to run the training for the mobilenet model
 * [retrain.sh](./retrain.sh): bash script to re-train the model on new images
 * [retrain_compare.py](./retrain_compare.py): utility script for retrain.sh
 * [README.md](./README.md)

## Training the image-classification model

### Activate the python virtualenv (run from root)
source py35/bin/activate

### Change the dir to crop_classifcation_updated
cd KisanLab_CPU

### Train the inceptionV3 model (more accurate, larger size, slow to train)
sh train.sh

or 

### Train the mobilenet model (less accurate, smaller size, fast to train)
sh train_mobilenet.sh

## API

Kisan_app folder contains all the files for the API. 

### How to run the API?

### Start the server (run from root)
source py35/bin/activate

### Change dir to server dir
cd crop_classification_updated/kisan_app

### Start the server (logging occurs in nohup.out file)
nohup gunicorn app:app -b localhost:8000 &

### Access API via curl cmd
curl -F file=@/path/to/your/image ${PUBLIC_IP_OF_EC2_INSTANCE}/api_call

#### Output (in json)

[
{
"Prediction1": "tomato fruit borer", 
"Confidence1": "0.490311", 
"Confidence2": "0.258647", 
"Prediction2": "cutworm on tomato"
}
]

or 

### Access the web-api via the public IP of the ec2 instance

#### Output
![Alt text](./sample.png?raw=true)

