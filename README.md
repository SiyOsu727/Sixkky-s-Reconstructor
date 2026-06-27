# Tosu-Reconstructor
Tosu-Reconstructor is a modified OpenTabletDriver plugin that reconstructs pen input while applying a subtle aim assist. The assist is intentionally lightweight, providing smoother target acquisition and improved consistency without significantly altering the natural feel of tablet movement. Designed to enhance the user experience rather than automate aiming.

# How It Works
This plugin works by applying a small cursor offset (referred to as a nudge vector) to the pen position before it is processed by OpenTabletDriver. Instead of replacing the user's input, the plugin continuously adjusts the reported cursor position, providing subtle aim assistance while preserving natural hand movement.

-Cursor Offset
Every frame, a small correction vector is added to the current cursor position. The application receives the adjusted coordinates rather than the raw pen position.
For example, if the offset points 5 pixels to the right, the cursor will be reported as being 5 pixels further right than the physical pen position.
Unlike a temporary correction, this offset is persistent and carries over between frames, allowing smooth and consistent adjustments instead of abrupt snapping.

-Correction Strength

The amount of correction applied each frame is approximately calculated as:
Correction = Power × Closeness Bonus × min(Target Distance, Pen Movement)

Where:
Power controls the overall assist strength.
Closeness Bonus reduces correction when the cursor is already close to the target.
min(Target Distance, Pen Movement) prevents the plugin from correcting more than either:
the remaining distance to the target, or
the distance physically moved by the pen during the current frame.
This means that faster and more deliberate pen movement naturally allows proportionally stronger assistance while still remaining limited by user input.

-Power Scaling

The assist scales linearly with the configured Power value.
Examples:
Power	Effective Strength
0.40	28%
1.00	70%

-Closeness Reduction

When the adjusted cursor is already near the center of the target (approximately within 35% of its radius), the assist automatically reduces its strength to 65% of its normal value.
This prevents noticeable snapping and helps maintain smooth, natural cursor movement.

-Maximum Offset

To prevent excessive correction, the accumulated offset is limited to:
Maximum Offset = Power × 16 pixels
For example:

Power 0.40 → Maximum offset: 6.4 px
Power 0.70 → Maximum offset: 11.2 px
Power 1.00 → Maximum offset: 16 px

-Offset Recovery

When no valid target is available (such as between notes or outside the configured field of view), the accumulated offset is not removed instantly.
Instead, it gradually decays as the pen moves, avoiding sudden cursor jumps. The decay rate is proportional to the user's movement, allowing the cursor to smoothly return to its original position.

- Slider Behavior

The assist only affects the slider head.
No correction is applied while following the slider body, aiming the slider tail, or completing the slider end.

-Design Goals
Lightweight and natural feeling assistance.
Preserves manual control and pen movement.
Smooth corrections without visible snapping.
Configurable strength through the Power parameter.
Minimal latency and consistent behavior across frames.


# Installation
Download the modified Reconstructor.dll.
Close OpenTabletDriver if it is running.
Copy Reconstructor.dll into your OpenTabletDriver Plugins folder.
If the original Reconstructor plugin is already installed, remove it first to avoid compatibility issues.
Launch OpenTabletDriver.
Enable the plugin and configure the desired Power value.

Note: Only one version of Reconstructor.dll should be installed at a time. Running both the original and modified versions may cause conflicts.


