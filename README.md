# ROS Norbit Multibeam sonar driver

This driver is designed interface directly with a Norbit sonar compatible with the [Norbit DFD](https://raw.githubusercontent.com/uri-ocean-robotics/norbit/master/norbit/doc/TN-180196-1D-WBMS_DFD_External.pdf?token=GHSAT0AAAAAABZI2NASRGSDYGYNFR7TZINCY2IJH6Q)

![image](docs/media/norbit_demo.gif)

## Installation ##

This project can be installed like any other ROS1 source package

### Begin by getting the package the dependencies ###

First clone the package to your ros workspaces source directory
```
git clone https://github.com/uri-ocean-robotics/norbit.git
```

This package uses the hydrographic_msgs package. Clone them to your workspace's src directory

```
git clone https://github.com/apl-ocean-engineering/hydrographic_msgs
```

The rest of the dependencies can be acquired through rosdep.   Run the follwing command from your workspaces root directory.
```
rosdep install --from-paths src --ignore-src -r -y
```

### Compile the package ###
from your catkin workspaces root directory run
```
catkin_make
```

## Running the package ##

An example launch file can be found in the norbit/launch directory.   An example parameter configuration yaml with internal documentation describing all parameters can be found in the norbit/config directory.   

To run the norbit with the parameters speified in the aforementioned yaml config file simply run
```
roslaunch norbit norbit.launch ip:=<your unit's ip address>
```

## Additional Utilities ##

There are a few other packages associated with the [hydrographic_msgs](https://github.com/apl-ocean-engineering/hydrographic_msgs) standards that may be useful for processing and visualizing data generated by this driver

**[Acoustic Msgs Tools](https://github.com/k2oceanic/acoustic_msgs_tools)** Is a collection of tools to view and interpret messages in the [hydrographic_msgs](https://github.com/apl-ocean-engineering/hydrographic_msgs) package.
![wc_gui2-2022-02-22_15 21 30](https://user-images.githubusercontent.com/23006525/155218007-6cd1ff3e-8736-47ba-ba1b-50a0fee31345.gif)

**[Watercolumn Rviz Plugin](https://github.com/rolker/rviz_sonar_image)** is a plugin that allows you to visualize the norbit watercolumn in RViz
![image](https://user-images.githubusercontent.com/23006525/195664892-8db3c42e-3afb-4a89-9e61-84f3f9fd1ff8.png)

## Parameters ##

below you can find a sample norbit configuration with comments describing each parameter.

for more detials on the startup settings parameter refer section 7 in the [Norbit DFD](https://raw.githubusercontent.com/uri-ocean-robotics/norbit/master/norbit/doc/TN-180196-1D-WBMS_DFD_External.pdf?token=GHSAT0AAAAAABZI2NATU3KXJNE6RCIBDIQYY2IKYOQ)

```
ip:                 "10.1.10.61"            # the IP addres of your norbit
cmd_port:           2209                    # the port the norbit listens for commands. normally/default 2209
bathy_port:         2210                    # the port which bathymetry is published. normally/default 2210
sensor_frame:       "norbit"                # the TF frame of the norbit
pointcloud_topic:   "cloud"                 # the topic you want to send your pointclouds on
bathymetric_topic:  "bathymetric"           # norbit_msgs raw bathymetric topic
detections_topic:   "detections"            # hydrographic_msgs detection topic

# watercolumn stuff If both are left blank WC data will not be requested from the unit
norbit_watercolumn_topic: "watercolumn_raw" # the norbit_msgs watercolum message topic.  not published if blank.
watercolumn_topic:  "watercolumn"           # the hydrographic_msgs watercolum topic.  not published if blank.
cmd_timeout:        0.5                     # how long should you wait for a command response before you consider it timed out
startup_settings:                           # the norbit settings to be applied at startup (see doc/*DFD_external.pdf)
    set_power:        "1"
    set_gate_mode:    "1"
    set_range:        ".5 30"
    set_time_source:  "1"                   # 0: IRIG-B 1:NTP 2: IRIG-B (inverted) 3: NTP+PPS(pos) 4: 
                                            # NTP+PPS(neg) 5: ZDA+PPS(pos) 6:ZDA+PPS(neg) 
                                            # 7:ZDA 8:Free run (time since startup)
    set_ntp_server:   "timepi.aura"

shutdown_settings:
    set_power:      "0"
```

## Issuing Norbit commands via ROS services ##

Comands can be via a rosservice call.  This grants control of the unit without the Norbit GUI.  These services can also be called from other ROS nodes such as mission planners.

You can also call these commands from the terminal using the ROS service command line utility.  See the example below that tells the unit to start pinging:

```
rosservice call <norbit node name including namespace> "cmd: 'set_power'
val: '1'" 

```

For a list of available commands reffer to section 7 of the [Norbit DFD](https://raw.githubusercontent.com/uri-ocean-robotics/norbit/master/norbit/doc/TN-180196-1D-WBMS_DFD_External.pdf?token=GHSAT0AAAAAABZI2NATU3KXJNE6RCIBDIQYY2IKYOQ)


## Contributing ##

Thank you for considering a contribution to this package.   Please review our [CONTRIBUTING](CONTRIBUTING.md) guidelines to get started.
