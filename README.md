Group 4
Kevin Han - 1003488870
Katrina Cecco - 1003145615
Anh Kiet Nguyen - 1002901210
Mariem Ahmed - 1003269688
Wenhan Jiang - 1003065274

Contest 3 - Finding and Interacting with Emotional People in an Unknown Environment

How the code runs:
1. Open 8 terminator windows on Ubuntu inside the catkin_ws folder from the submission package.
	cd catkin_ws/
	Then split into eight more terminal windows

2. Run the following command in each of the eight windows:
	source devel/setup.bash

3. In the 1st window, run the following command to compile the code package:
	catkin_make

4. In the 2nd window, run the following commands to launch Gazebo:
	conda deactivate
	roslaunch mie443_contest3 turtlebot_world.launch world:=practice

5. In the 3rd window, run the following commands to run soundplay_node.py to play audio while running contest 3 code: 
	conda deactivate
	cd src/mie443_contest3/src
	rosrun sound_play soundplay_node.py

6. In the 4th window, run the following commands to implement the victim localization: 
	conda activate mie443
	cd src/mie443_contest3/src
	python victimLocator.py

7. In the 5th window, run the following commands to run GMapping: 
	conda activate base
	roslaunch mie443_contest3 gmapping.launch

8. In the 6th window, run the following command to see the visualization of the algorithm:
	conda activate base
	roslaunch turtlebot_rviz_launchers view_navigation.launch

9. In the 7th window, run the following commands to run the emotion classification utilizing our trained CNN model mdl_best.pth:
	conda activate mie443
	cd src/mie443_contest3/src
	python emotionClassifier.py

10. In the 8th window, run the following command to run the contest3 file and initiate the simulation: 
	conda activate base
	roslaunch mie443_contest3 contest3.launch

11. Once the simulation is finished, CNN_result.txt file is saved in the following directory:
	/home/turtlebot/catkin_ws/src/mie443_contest3/CNN_result.txt

***
Notes:
- To prevent an issue of sound files cutting off after 10 seconds, soundplay_node.py needs to be adjusted using 'sudo gedit' to change lines 185-186 from:
	position = self.sound.query_position(Gst.Format.TIME)[0]
	duration = self.sound.query_duration(Gst.Format.TIME)[0]
  to:
	position = self.sound.query_position(Gst.Format.TIME)[1]
	duration = self.sound.query_duration(Gst.Format.TIME)[1]

- There are three modes of robot responses in this implementation: sound, image, and robot movements. Make sure to zoom into the robot in Gazebo to see certain little movements that may be hard to see when zoomed out.

- emotionClassifier.py has been changed to publish to a topic where the contest3.cpp can sense when an emotion is detected and performs/shows appropriate robot response movements, sounds, and images.

- explore.cpp has been changed to publish to a topic where contest3.cpp can sense when the frontier exploration has stopped, and if it is near the beginning of the exploration (has not found any victims), it treats the frontier algorithm as stuck (common error discussed in piazza) and does a small circle around the spot to scan more of the environment until frontier exploration gains momentum and enough map scan where it won't run into any more of the issues near the start.

- emotionTrainingSample.py has our optimized CNN training algorithm with various strategies explored as explained in the report.
Lines 78, 80, and 200 contain addresses to directories we used in Google Colab training. They must be updated accordingly by the TA in order to run the training code on another system.

- mdl_best.pth is the optimized model trained by emotionTrainingSample.py

