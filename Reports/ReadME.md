# ue5osc

- ue5osc refers to the environment we use in order to communicate between Unreal Engine 5 and python. 
- This environment works through UE5+'s Open Sound Control(OSC) plugin and python's pythonosc library.
- [ue5osc](https://github.com/arcslaboratory/ue5osc) can be cloned from this repository.
    - After cloning this repository in order to install this package locally:
    ```
    conda activate ...
    python -m pip install --editable .```
- You can run this example code with

~~~bash
python demo.py
~~~

- This package contains 4 important files:
    - **ue5osc/__init__.py**: This file handles the interaction between the UE5 environment and a python script by allowing sending messages in the form of an address and an information type to UE5.
    - **ue5osc/osc_dispatcher.py**: This file handles the communication from Unreal Engine 5+ to python. The way this file works is by having a dispatcher that parses for certain addresses to decode what type of information is being sent to python. We then return this information as requested.
    - **demo.py**: Demo.py simply shows how one is able to use the package to send and receive information from unreal engine.
    - **setup.py**: This file  is necessary for allowing one to setup this package on their local system.

# ARCS Demo

The rest of our repository's were built with our ARCS Demo project in mind. You will need to download our packaged game or the version on GITEA.

This demo project has a 3D model of what Oldenborg looks like, has a robot with movement interactive movement controls that work with ue5osc, and has the ability to change the environment's textures to allow for great variance.

# Boxnav

- Boxnav is a repository which allows for the creation of a virtual environment which will move said agent alongside their unreal counterpart. It is made to collect images as it traverses an environment and build a dataset.
- To use this repository:
    - Clone the [Boxnav Repository](https://github.com/arcslaboratory/boxnav).
    - Then you will need to create and activate an anaconda environment with 
    ```
    conda create --name <env_name>
    conda activate <env_name>
    ```
    - Next, install the needed libraries with:
    ```
    conda install python matplotlib celluloid
    python -m pip install python-osc
    ```
    - After cloning the ue5osc library, navigate into this repository and install the library with:
    ```
    cd ue5osc
    python -m pip install --editable
    ```
    - Now ensure that every time you wish to run boxnav you have first opened the environment with
    ```
    conda activate <env_name>
    ```
    - After activating the environment, the script is now ready to run.
    - You can look at the arguments in more detail in boxsim.py but in general to start the script you will type the folllowing:
    ~~~bash
    python boxsim.py <navigator> --ue
 
 ## Boxnav Files (A closer look):
    
- **box.py**: This file creates some simple methods to create points, determine their distance between themselves and the agent.
- **boxenv.py**: This file creates boxes with coordinates to create an environment where the virtual agent will navigate around in.
- **boxnavigator.py**: Contains two different navigators which affect the behavior of the agent navigating throughout the environment. Also contains functions, that allow for movement of the agent throughout the simulated environment. The Perfect Navigator works by the agent shooting a straight line to the target and heading directly to it until it reaches the end, while the wandering navigator has a 25% to take a random action.
- **boxunreal.py**: Wrapper for the navigators that facilitates coordination with Unreal Engine 5. It contains the methods that allow the robot in unreal engine to make valid movements, save images, and sync the position of the simulated agent to the robot in UE5+.
- **Boxsim.py**: This file creates the boxes for our simulated environment. We then have multiple arguments to decide how you want to run our code. The only ones that are necessary for the script to run is the navigator. This code runs a simulation where the agent and robot in unreal engine both move in tandem throughout the environment attempting to get to the end target within a specified amount of actions.

# Oldenborg Training

- This repository is meant to allow for easy training of a model using a dataset from our boxnav simulation, and then using the trained model to run inference all in one place. 
- To run this repository once again create and open an environment and run the following commands:

~~~bash
conda create --name <env_name>
conda activate <env_name>
~~~

~~~bash
python -m pip install python-osc
pip install wandb fastai
~~~

- Navigate into the ue5osc repository and install the library with:

~~~bash
cd ue5osc
python -m pip install --editable
~~~

## Training.py

## Inference.py

- Inference.py has three main functions. 
    - *label_func(filename)* which separates the name of any image files into the appropriate directions.
    - *get_image(filename)* which requests the image that we have recently saved.
    - *main()* which asks for command line arguments and allows the script to run


- To run this script one must install the following packages:
```
pip install wandb fastai
```


    - Either use learn.export and load_learner or torch.save - Should just use the fastai stuff. Alter Img+command to work with the fastai library and not just pytorch. 
    - get rid of wandb stuff and just use the stuff that we already have downloaded
    - just so we can figure out how to do learner.export

    - linux vs windows thing. Should just train the models on the server.
    - Forward should be between 0.10472
    - Should make the threshold into an argument to change what it considers forward vs. a rotation.
    - Make the boxnav boxes skinnier to avoid obstacles
    - Make a function that only allows movement inside of the box where the target is located.
    - env-visiblity-navigator-estimate-img-optional-descriptor (testing wandb notes)
    - Uploading models: https://wandb.ai/arcslaboratory/registry/model
