# Tranferred Grasps

Our simulation was able to evaluate the object fall-off time for a large amount of generated grasps. The successful grasps of one gripper can represent successful grasps in others and increase the overall amount of successful grasps in the dataset. 
To test this hypothesis, we implemented the grasp transfer of successful grasps from one gripper to others and evaluated the transferred grasps using Isaac Sim.

We utilize an alignment between grippers to transfer grasps, which used a common notion of gripper pose (translation and orientation). The translation refers to the palm center of the gripper, and the orientation is with respect to a canonical pose of the gripper palm pointing in a fixed direction. Thus, any grasp pose from a gripper was transferred to another by using this pose alignment. We first transform a source gripper pose to its aligned pose, and then transform the aligned pose to the target gripper. 

<p align="center">
<img  src='https://github.com/IRVLUTD/isaac_sim_grasping/blob/f90c79a7d31b3d02773a72df13ec0cfee4cf9409/media/transfer_grasp.png' width='750'>
</p>

When transfering a grasp from a source gripper to a target gripper, the joint configuration of the source gripper cannot easily be mapped to the target gripper. To execute the new grasp, the target gripper is fully opened first and then closed using a new version of our position controller. The joint values at the moment of contact between the gripper and object are recorded, and the object fall-off time is simulated.

<p align="center">
<img src='https://github.com/IRVLUTD/isaac_sim_grasping/blob/fe181394075363c715c04f819bbd214a93794e25/media/transfer_close.gif' width='750'>
</p>

## Parameters and Inputs
The standalone executable for the tranferred grasp simulation is the transfer_st.py file; a command to run the simulation is shown below. Note: for Isaac Sim standalone executables, the commands must be run from the isaac sim python.sh directory. For this simulation Isaac Sim 2023.1.0 was used. Sample running command:


./python.sh (standalone folder)/transfer_st.py --json_dir=(json folder)/obj8 --gripper_dir=(repo directory)/grippers --objects_dir=(object directory) --output_dir=(output directory) --num_w=100 --test_time=3 --controller=position --headless --print_results


The standalone.py takes as input:
- json_dir: Grasp data directory (.json file)
- gripper_dir: Gripper directory (folder containing all the gripper .usd files)
- objects_dir: Object directory (folder containing all the object .usd files)
- output_dir: Output directory (directory to save the outputed .json file)
- num_w: Number of Workstations to run the simulation with (gripper, object pair) (default: 150)
- test_time: Total test time for each grasp test (default:6).
- controller: Controller reference (within controllers.py)
- (Optional) print_results: Verbosity of standalone after finishing one .json file.
- (Optional) headless: Run the simulation headless
- (Optional) force_reset: Reset the simulation forcefully by restarting Isaac Sim

Note: 
- To run the simulation without warnings add the following parameters to the command: 
 --/log/level=error --/log/fileLogLevel=error --/log/outputStreamLevel=error


<p align="center">
<img src='https://github.com/IRVLUTD/isaac_sim_grasping/blob/1b3484870d14923391dae18605cba453193ed566/media/transfer_far.gif' width='750'>
</p>