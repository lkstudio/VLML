# VLML
Machine learning in the visual language VL, part of [vvvv Gamma](https://vvvv.org/blog/vvvv-gamma-2019.1-preview) and [vvvv Beta](https://vvvv.org/downloads)

---
## VLML is a machine learning framework that aims to take some of the hassle out getting your hands dirty with machine learning, making it a relatively easy to get started. VLML uses [CNTK](https://github.com/Microsoft/CNTK) as its back end, one of the most performant machine learning libraries out there. __There is a short guide [here](https://github.com/ThinkingForms/VLML/wiki/How-to-Install,-in-pictures) on how to install VLML.__ 

- For now I __recommend installing__ and testing VLML with [vvvv alpha f7d5bf1879](http://teamcity.vvvv.org/guestAuth/app/rest/builds/id:30173/artifacts/content/vvvv_50alpha38.2_x64.zip) because later versions currently have some compile errors, I'll update this when they stabilize. 
- There are now some __demos__ available to download from [here](https://drive.google.com/file/d/10EqCHrWxLO3k-K4JzKLmKHOXVc-noURU/view?usp=sharing) that go through some basic usecases for machine learning. A git repository can be found [here](https://github.com/ThinkingForms/VLML_Demos_V1).
- Finally there is also a __toy__ to play with: [pix2pix face painting!](https://drive.google.com/open?id=1-kl213Frwk4mAQLASJ8EyZpBlRty9lcc). This is an evaluator of a pix2pix (ConditionalGan) model was trained on photographs and segmentation data. The neural network will be released soon so you can train your own. See more about this subject [here](https://github.com/phillipi/pix2pix).
<img src="https://vvvv.org/sites/default/files/screenshot1556244666.png" width="400">

---
## Setting up a basic network
#### Create and two flattened collections of data.

The first will be X (the kind of data you want to process) and second will be Y (the kind of data you expect the process to result in).

![Creating a dataset](https://github.com/YanYas/VLMLDocuments/blob/master/documentation_assets/Getting_Started/light/VLML101-Application-Dataset.png)

#### DataInput: Create __DataIn__ nodes for each of your datasets
The sample size refers to the number of elements in each sample of your dataset. In this example X has 3 elements, Y has 1.

![Creating inputs](https://github.com/YanYas/VLMLDocuments/blob/master/documentation_assets/Getting_Started/light/VLML101-Application-DataIn_Nodes.png)

#### Design your network
Put in some layers. Here we have just one and without an activation function.

![Creating a network](https://github.com/YanYas/VLMLDocuments/blob/master/documentation_assets/Getting_Started/light/VLML101-Application-Layers.png)

#### Pass your network and target data into a loss function.
There several to choose from that are best suited to different ML tasks. You can also write your own.

![Setting a loss function](https://github.com/YanYas/VLMLDocuments/blob/master/documentation_assets/Getting_Started/light/VLML101-Application-Loss_Function.png)

---


## Set up the trainer
Pass your network and loss function into a trainer:

- Set your Minibatch Size
    This the size of a chunk of you data set to train consecutively each step. In this case it is set to zero so the whole batch of data (every sample of the data set) is used in each training step.

- Set the learning rate for your trainer.
    This defines the size of the learning steps the trainer can adjust the network with.

- Press train to run the training session

![Training](https://github.com/YanYas/VLMLDocuments/blob/master/documentation_assets/Getting_Started/light/VLML101-Application-Training_Optimizer.png)

---

## Evaluate your model

#### Method 1

- Create an DataIn node with a sample size that matches the one used for the network's 'x' input (3). This will have test data.
- Make an Evaluate(Debuggable) node
  Pass in the Network
  Pass in the test DataIn
- Use a DataOut node to output the result in a format of your needs, in this case float.

![Evaluation 1](https://github.com/YanYas/VLMLDocuments/blob/master/documentation_assets/Getting_Started/light/VLML101-Application-Evaluate_Model_1.png)

#### Method 2
- Use 'SetInput' node, taking your model and a DataIn node with a size matching the input of the your model.
- pass this into Evaluate node
- Use a DataOut node to output the result in a format of your needs.

![Evaluation 2](https://github.com/YanYas/VLMLDocuments/blob/master/documentation_assets/Getting_Started/light/VLML101-Application-Evaluate_Model_2.png)

The main difference between these two approaches is that the Debuggable Evaluator can use the latest model while it updates. The 'SetInput' node only updates the model when a new model is passed into in to it. It is best used with finished models.
