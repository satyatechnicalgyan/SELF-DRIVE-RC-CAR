# SELF-DRIVE-RC-CAR
Python3 + OpenCV3
See self-driving in action



This project builds a self-driving RC car using Raspberry Pi, Arduino and open source software. Raspberry Pi collects inputs from a camera module and an ultrasonic sensor, and sends data to a computer wirelessly. The computer processes input images and sensor data for object detection (stop sign and traffic light) and collision avoidance respectively. A neural network model runs on computer and makes predictions for steering based on input images. Predictions are then sent to the Arduino for RC car control.

Setting up environment with Anaconda
Install miniconda(Python3) on your computer

Create auto-rccar environment with all necessary libraries for this project
conda env create -f environment.yml

Activate auto-rccar environment
source activate auto-rccar

  To exit, simply close the terminal window. More info about managing Anaconda environment, please see here.

About the files
test/
    rc_control_test.py: RC car control with keyboard
    stream_server_test.py: video streaming from Pi to computer
    ultrasonic_server_test.py: sensor data streaming from Pi to computer
    model_train_test/
        data_test.npz: sample data
        train_predict_test.ipynb: a jupyter notebook that goes through neural network model in OpenCV3

raspberryPi/
    stream_client.py: stream video frames in jpeg format to the host computer
    ultrasonic_client.py: send distance data measured by sensor to the host computer

arduino/
    rc_keyboard_control.ino: control RC car controller

computer/
    cascade_xml/
        trained cascade classifiers
    chess_board/
        images for calibration, captured by pi camera

    picam_calibration.py: pi camera calibration
    collect_training_data.py: collect images in grayscale, data saved as *.npz
    model.py: neural network model
    model_training.py: model training and validation
    rc_driver_helper.py: helper classes/functions for rc_driver.py
    rc_driver.py: receive data from raspberry pi and drive the RC car based on model prediction
    rc_driver_nn_only.py: simplified rc_driver.py without object detection

Traffic_signal
    trafic signal sketch contributed by @geek111

How to drive
Testing: Flash rc_keyboard_control.ino to Arduino and run rc_control_test.py to drive the RC car with keyboard. Run stream_server_test.py on computer and then run stream_client.py on raspberry pi to test video streaming. Similarly, ultrasonic_server_test.py and ultrasonic_client.py can be used for sensor data streaming testing.

Pi Camera calibration (optional): Take multiple chess board images using pi camera module at various angles and put them into chess_board folder, run picam_calibration.py and returned parameters from the camera matrix will be used in rc_driver.py.

Collect training/validation data: First run collect_training_data.py and then run stream_client.py on raspberry pi. Press arrow keys to drive the RC car, press q to exit. Frames are saved only when there is a key press action. Once exit, data will be saved into newly created training_data folder.

Neural network training: Run model_training.py to train a neural network model. Please feel free to tune the model architecture/parameters to achieve a better result. After training, model will be saved into newly created saved_model folder.

Cascade classifiers training (optional): Trained stop sign and traffic light classifiers are included in the cascade_xml folder, if you are interested in training your own classifiers, please refer to OpenCV doc and this great tutorial.

Self-driving in action: First run rc_driver.py to start the server on the computer (for simplified no object detection version, run rc_driver_nn_only.py instead), and then run stream_client.py and ultrasonic_client.py on raspberry pi.
