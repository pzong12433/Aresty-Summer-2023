https://deeplabcut.github.io/DeepLabCut/docs/standardDeepLabCut_UserGuide.html#i-novel-video-analysis

To open DLC:

1. Search for and open Anaconda Prompt in Windows search bar. 

2. Type "activate deeplabcut" 

3. Type "ipython" 

4. Type "import deeplabcut" 

To start a new project:

1. deeplabcut.create_new_project('name_of_project', 'name_of_user', [r'path_of_video(s)']) 
(I would save it to the desktop for easy access) (in the config.yaml file edit the bodyparts, frames to pick, and augmenter type to whatever you need)

2. deeplabcut.add_new_videos(r'path_of_config.yaml', [r'path_of_video(s)'])
(You can right click a file and an option to copy path will appear) (you have to delete the quotation marks)

3. deeplabcut.extract_frames(r'path_of_config.yaml', mode='automatic', algo='kmeans', userfeedback=True, crop=True) 

4. deeplabcut.label_frames(r'path_of_config.yaml')
(A GUI called napari will pop up and you have to drag a folder in "Labled Data" into it) (label bodyparts consistently)

5. deeplabcut.check_labels(r'path_of_config.yaml', visualizeindividuals=True)
(optional)

Training the model:

1. deeplabcut.create_training_dataset(r'path_of_config.yaml', augmenter_type='imgaug')
(edit the training pose_cfg file in the "dlc-models" folder) (change display iterations to 50000) 
(for multistep, keep 
- - 0.005
  - 10000
- - 0.02
  - 430000) delete the other 2. (you can set the 430000 to anything between ~200000-400000)
(if you are retraining, set the initial weights to the path of the latest snapshot)

2. deeplabcut.train_network(r'path_of_config.yaml')
(For a batch size of 8, every 50k iterations is around 1 hour and 15 minutes eg. 200k iterations will take 5 hours)

3. deeplabcut.evaluate_network(r'path_of_config.yaml',Shuffles=[1], plotting=True)

Refining the model:

1. deeplabcut.analyze_videos(r'path_of_config.yaml',[r'path_of_videos'], save_as_csv=True)
(will take approximately 1.5 the length of the video eg.1 hour long video will take 1.5 hours to analyze)

2. deeplabcut.filterpredictions(r'path_of_config.yaml',[r'path_of_videos'])

3. deeplabcut.plot_trajectories(r'path_of_config.yaml',[r'path_of_videos'])
(will create a folder called plot poses in "Videos" folder

4. deeplabcut.create_labeled_video(r'path_of_config.yaml',[r'path_of_videos'], videotype='.mp4', filtered=True)
(will take approximately 0.25 length of video eg.1 hour long video will take 15 minutes to create a labeled video)

5. deeplabcut.extract_outlier_frames(r'path_of_config.yaml', [r'path_of_specific_video'], outlieralgorithm='jump')
(if the number of outlier frames is more than 10k, then change outlieralgorithm to manual)
(this will open up a GUI in napari, if you are retraining, delete the previous machine-iterations.h5 file before extracting frames)

6. deeplabcut.refine_labels(r'path_of_config.yaml')
(be consistent and careful when refining the labels)

7. deeplabcut.merge_datasets(r'path_of_config.yaml')
(can only be done when all analyzed videos have been refined)

8. retrain the model by creating a new dataset
(check the DLC commands file for examples of commands to run)




