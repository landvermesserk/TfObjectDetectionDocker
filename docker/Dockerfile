FROM tensorflow/tensorflow:2.14.0rc1-jupyter

RUN pip install Cython==3.0.11

RUN apt-get update && \
    apt-get install -y \
    protobuf-compiler \
    cmake

COPY /models /models

WORKDIR /models/research

RUN cp object_detection/packages/tf2/setup.py . && python -m pip install .

RUN pip install protobuf==3.20.3

RUN pip install tf-keras==2.14.1

RUN protoc object_detection/protos/*.proto --python_out=. 

RUN apt-get update && apt-get install -y libgl1

RUN pip install gin-config

RUN pip install onnx==1.14.0 onnxruntime==1.15.1 tf2onnx==1.15.0

RUN pip install onnx-graphsurgeon==0.3.10 --index-url https://pypi.ngc.nvidia.com

COPY tfexample_decoder.py /usr/local/lib/python3.11/dist-packages/tf_slim/data/

COPY create_onnx.py object_detection/packages/tf2/setup.py

COPY onnx_utils.py object_detection/packages/tf2/setup.py

RUN apt-get install -y vim

ENV PYTHONPATH=/models


#python model_main_tf2.py --model_dir=/model-training/training/ --pipeline_config_path=/model-training/training/pipeline.config
#python exporter_main_v2.py --input_type float_image_tensor --pipeline_config_path /model-training/training/pipeline.config --trained_checkpoint_dir /model-training/training/ --output_directory /model-training/models
#python create_onnx.py --pipeline_config /model-training/training/pipeline.config --saved_model /model-training/models/saved_model/ --onnx /model-training/models/saved_model/model_onnx.onnx --batch_size 1 --input_format NCHW --tf2onnx /model-training/models/model_tfonnx.onnx
#cp /models/research/object_detection/model_main_tf2.py /model-training && cp /models/research/object_detection/exporter_main_v2.py /model-training && cp /models/research/object_detection/create_onnx.py /model-training && cp /models/research/object_detection/onnx_utils.py /model-training
