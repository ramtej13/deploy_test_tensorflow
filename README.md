# deploy_test_tensorflow


# BUG


export_path = /Users/ramteja/Downloads/deploy_tensor/1

running these comands in the start docker with tenserflow serving

$ docker pull tensorflow/serving

-----> problem is at this command i am not able figurout bro it shows it is not able to fine the path. 

# try one:
$ docker run -p 8501:8501 --mount type=bind,source=/Users/ramteja/Downloads/deploy_tensor/1,target=/models/1 -e MODEL_NAME=test_model -t tensorflow/serving

#Error 1:

2020-10-13 17:41:14.543070: I tensorflow_serving/model_servers/server.cc:87] Building single TensorFlow model file config:  model_name: test_model model_base_path: /models/test_model
2020-10-13 17:41:14.543342: I tensorflow_serving/model_servers/server_core.cc:464] Adding/updating models.
2020-10-13 17:41:14.543903: I tensorflow_serving/model_servers/server_core.cc:575]  (Re-)adding model: test_model
2020-10-13 17:41:14.547615: E tensorflow_serving/sources/storage_path/file_system_storage_path_source.cc:362] FileSystemStoragePathSource encountered a filesystem access error: Could not find base path /models/test_model for servable test_model
2020-10-13 17:41:15.551822: E tensorflow_serving/sources/storage_path/file_system_storage_path_source.cc:362] FileSystemStoragePathSource encountered a filesystem access error: Could not find base path /models/test_model for servable test_model




# Try two:
$ docker run -p 8501:8501 --mount type=bind,source=/Users/ramteja/Downloads/deploy_tensor/1,target=/models/test_model -e MODEL_NAME=test_model -t tensorflow/serving

#Error 2:

2020-10-13 17:39:57.973846: I tensorflow_serving/model_servers/server.cc:87] Building single TensorFlow model file config:  model_name: test_model model_base_path: /models/test_model
2020-10-13 17:39:57.978167: I tensorflow_serving/model_servers/server_core.cc:464] Adding/updating models.
2020-10-13 17:39:57.978578: I tensorflow_serving/model_servers/server_core.cc:575]  (Re-)adding model: test_model
2020-10-13 17:39:57.997829: W tensorflow_serving/sources/storage_path/file_system_storage_path_source.cc:267] No versions of servable test_model found under base path /models/test_model











#Extra info:
i tryed default as show in this every thing works 

# Download the TensorFlow Serving Docker image and repo
docker pull tensorflow/serving

git clone https://github.com/tensorflow/serving
# Location of demo models
TESTDATA="$(pwd)/serving/tensorflow_serving/servables/tensorflow/testdata"

# Start TensorFlow Serving container and open the REST API port
docker run -t --rm -p 8501:8501 \
    -v "$TESTDATA/saved_model_half_plus_two_cpu:/models/half_plus_two" \
    -e MODEL_NAME=half_plus_two \
    tensorflow/serving &

# Query the model using the predict API
curl -d '{"instances": [1.0, 2.0, 5.0]}' \
    -X POST http://localhost:8501/v1/models/half_plus_two:predict

# Returns => { "predictions": [2.5, 3.0, 4.5] }
