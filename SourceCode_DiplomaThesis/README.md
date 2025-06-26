This repository includes all the code related to the work of my Diploma thesis: <b>Motion synthesis about articulated 
and non-articulated characters</b>, during the last year of my studies at the Computer Science and Enginneering Department of University of Ioannina.

<H2>Blender</H2> 

We use the Blender enviroments in which we import the motions from the isotope Mixamo. \n
<u>Examples of motions</u>  

![Screenshot (1472)](https://github.com/GiorgosDrog/diploma_thesis_motion_synthesis/assets/72260809/73cf04e9-8390-491a-8c2c-ab03eece8074)

From motions like this we extract files with numpy format
We already have extracted files in the folder npy_files. These files are used for Datasets about model's training and validation datasets 

For the DataSets we follow the class stucture which are appeared in the schema below.
<H2> Dataset </H2>

![image](https://github.com/GiorgosDrog/diploma_thesis_motion_synthesis/assets/72260809/0a120926-4229-4270-94fe-2970596065c5)

<H2> WorkFlow </H2>

![ροήδεδομέωνΔιπλωματικη (3)](https://github.com/GiorgosDrog/diploma_thesis_motion_synthesis/assets/72260809/08ea37ae-2d0c-4356-9044-93d4d4c37b46)

<H2> Models architectures </H2>
The models follow an Encoder Decoder architecture. The goal is the extraction meaningful information from bones matrices <br> and with them we want to genereate the mesh vertices of the motion for each animation. for the project have beed developed two architecture, one with LSTM encoder decoder with attention mechanism and the second one with encoder decoder intermidiate calculations.   
The schemmas below show the both architecture

<H3>LSTM_Encoder_decoder_attention</H3>

![image](https://github.com/GiorgosDrog/diploma_thesis_motion_synthesis/assets/72260809/420ae429-ae62-4c8f-8646-69a85df19ad1)

<H3>LSTM_Encoder_Decoder</H3>

![image](https://github.com/GiorgosDrog/diploma_thesis_motion_synthesis/assets/72260809/982f983c-88c1-4c5e-893a-7226149fd3e8)

<H2>Motion Synthesis</H2>
To acheive the goal of the project we develop two methods. At first we take the already trained models and we use them for the moethod. We swap the decoder elements to blend the trained parameters for one motion with the encoded information of the other motion. With this way the decoder generates a mixed motion. The schema below represents the architecture of the method.

![image](https://github.com/GiorgosDrog/diploma_thesis_motion_synthesis/assets/72260809/728f3ffe-dbe8-4668-a547-9326808deb6c)

Furthermore the second method splits the motions in seperate categories. The categories are seperated with vector like [0.0,0.0,0.0,1.0,0.0] - > Jump , [0.0,0.0,0.0,0.0,1.0] - > Walk , [1.0,0.0,0.0,0.0,0.0] - > point , [0.0,1.0,0.0,0.0,0.0] - >  rotate , [0.0,0.0,1.0,0.0,0.0] - > turn. When we split the animation in categories we trained a another model like the LSTM_Encoder_Decoder_Αttention with addition of an input to the decoder with will takes the cotegory vector. In the training phase the model must understand the connection and the categories. Also it must generate well the mesg vertices for the motion. With these changes at the inference mode of the model we can feed an category like [1.0,0.0,0.0,0.0,1.0] - > Walk + Point which must gives us the a mix motion of walking and pointing.

The schema below represents the architecture of the category model 
<H3>LSTM_encoder_decoder_attention_categories</H3>

![MainTechnique (1) (1)](https://github.com/GiorgosDrog/diploma_thesis_motion_synthesis/assets/72260809/c80cd8e5-567b-4f3d-ae0d-19f9665b7338)

<H2> Results of the project  </H2>
+ Training and Validation diagrams 
Below we represent example of training and validation

![image](https://github.com/GiorgosDrog/diploma_thesis_motion_synthesis/assets/72260809/8ccb9f29-0c11-4e22-9c33-380097f27363)
![image](https://github.com/GiorgosDrog/diploma_thesis_motion_synthesis/assets/72260809/d3a1a77e-0587-4746-b17a-b183e8cf154b)

+ Motion results
  
 ![Screenshot (1475)](https://github.com/GiorgosDrog/diploma_thesis_motion_synthesis/assets/72260809/79116173-21f8-4c4a-a8ce-edf52b5bab54)

+ Motion blending  -  Synthesis
Motion Synthesis results with Swap method

![Screenshot (1477)](https://github.com/GiorgosDrog/diploma_thesis_motion_synthesis/assets/72260809/d0649dd0-e013-4292-a1c1-f5539c9f1c25)


Motion Synthesis result with categories method

Jump + Walk - > [0.0,0.0,0.0,1.0,1.0]

![Screenshot (1478)](https://github.com/GiorgosDrog/diploma_thesis_motion_synthesis/assets/72260809/df455683-78f7-46f3-8c36-a118fd830b89)

Jump + Point - > [1.0,0.0,0.0,1.0,0.0]

![Screenshot (1479)](https://github.com/GiorgosDrog/diploma_thesis_motion_synthesis/assets/72260809/c87deec5-ab1a-49b3-8c66-40f8e10fc8f8)



<H2> HOW TO RUN THE CODE </H2>
To run the code you must have already initialize all the libraries of the requirements (Cuda,PyTorch,Numpy,Matlib)
Also the folder <b>npy_files</b> must be in the same path as the <b>MainController.py</b> script.
The MainController.py script is the main script of the project. In this script we initialize the models, the Datasets, and we handle the training and the test section of the models. Also, in the this script we develop the synthesis and the categories method.  

Steps to execute the code: 

+ You can run the code by the script MainController.py, this includes all the mothods which are described above.
+ You have to choose between functionalities of the code, <b>Train, test, Synthesis,categories ,Evaluate categories </b>
+ You can generate new npy_files with the blender files which have blend suffix. when you open one of the blender file you can run code from blender scripts to visualize or generate new npy_files.
+ we have all ready npy_files with bones and vertices information because these are nessecery for the Dataset initialization.
+ If you want to train or test a model, you must execute Test or Train.
+ By default we train the LSTM_Encoder_Decoder_Attention. WE can change this if you change the name of the initialization of the models at the MainController.py script

![Screenshot (1480)](https://github.com/GiorgosDrog/diploma_thesis_motion_synthesis/assets/72260809/9679fb1d-5179-467c-91c1-96e1c3cbffae)

Also you can use the Synthesis method if you choose <b> Synthesis </b> 
To train the categories model choose <b> categories </b>
To generate results of the categories model choose <b> Evaluate categories </b>

<H3>Blender scripts</H3>

+ The script LBS.py generates the bone and the mesh vertices of all the motions. 
+ The script Visualize_mesh.py gives us the opportunity to visualize all the genereted mesh.
+ The LBS.py is working only in the GenerateData.blend file
+ The Visualize_mesh.py working only in the OnlyVisualization.blend file 

<H2> Warning </H2>
The GenerateData.blend is not in this repository. you can find the file in the google drive at this link: https://drive.google.com/file/d/18ndAB3Q7Zwf4Ug8R6IousgCMdaCBQNik/view?usp=sharing

<H2> Requirements to run  </H2>
The versions of the libraries to run the code appears below 

![Screenshot (1474)](https://github.com/GiorgosDrog/diploma_thesis_motion_synthesis/assets/72260809/6b8c4538-7d4f-4379-96f8-82e8abee863e)

