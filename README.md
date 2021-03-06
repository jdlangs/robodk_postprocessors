## RoboDK Post Processors
Open source vendor specific code generators/post processors from the RoboDK (https://robodk.com) application.

Post processors allow generating vendor specific programs from a generic/universal programming language. These post processors use a generic Python program to linked to a specific post processor (RobotPost class) that dictates how to generate the program.

# Dependency
Python must be installed.
The robodk.py is included with the post processors and is required by all post processors.
The robodk module includes useful tools for Pose multiplications and converting between poses and xyzwpr format.
More information here:
https://pypi.org/project/robodk/
https://github.com/RoboDK/RoboDK-API/tree/master/Python
https://robodk.com/doc/PythonAPI/robodk.html

## Example
# Generic code
This is an example of a pre-processed program that can be used to generate any vendor specific program.

```python
import sys
import os
sys.path.append(os.path.abspath("C:/RoboDK/Posts/")) # temporarily add the path to POSTS folder

# link to a specific post processor
from Universal_Robots import *

def Pose(xyzrpw):
    [x,y,z,r,p,w] = xyzrpw
    a = r*math.pi/180.0
    b = p*math.pi/180.0
    c = w*math.pi/180.0
    ca = math.cos(a)
    sa = math.sin(a)
    cb = math.cos(b)
    sb = math.sin(b)
    cc = math.cos(c)
    sc = math.sin(c)
    return Mat([[cb*ca, ca*sc*sb - cc*sa, sc*sa + cc*ca*sb, x],[cb*sa, cc*ca + sc*sb*sa, cc*sb*sa - ca*sc, y],[-sb, cb*sc, cc*cb, z],[0.0,0.0,0.0,1.0]])

print('Total instructions: 5')
robot = RobotPost(r'Universal_Robots', r'UR10', 6, axes_type=['R','R','R','R','R','R'])

robot.RunMessage(r'Program generated by RoboDK 3.1.3 for UR10 on 04/04/2017 09:08:52', True)
robot.RunMessage(r'Using nominal kinematics.', True)
robot.setFrame(Pose([334.378321, -457.798514, 0, 0, 0, 0]), 2, r'Frame 2')
robot.setTool(Pose([50, 0, 200, 0, 0, -30]), 1, r'Tool 1')
robot.MoveJ(None,[0, -90, -90, 0, 90, -0], None)
robot.MoveJ(Pose([635.659863, 119.697954, 472.379421, -90, -0, -164.143137]), [-9.12289, -104.73591, -87.09344, -32.67838, 96.53313, -6.38140], [0.0, 1.0, 0.0])
robot.MoveL(Pose([668.264687, 119.697954, 357.591360, -90, -0, -164.143137]), [-8.76522, -110.64538, -91.21248, -22.62172, 96.27799, -6.12915], [0.0, 1.0, 0.0])
robot.MoveL(Pose([668.264687, 389.011574, 357.591360, -90, 0, -164.143137]), [10.56808, -106.15774, -97.32175, -21.15363, 82.43731, 7.40331], [0.0, 1.0, 0.0])
robot.MoveJ(Pose([529.421679, 243.898514, 846.4, -90, -0, -120]), [0, -90, -90, 0, 90, -0], [0.0, 1.0, 0.0])
robot.ProgFinish(r'TestProgram')
robot.ProgSave(r'C:/Users/User1/Desktop/',r'TestProgram',False,'C:/Program Files (x86)/Notepad++/notepad++.exe')
```

# Result
The previous pre-processed program, linked against the Universal Robot post processor, generates the following SCRIPT code that can be executed by a Universal Robot controller.
```python
def TestProgram():
  # default parameters:
  global speed_ms    = 0.300
  global speed_rads  = 0.750
  global accel_mss   = 3.000
  global accel_radss = 1.200
  global blend_radius_m = 0.001
  # Program generated by RoboDK 3.1.3 for UR10 on 04/04/2017 09:12:17
  # Using nominal kinematics.
  # set_reference(p[0.334378, -0.457799, 0.000000, 0.000000, 0.000000, 0.000000])
  set_tcp(p[0.050000, 0.000000, 0.200000, -0.523599, 0.000000, 0.000000])
  movej([0.000000, -1.570796, -1.570796, 0.000000, 1.570796, -0.000000],accel_radss,speed_rads,0,0)
  movej([-0.159224, -1.827986, -1.520067, -0.570345, 1.684821, -0.111376],accel_radss,speed_rads,0,0)
  movel(p[1.002643, -0.338101, 0.357591, -2.073257, 2.073257, -0.288737],accel_mss,speed_ms,0,blend_radius_m)
  movel(p[1.002643, -0.068787, 0.357591, -2.073257, 2.073257, -0.288737],accel_mss,speed_ms,0,blend_radius_m)
  movej([0.000000, -1.570796, -1.570796, 0.000000, 1.570796, -0.000000],accel_radss,speed_rads,0,0)
end

TestProgram()
```

## Other
More information about RoboDK Post Processors here:
https://www.robodk.com/doc/PythonAPI/postprocessor.html
