---
title: BalancedDrive
parent: Examples
has_children: false
nav_order: 1
---

# Using encoder data to balance motors
## Exercise from Oct. 25, 2019 workshop

A common issue one faces when working with drivebase motors is that motors vary, so they may not turn at the same rate.
Without accounting for this when controlling, the robot will veer off course. We can use encoder data to try 
to deal with this issue.

## Selected Student Solutions

### Solution by Ruchi Dixit


```java
package frc.team670.robot.commands.drive;

import edu.wpi.first.wpilibj.command.Command;
import frc.team670.robot.Robot;
import frc.team670.robot.utils.Logger;

public class BalancedDrive extends Command {

	private double speedL, speedR, seconds;

	public BalancedDrive(double seconds, double lspeed, double rspeed) {
		this.speedL = lspeed;
		this.speedR = rspeed;
		this.seconds = seconds;
		requires(Robot.driveBase);
	}

	// Called just before this Command runs the first time
	@Override
	protected void initialize() {
		setTimeout(seconds);
		Logger.consoleLog("Left Speed: %s Right Speed: %s Seconds: %s", speedL, speedR, seconds);

	}
	
	public void correct() {
		double currentTicksL = Robot.driveBase.getLeftEncoder().getTicks();
		double currentTicksR = Robot.driveBase.getRightEncoder().getTicks();
		if (Math.abs(currentTicksL - currentTicksR) < 5)
			return;
		else if (currentTicksL > currentTicksR)
				speedL -= 0.01;
		else if (currentTicksL < currentTicksR)
				speedR -= 0.01;
	}
	
	// Called repeatedly when this Command is scheduled to run
	@Override
	protected void execute() {
		Logger.consoleLog();
		Robot.driveBase.tankDrive(speedL, speedR, false);
		correct();
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
		Logger.consoleLog("Left Speed: %s Right Speed: %s Seconds: %s", speedL, speedR, seconds);
	}

	// Called when another command which requires one or more of the same
	// subsystems is scheduled to run
	@Override
	protected void interrupted() {
		end();
	}

}
```


### Solution by Eddie Li and Megan Choy


```java
import edu.wpi.first.wpilibj.command.Command;
import frc.team670.robot.Robot;
import frc.team670.robot.utils.Logger;

/**
 * Driving forward for a specified amount of time
 */
public class BalancedDrive extends Command {

	private double seconds;
	private double leftSpeed, rightSpeed, originalSpeed;

	public BalancedDrive(double seconds, double speed) {
		this.leftSpeed = speed;
		this.rightSpeed = speed;
		originalSpeed = speed;
		this.seconds = seconds;
		requires(Robot.driveBase);
	}

	// Called just before this Command runs the first time
	@Override
	protected void initialize() {
		setTimeout(seconds);
	}

	// Called repeatedly when this Command is scheduled to run
	@Override
	protected void execute() {
		Logger.consoleLog();

		double leftTicks = Robot.driveBase.getLeftEncoder().getTicks();
		double rightTicks = Robot.driveBase.getRightEncoder().getTicks();

		if (leftTicks > 20) {
			double ratio = rightTicks / leftTicks;
			rightSpeed = originalSpeed / ratio;
		}

		Robot.driveBase.tankDrive(leftSpeed, rightSpeed, false);

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
	}

	// Called when another command which requires one or more of the same
	// subsystems is scheduled to run
	@Override
	protected void interrupted() {
		end();
	}
}
```

### Solution by Justin Hwang


```java
package frc.team670.robot.commands.drive;

import edu.wpi.first.wpilibj.command.Command;
import frc.team670.robot.Robot;
import frc.team670.robot.utils.Logger;
import frc.team670.pi.sensors.*;

public class BalancedDrive extends Command {

	private double speed, seconds;
	private Encoder leftEncoder, rightEncoder;
	private int leftTickCount, rightTickCount;
	private double time;

	public BalancedDrive(double time, double speed) {
		this.speed = speed;
		this.time = time;

		requires(Robot.driveBase);

		leftEncoder = Robot.driveBase.getLeftEncoder();
		rightEncoder = Robot.driveBase.getRightEncoder();

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
		leftTickCount = leftEncoder.getTicks();
		rightTickCount = rightEncoder.getTicks();

		if (leftTickCount < rightTickCount) {
			Robot.driveBase.tankDrive(speed, speed * 0.9, false);
		} else if (rightTickCount < leftTickCount) {
			Robot.driveBase.tankDrive(speed * 0.9, speed, false);
		} else {
			Robot.driveBase.tankDrive(speed, speed, false);
		}

		Logger.consoleLog();

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
