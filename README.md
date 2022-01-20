# cartographer_mr500

Cartographer configurations and launch files for mr500 robots.

下面用到的rosbag都在 /Workspace/Dataset/m500 上

${PATH_BAG}是绝对路径

#  Usage

**编译:**

	$ cd ~/catkin_ws/

	$ catkin_make_isolated --install --use-ninja 

注意：任何文件只要修改了内容就需要重新编译，即使修改的launch文件或配置都需要重新编译

**运行之前souce环境变量：**

	$ source install_isolated/setup.bash

**建图加定位模式：**

	$ roslaunch cartographer_mr500 demo_mr500_3d.launch bag_filename:=${PATH_BAG}/2021-05-21-15-14-09.bag

**生成pbstream：**

	$ roslaunch cartographer_mr500 offline_mr500_3d.launch bag_filenames:=${PATH_BAG}/2021-05-21-15-14-09.bag
	
等待以上命令的节点运行完毕，自动生成.pbstream
	
若想随时生成pbstream：
	
	$ rosservice call /write_state 用tab键补全后面的参数 "filename: '${PATH_BAG}/2021-05-21-15-14-09.bag.pbstream' 和 include_unfinished_submaps: true" 


**纯定位模式：**

参考：https://google-cartographer-ros.readthedocs.io/en/latest/demos.html

用同一个包测试定位：

	$ roslaunch cartographer_mr500 demo_mr500_3d_localization.launch load_state_filename:=${PATH_BAG}/2021-05-21-15-14-09.bag.pbstream bag_filename:=${HOME}/mr500_experiment_bag/2021-05-21-15-14-09.bag
	
或者用不同的包测试定位：

	$ roslaunch cartographer_mr500 demo_mr500_3d_localization.launch load_state_filename:=${PATH_BAG}/2021-05-21-15-14-09.bag.pbstream bag_filename:=${HOME}/mr500_experiment_bag/2021-05-21-15-03-45.bag
	
**pbstream 生成 pcd：**
	
	$ roslaunch cartographer_mr500 assets_writer_mr500_3d.launch bag_filenames:=${PATH_BAG}/2021-11-29-15-26-33.bag pose_graph_filename:=${PATH_BAG}/2021-11-29-15-26-33.bag.pbstream

	$ pcl_viewer 2021-11-29-15-26-33.bag_points.pcd
	
**pbstream 生产 ros标准格式地图（.yaml+.pgm）：**

	$ roslaunch cartographer_mr500 assets_writer_mr500_3d.launch bag_filenames:=${PATH_BAG}/2021-11-29-15-26-33.bag pose_graph_filename:=${PATH_BAG}/2021-11-29-15-26-33.bag.pbstream
	
或者：

	$ rosrun cartographer_ros cartographer_pbstream_to_ros_map -map_filestem=${PATH_BAG}/2021-11-29-15-26-33.bag -pbstream_filename=${PATH_BAG}/2021-11-29-15-26-33.bag.pbstream -resolution=0.05
	
注意：这两种命令生成的地图是不一样的！！！
