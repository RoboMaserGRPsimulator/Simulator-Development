# RoboMaster Power Rune Simulator

This is the team repository for COMP2043 UNNC Team 202201's project -- "Power Rune Simulator" .



## Technical Solutions

| Item                                   | Solution                                                     | Language                      |
| -------------------------------------- | ------------------------------------------------------------ | ----------------------------- |
| Simulator Enviroment | Unreal Engine 4.27(early) && 5.03(later)  | C++, C#, blueprint |
| Power Rune Detection | OpenVino, OpenCV | python,C++ |
| Operating Communication|  Unreal Plugin: ROS Integration   |  C++   |


## Simulator Developing Plan

 - 10/25 - 11/15 
    - Create vehicle control system and Shooting system
    - Import open source model into Unreal Engine
    - Realize Mecanum wheel and Power Rune rotation in Unreal.
 -  11/15 - 12/6
    - Build linux ue5 development environment
    - Use ROS Intergration Plugin for communication
    - Code testing on simulator
-   12/6 - 12/24
    -  Code tesing on real vehicle
    -  made ballistic dispersion effect on Unreal


## Technical issue

### 1. How to deploy our autoaim algorithm into simulator

Our vision code will be based on Opencv. How every, there is a conflit about "protobuf" file so that we failed to create an OpenCV plugin in the Unreal Engine.

After Reasearch, We planed to use ROS Intergration Plugin. 






