# swerve-2023
The project for new programmers to design the new robot chassis using swerve drive.


## For Programmers:
Don't worry about this step-by-step guide, especially if you are stressed by seeing this. If so, all you really have to do is copy and paste the swerve code from Rowdy23 into a new branch on this project so that it works. For reference, take a look at the ez500-dev branch (it is important to note that this branch has NOT been tested as of August 1, 2023).

Estimated time to finish branch:

  - Legimiately + debug: ~3 months

  - Copy-paste from Rowdy23 + debug: ~1 week - 1 month

### Given in the project:
  - Project Template
  - Extra libraries for Phoenix, CANCoder, REVRobotics
  - Extra util libraries for CANCoders and CTREModules (i.e. swerve modules)

To build a swerve drive train, these are the components needed:
  1. Constants.java

      1a. PIDs and offsets (obtained by testing the robot using CANCoder test kits e.g. Phoenix5)
  
      1b. IDs of the motor controllers & encoders (obtained by setting up the hardware of the robot)
  
      *1c. max speed/angular velocity, deadband*
    
  2. subsystems/SwerveModule.java (defined by a motor/angle controller/encoder, its ID/port, PID, etc.)

      IMPORTANT:   *CTREConfigs is required. Copy and paste from Rowdy23.*

      2a. defines a module as a motor & angle controller with corresponding IDs and connected to the same unit
      
      2c. configures its ID, PID, and offsets (from Constants.java)
      
      2b. configures its drive/angle motor/encoder (from its ID)
      
      2d. contains methods including:
        - getter methods for angle, relative position, and absolution position
        - actually configuring/initializing the motors & encoders (including PID)
        - setting its speed, rotation, "state" (speed & rotation).
          - setting its "state" would be most used
          - setting its speed, and thus also setting its "state", would require whether its open loop (i.e. it moves in the calculated direction independent from its rotation)
  
  2. subsystems/Swerve.java Subsystem

      3a. uses four swerve modules
      
      3b. integrates other swerve constants (e.g. wheel unit measurements, translations of the kinematics of such wheels, etc.)
      
      3c. declares/defines methods to directly control all four motor controllers to drive, strafe, etc. using translations and setting each module "state" (as stated in 2d)
        - to drive, requires if it is field relative (driving relative to field or robot), and if it is open loop (using holonomic rotation or not, meaning if the rotation is independent of its driving direction)

       3d. integrates pigeon sensor for yaw, pose, odometry, and using all four modules to drive/rotate together
  3. Swerve Command (e.g. "commands/TeleopSwerve") (since this project uses a command-based framework, a swerve command for Teleop is needed in order to move the robot upon a trigger on a joystick controller)
      
      4a. Uses an instance of the swerve subsystem to drive the robot based on the translation of where the joystick is
      
      4b. It is given the joystick's location relative to its origin and converts it to a double representation of translation, strafe, and rotational values.
      
      4c. apply deadband, speed caps
  
  4. Updating RobotContainer.java
     
      5a. instance of swerve to set its default command (and also for its methods)
      
      5b. driver joystick & axis controllers (read up on docs) and name certain axes how they would be used to control the robot (i.e. translationAxis for the direct movement up/down of the robot)
        - Hint; need a translation axis, strafe axis (on the same joystick), and rotation axis (another joystick to control rotation of the robot)
      
      5c. Create an instance of the command in the RobotContainer constructor by setting the default command of the subsystem to a new instance of the TeleopSwerve command that takes in all three axes
      
      5d. (OPTIONAL) Create button bindings for zeroing the gyro (using yaw), autobalancing (creating a target, PID controller, and pitch using pigeon sensors), and a robotcentric toggle/hold-to-enable, and add that as an argument to the swerve command to determine whether to use robot centric driving or not.
