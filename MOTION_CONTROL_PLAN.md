# Motion Control Balancing Plan

This plan outlines the steps required to make the mobile motion control for the night ball feel responsive and intuitive while preventing overpowering or chaotic behavior.

## 1. Establish a Baseline for Current Behavior
- [ ] Add a temporary debug overlay that visualizes raw tilt (beta/gamma), filtered tilt, and the applied steering impulse so we can observe how current inputs map to ball movement.
- [ ] Log tilt deltas, steering impulses, and resulting ball velocity vectors to the console (behind a debug flag) to quantify the current gain and damping.
- [ ] Verify calibration flow on both iOS and Android to ensure baseline orientation is set exactly when the Interact button is pressed.

## 2. Rework Orientation Processing
- [ ] Split orientation tracking into three stages: calibration (baseline capture), filtering (low-pass to smooth jitter), and impulse detection (high-pass for quick tilts).
- [ ] Replace the single smoothing factor with a complementary filter that blends low-frequency drift removal with high-frequency responsiveness.
- [ ] Introduce adjustable sensitivity profiles (e.g., "subtle", "balanced", "aggressive") to fine-tune for different device sensor qualities during testing.

## 3. Map Tilt to Steering Force
- [ ] Convert filtered tilt angles to a normalized vector capped between -1 and 1 using a sigmoid curve so extreme tilts don't jump straight to full force.
- [ ] Apply easing that ramps in force over ~150 ms to avoid sudden lateral spikes yet still feel immediate.
- [ ] Add inverse damping proportional to the ball's current forward velocity to keep overall speed within bounds while allowing noticeable curvature.
- [ ] After applying steering, re-clamp the total speed and project the velocity vector to maintain consistent kinetic energy.

## 4. Preserve Player Intent While Avoiding Drift
- [ ] Keep the ball's forward momentum oriented toward its current travel direction; steer by rotating the velocity vector rather than simply adding to dx/dy.
- [ ] Reset the baseline automatically if the device remains steady for >2 seconds to prevent accumulation of offset drift.
- [ ] Provide an optional "hold to steer" mode where the Interact button becomes a press-and-hold trigger that temporarily increases sensitivity for precise shots.

## 5. Feedback & Accessibility
- [ ] Update the UI copy to explain the feature and display the current sensitivity mode.
- [ ] Show a subtle on-canvas arrow or glow on the night ball indicating the steering direction strength.
- [ ] Ensure non-mobile users never see the motion UI, and mobile users can disable it entirely.

## 6. Testing Strategy
- [ ] Test on at least one iOS and one Android device, covering Safari, Chrome, and Firefox where possible.
- [ ] Validate that small tilts (~5°) produce gentle arcs, medium tilts (~15°) create visible curvature, and extreme tilts (~30°+) result in near-max steering without losing control.
- [ ] Confirm recalibration works mid-game and that sensitivity adjustments are respected immediately.
- [ ] Gather subjective feedback from at least two testers to confirm the controls feel balanced.

## 7. Cleanup
- [ ] Remove temporary debug overlays and logs once tuning is complete.
- [ ] Document the control behavior in the README, including calibration instructions and recommended device posture.

Following this plan should deliver a motion-control experience that is responsive, predictable, and fun across both iOS and Android devices.
