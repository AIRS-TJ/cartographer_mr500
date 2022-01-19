# cartographer_m500

Cartographer configurations and launch files for mr500 robots.

下面用到的rosbag都在 /Workspace/Dataset/m500 上

${HOME}是绝对路径

#  Usage

**编译:**

	$ cd ~/catkin_ws/

	$ catkin_make_isolated --install --use-ninja 

注意：任何文件只要修改了内容就需要重新编译，即使修改的launch文件或配置都需要重新编译

**运行之前souce环境变量：**

	$ source install_isolated/setup.bash

**建图加定位模式：**

	$ roslaunch cartographer_mr500 demo_mr500_3d.launch bag_filename:=${HOME}/mr500_experiment_bag/2021-05-21-15-14-09.bag


**纯定位模式：**

参考：https://google-cartographer-ros.readthedocs.io/en/latest/demos.html

	$ roslaunch cartographer_mr500 offline_mr500_3d.launch bag_filenames:=${HOME}/mr500_experiment_bag/2021-05-21-15-14-09.bag

等待以上命令的节点运行完毕，并生成.pbstream，再运行以下命令

用同一个包测试定位：

	$ roslaunch cartographer_mr500 demo_mr500_3d_localization.launch load_state_filename:=${HOME}/mr500_experiment_bag/2021-05-21-15-14-09.bag.pbstream bag_filename:=${HOME}/mr500_experiment_bag/2021-05-21-15-14-09.bag
	
或者用不同的包测试定位：

	$ roslaunch cartographer_mr500 demo_mr500_3d_localization.launch load_state_filename:=${HOME}/mr500_experiment_bag/2021-05-21-15-14-09.bag.pbstream bag_filename:=${HOME}/mr500_experiment_bag/2021-05-21-15-03-45.bag
	
**pbstream to pcd ：**

	$ roslaunch cartographer_mr500 offline_mr500_3d.launch bag_filenames:=${HOME}/mr500_experiment_bag/2021-11-29-15-26-33.bag

等待以上命令的节点运行完毕，并生成.pbstream，再运行以下命令
	
	$ roslaunch cartographer_mr500 assets_writer_mr500_3d.launch bag_filenames:=${HOME}/mr500_experiment_bag/2021-11-29-15-26-33.bag pose_graph_filename:=${HOME}/mr500_experiment_bag/2021-11-29-15-26-33.bag.pbstream

	$ pcl_viewer 2021-11-29-15-26-33.bag_points.pcd







