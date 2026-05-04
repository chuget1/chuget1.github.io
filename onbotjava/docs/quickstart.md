### chuget1.github.io
## Quick Start Guide for OnBotJava
### Most Important Functions:
`.setPower(x);`: Sets motor power to x% of total power as a decimal  
`.setVelocity(x);`: Sets motor velocity to x ticks per second  
`.setDirection(x);` Sets motor logical direction  
`.setPosition(x);`: Sets servo to x% of servo's maximum angle as a decimal (0.5 is midpoint)  
### Basic Drive Code
```java
package org.firstinspires.ftc.teamcode.drive.opmode;

import com.qualcomm.robotcore.eventloop.opmode.TeleOp;
import com.qualcomm.robotcore.hardware.DcMotor;
import com.qualcomm.robotcore.robot.Robot;
import com.qualcomm.robotcore.eventloop.opmode.LinearOpMode;
import com.qualcomm.robotcore.hardware.DcMotorEx;
import com.qualcomm.robotcore.hardware.DcMotorSimple;


@TeleOp() // Tele-operated, operated by human

public class OpModeBasic extends LinearOpMode {
    // Declare motor variables
    public DcMotorEx topLeft;
    public DcMotorEx topRight;
    public DcMotorEx bottomLeft;
    public DcMotorEx bottomRight;

    public void runOpMode() {
        // Initialize motor variables (map physical motor to variable)
        topLeft = hardwareMap.get(DcMotorEx.class, "topLeft");
        topRight = hardwareMap.get(DcMotorEx.class, "topRight");
        bottomLeft = hardwareMap.get(DcMotorEx.class, "bottomLeft");
        bottomRight = hardwareMap.get(DcMotorEx.class, "bottomRight");

        // topRight.setDirection(DcMotorSimple.Direction.REVERSE);
        // bottomRight.setDirection(DcMotorSimple.Direction.REVERSE);
        // Set wheels to brake when not receiving power
        topLeft.setZeroPowerBehavior(DcMotorEx.ZeroPowerBehavior.BRAKE);
        topRight.setZeroPowerBehavior(DcMotorEx.ZeroPowerBehavior.BRAKE);
        bottomLeft.setZeroPowerBehavior(DcMotorEx.ZeroPowerBehavior.BRAKE);
        bottomRight.setZeroPowerBehavior(DcMotorEx.ZeroPowerBehavior.BRAKE);
        
        telemetry.addData("Hardware", "initialized"); // Add text to be printed to driver hub on next update
        telemetry.update(); // Print to driver hub, confirms initialization is complete
        
        // Driver hub emergency stop
        if (isStopRequested()) return;
        // topLeft.setZeroPowerBehavior(DcMotorEx.ZeroPowerBehavior.BRAKE);
        // topRight.setZeroPowerBehavior(DcMotorEx.ZeroPowerBehavior.BRAKE);
        // bottomLeft.setZeroPowerBehavior(DcMotorEx.ZeroPowerBehavior.BRAKE);
        // bottomRight.setZeroPowerBehavior(DcMotorEx.ZeroPowerBehavior.BRAKE);

        telemetry.addData("software", "Waiting for start");
        telemetry.update();
        waitForStart(); // Waits for start on driver hub
        telemetry.addData("software", "Start!!");
        telemetry.update();
        
        while(opModeIsActive()) { // Runs when started on driver hub, acts as while true loop
            // Map controller joystick positions for player 1 and 2 to variables
            double LY1 = gamepad1.left_stick_y;
            double LX1 = gamepad1.left_stick_x;
            double LT1 = gamepad1.left_trigger;
            double RX1 = gamepad1.right_stick_x;
            double RY1 = gamepad1.right_stick_y;
            double RT1 = gamepad1.right_trigger;
            double LY2 = gamepad2.left_stick_y;
            double LX2 = gamepad2.left_stick_x;
            double LT2 = gamepad2.left_trigger;
            double RX2 = gamepad2.right_stick_x;
            double RY2 = gamepad2.right_stick_y;
            double RT2 = gamepad2.right_trigger;
            

            // Default and boost speed modifiers
            double BoostSpeed = 1;
            double DriveSpeed = 0.5;
            

            // Power variables for each wheel
            // !!!NOTE!!! THIS REQUIRES TINKERING AS EVERY ROBOT'S MOTORS ARE INSTALLED DIFFERENTLY
            double topLeftPower = (LY1 - LX1) - RX1;
            double bottomLeftPower = (LY1 + LX1) - RX1;
            double topRightPower = (LY1 + LX1) + RX1;
            double bottomRightPower = (LY1 - LX1) + RX1;
            // Boost when left bumper is held down
            if(gamepad1.left_bumper) {
                topLeft.setPower(topLeftPower * BoostSpeed);
                bottomLeft.setPower(bottomLeftPower * BoostSpeed);
                topRight.setPower(topRightPower * BoostSpeed);
                bottomRight.setPower(bottomRightPower * BoostSpeed);
            }
            // Drive normal when left bumper is not held
            else {
                topLeft.setPower(topLeftPower * DriveSpeed);
                bottomLeft.setPower(bottomLeftPower * DriveSpeed);
                topRight.setPower(topRightPower * DriveSpeed);
                bottomRight.setPower(bottomRightPower * DriveSpeed);
            }

            // Output motor power values to driver hub
            telemetry.addData("topLeftPower", topLeftPower);
            telemetry.addData("topRightPower", topRightPower);
            telemetry.addData("bottomLeftPower", bottomLeftPower);
            telemetry.addData("bottomRightPower", bottomRightPower);
            telemetry.update();
        }
    }
}
```
