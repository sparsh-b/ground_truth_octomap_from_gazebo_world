# Generate Ground truth Octomap from Gazebo World
- This repository describes a method to generate a Ground Truth Octomap `.bt` file from a Gazebo `.world` file without the use of any robotic sensors & Mapping algorithms.
- The [OpenMACE]((https://github.com/heronsystems/OpenMACE) repository is used to achieve this task & the current repository presents only the relevant parts of OpenMACE in a structured way to make the task at hand easier.

## Configuration
- I tested the method on `Ubuntu 18.04` + `Gazebo 9.19.0` + `cmake 3.10.2` in WSL2 on Visual Studio Code on a Windows machine.
- But I believe my setup should be identical to an Ubuntu 18.04 machine running the same versions of the above software.

## Method
- Generating octomap from Gazebo world is acheived using a plugin built as part of [this](https://github.com/heronsystems/OpenMACE) repo.
- Check-out code from the tag pertaining to your Ubuntu version.
  - Example `git clone https://github.com/heronsystems/OpenMACE.git --branch=0.0.2` for Ubuntu 18.04.
  - Tag `0.0.1` corresponds to Ubuntu 16.04 & tag `v0.0.3` corresponds to Ubuntu 20.04.
  - With this clone, a catkin workspace also gets created locally at `<path-to-OpenMACE>/catkin_sim_environment` (this'll be relevant for one of the next steps).
- Build the repo using the instructions <a name="build-inst">[here](https://github.com/heronsystems/OpenMACE/wiki/Linux-Installation#-automated-build-steps)</a>
  - Look at [this](#errors) for a note on the errors you might encounter in this step.
- Install `tf2-sensor-msgs`
  - For melodic, `sudo apt-get install ros-melodic-tf2-sensor-msgs` works.
- Install `octomap-ros`
  - For melodic, `sudo apt-get install ros-melodic-octomap-ros` works.
- Go to `OpenMACE/catkin_sim_environment` local folder & build the catkin environment.
  - <a name="catkin-make-cmd">`cd <path-to-OpenMACE>/catkin_sim_environment && catkin_make`</a>
  - The plugin should be located at `<path-to-OpenMACE>/catkin_sim_environment/devel/lib/libBuildOctomapPlugin.so`
- Add the plugin into your desired gazebo `.world` file as shown [here](https://github.com/heronsystems/OpenMACE/wiki/Generate-Octomap-from-Gazebo-world#configuring-world-file).
- Run Gazebo with this modified `.world` file & call the octomap service as shown [here](https://github.com/heronsystems/OpenMACE/wiki/Generate-Octomap-from-Gazebo-world#calling-octomap-service)
  - While executing the `rosrun` command, set the `GAZEBO_PLUGIN_PATH` env variable to the directory containing the `libBuildOctomapPlugin.so` plugin so that Gazebo can find it.
  - While executing the `rosservice` command, source the `<path-to-catkin_sim_environment>/devel/setup.bash` file in the terminal in which you want to run `rosservice` command.


## Errors
- Facing a few errors (and not resolving them) was alright for me as long as the `libBuildOctomapPlugin.so` plugin got built after running [this](#catkin-make-cmd) `catkin_make` command.
  - While building the repo in [this](#build-inst) step, I came across the below warnings & errors but (at least) these didn't effect the plugin build.
  - ![err1](./imgs/Screenshot%20(312).png)
  - ![err1](./imgs/Screenshot%20(313).png)
  - ![err1](./imgs/Screenshot%20(314).png)


## Additional Links
### Here are a few more additional useful links
- [Related Discussion](https://github.com/heronsystems/OpenMACE/issues/12)