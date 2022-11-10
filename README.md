# Mask-RCNN TensorFlow 2
The repository provides a refactored version of the original [Mask-RCNN](https://github.com/matterport/Mask_RCNN) **without the need for any references to the TensorFlow v1 or the standalone Keras packages** anymore! Thus, the Mask-RCNN can now be executed on **any recent TensorFlow version** (`tested onto TF 2.9.1 env.`) and the **eager execution** can be tuned on/off for debugging anytime. Additionally, the DataGenerator is refactored providing the same results using *use_multiprocessing=False/True*.

The project was created to improve the old well known network while having fun breaking it, learning many crazy aspectes of TensorFlow and Python, and simply enjoying coding. I want also to thank [akTwelve](https://github.com/akTwelve/Mask_RCNN) for providing a great starting point I could build on. For full explanation on Mask-RCNN refer to the [original repo](https://github.com/matterport/Mask_RCNN) and to the original [medium article](https://medium.com/matterport-engineering/splash-of-color-instance-segmentation-with-mask-r-cnn-and-tensorflow-7c761e238b46).

# Getting started
1. Clone the repo.
2. Download the test images and the COCO weights from the [original Mask-RCNN repo](https://github.com/matterport/Mask_RCNN): 
    ```bash
    cd Mask_RCNN
    git clone https://github.com/matterport/Mask_RCNN.git original_repo
    mv original_repo/images .
    rm -rf original_repo
    wget -P weights https://github.com/matterport/Mask_RCNN/releases/download/v2.0/mask_rcnn_coco.h5
    ```

3. Create and run a docker environment (or create a local one based on the dependencies in the DockerFile).

    Create a docker image:
    ```bash
    cd Mask_RCNN
    docker build --build-arg UID=$(id -u) --build-arg GID=$(id -g) -f DockerFile -t mrcnn:tf2 .
    ```

    Run a docker container:
    ```bash
    cd Mask_RCNN
    docker run --name mrcnn --gpus all --rm -it -v $(pwd):/home/ai/dev -ip 8321:8321 -w /home/ai/dev mrcnn:tf2 jupyter notebook --no-browser --ip 0.0.0.0 --port=8321
    ```
    Execute the model trained on COCO in jupyter notebook: `samples/demo.ipynb`.

4. To get started with a real example take a look at the samples/balloon notebooks in the project. Before running them download the dataset:
    ```bash
    cd Mask_RCNN
    wget -P data https://github.com/matterport/Mask_RCNN/releases/download/v2.1/balloon_dataset.zip
    unzip data/balloon_dataset.zip -d data
    ```
5. To train Mask-RCNN on a custom data copy the balloon folder and adjust everything you need to your dataset. 

# Known-Warnings
The warnings are annoying but doesn't harm anything: 
* ... Error in PredictCost() for the op: op: "CropAndResize" ...
* ... UserWarning: Converting sparse IndexedSlices ...

# Known-Issues
The model doesn't train well using the configuration **USE_MINI_MASK=True**. This is probably due to tf.round(ouput_resized_mask) operation in the training graph. However, there are no problems using **USE_MINI_MASK=False**!

# Compatability-Issues
* **model.py/MaskRCNN/ancestor** looks to be obsolete because the [KerasTensor](https://github.com/keras-team/keras/blob/v2.8.0/keras/engine/keras_tensor.py#:~:text=KerasTensors%20are%20intended,instantiate%20%60KerasTensor%60s.) doesn't support the **op** member calling since TF 2.4 anymore. Consequently, all ancestor callings are disabled and remarked in the coresponding notebooks.

