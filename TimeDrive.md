---
title: TimeDrive
parent: Examples
has_children: false
nav_order: 1
---


# Driving for a set amount of time

```java

import edu.wpi.first.wpilibj.command.Command;
import frc.team670.robot.Robot;
import frc.team670.robot.utils.Logger;

/**
 * Driving forward for a specified amount of time
 */
public class TimeDrive extends Command {

	private double speed, seconds;

  public TimeDrive(double seconds, double speed) {
    this.speed = speed;
    this.seconds = seconds;
    requires(Robot.driveBase);
  }

  // Called just before this Command runs the first time
  @Override
  protected void initialize() {
    setTimeout(seconds);
    Logger.consoleLog("Speed: %s Seconds: %s", speed, seconds);
  }

  // Called repeatedly when this Command is scheduled to run
  @Override
  protected void execute() { 
    Logger.consoleLog();
    Robot.driveBase.tankDrive(speed, speed, false);
  }

  // Make this return true when this Command no longer needs to run execute()
  @Override
  protected boolean isFinished() {
    return isTimedOut();
  }

  // Called once after isFinished returns true
  @Override
  protected void end() {
    Robot.driveBase.stop();
    Logger.consoleLog("Speed: %s Seconds: %s", speed, seconds);
  }

  // Called when another command which requires one or more of the same
  // subsystems is scheduled to run
  @Override
  protected void interrupted() {
    end();
  }
}

```






