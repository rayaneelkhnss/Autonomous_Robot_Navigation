# Autonomous_Robot_Navigation

## Présentation

Indoor real-time navigation for robot vehicles. 

Final year project for the specialization program in Embedded Systems Engineering (GSE) - Polytech'Nice Sophia 

The goal of the project is to design a real-time navigation system for a robot vehicle.

## Répertoires et fichiers du projet

• PC UBUNTU 20.04

	• catkin_ws
	
• RASPBERRY PI

	• catkin_ws
	
Projet_GSE5_2021_2022.pdf -> spécifications du projet

README.md -> fichiers du projet, description des dossiers, pré-requis, instructions de mise en route, tests et démos 

Rapport_Projet_GSE_ELKHANOUSSI_LENORMAND.pdf -> rapport final

## Pré-requis

Installer ROS Noetic sur votre ordinateur

Installer ROS Kinetic sur la Raspberry

## Enregistrement de la map avec Hector Slam

Configurons tout d’abord le port USB, pour cela branchez le LIDAR à votre ordinateur

Tapez la commande suivante :

	ls -l /dev | grep ttyUSB

Vous devriez avoir le port ttyUSB0 qui s’affiche

Modifiez les permissions de ce port à l’aide de la commande suivante :

	sudo chmod 666 /dev/ttyUSB0

Lancez le LIDAR :

	roslaunch rplidar_ros rplidar.launch

Lancez Hector SLAM via RVIZ :

	roslaunch hector_slam_launch tutorial.launch

Naviguez lentement dans la salle puis enregistrez la map créée :

	rosrun map_server map_saver -f my_map

Les fichiers my_map.yaml et my_map.pgm seront ainsi enregistrés pour la suite.

## Simulation avec Gazebo

Lancez les deux commandes suivantes pour lancer la simulation du robot sur Gazebo :

	export TURTLEBOT3_MODEL=burger

	roslaunch turtlebot3_gazebo turtlebot3_empty_world.launch
	
Lancez la commande suivante pour effectuer la navigation du robot simulé sous Rviz

	roslaunch turtlebot3_navigation turtlebot3_navigation.launch map_file:=$HOME/catkin_ws/map_finalb.yaml

## Navigation autonome du robot

Préalable : 

Connectez la Raspberry et le PC sur le même réseau

Récupérez l’adresse IP du PC avec la commande :
	
	ifconfig
	
Modifiez le fichier bashrc de votre PC à l’aide de la commande :

	gedit ~/.bashrc 
	
Indiquez l’adresse IP (171.20.10.8 dans notre cas) à l'aide des deux lignes suivantes à la fin du fichier bashrc :

	export ROS_MASTER_URI=http=//171.20.10.8:11311

	export ROS_HOSTNAME=171.20.10.8


Modifiez le fichier bashrc de votre Raspberry à l’aide de la commande :

	nano ~/.bashrc 
	
Ajoutez ces deux lignes à la fin du fichier bashrc (adresse IP du PC dans le ROS_MASTER et adresse IP de la Raspberry dans le ROS_HOSTNAME) :

	export ROS_MASTER_URI=http=//171.20.10.8:11311

	export ROS_HOSTNAME=171.20.10.6

Sur votre PC, lancez la commande suivante dans un nouveau terminal pour assurer la communication entre le PC et la Raspberry :
	
	roscore

Sur la Raspberry, lancez la commande suivante pour pouvoir utiliser les applications TurtleBot3 :

	roslaunch turtlebot3_bringup turtlebot3_robot.launch

Sur votre PC, dans un nouveau terminal tapez les deux commandes suivantes pour lancer la navigation temps réel sur Rviz (avec la map créée précédemment) : 

	export TURTLEBOT3_MODEL=burger

	roslaunch turtlebot3_navigation turtlebot3_navigation.launch map_file:=$HOME/catkin_ws/map_finalb.yaml

Sur RVIZ cliquez tout d’abord sur 2D Position Estimate pour indiquer où est le robot sur la map ainsi que son orientation.

Puis cliquez sur Set Navigation Goal pour définir le point de destination souhaité du robot.

Vous n’avez plus qu’à admirer le déplacement du robot!

