# Bouncing Ball Simulation with Spinning Arc

This project is a simple 2D physics simulation of bouncing balls within a circular boundary, featuring a "Pac-Man-like" spinning arc that allows balls to escape. It's built using Pygame.

## Core Features

* **Ball Dynamics**: Balls are initialized with a position and velocity. They are affected by gravity, causing them to accelerate downwards.
* **Circular Boundary Collision**: Balls bounce off the inner walls of a large orange circle. Collision detection is based on the distance from the ball's center to the circle's center, and the reflection logic inverts the velocity component normal to the circle's surface while also incorporating the effect of the circle's spin on the ball's velocity.
* **Spinning Arc Escape**: A black arc, defined by a start and end angle, spins around the circle's center. If a ball collides with the circle's boundary *outside* this arc (meaning `is_ball_in_arc` returns false for the arc's opening, and `ball.is_in` remains true), it bounces. If the collision point is *within* the spinning arc's opening, the ball is marked as `is_in = False` and it effectively passes through that segment, eventually leaving the main circle.
* **Ball Regeneration**: When a ball goes off-screen (outside the `WIDTH` or `HEIGHT` of the window), it is removed, and two new balls are generated at a starting position near the top-center of the circle with randomized horizontal and vertical velocities.
* **Visuals**: The simulation displays an orange circle as the boundary, the spinning black arc representing an opening, and multiple balls with randomly assigned colors bouncing within the window.

## How it Works

The simulation initializes Pygame and sets up the display window. A list of `Ball` objects is maintained. In the main loop:
1.  Events are handled (e.g., quitting the game).
2.  The `start_angle` and `end_angle` of the escape arc are incremented by `spinning_speed`, causing it to rotate.
3.  For each ball:
    * Gravity is applied to its vertical velocity.
    * Its position is updated based on its velocity.
    * Boundary checks are performed; if a ball is off-screen, it's replaced by two new ones.
    * Collision with the main circle is checked. If a collision occurs:
        * The `is_ball_in_arc` function determines if the collision point is within the opening of the spinning arc.
        * If not in the arc and still marked as `is_in = True`, the ball's position is adjusted to be just inside the boundary, and its velocity is updated to simulate a bounce, including an added component from the arc's spin.
        * If the collision is within the arc, the ball's `is_in` flag is set to `False`, allowing it to "escape" without bouncing.
4.  The screen is filled black, the orange circle and the black spinning arc are drawn, and then each ball is drawn at its current position with its unique color.
5.  The display is updated, and the simulation speed is controlled by `clock.tick(60)`.

## Key Parameters

* `WIDTH`, `HEIGHT`: 800x800 pixels.
* `CIRCLE_CENTER`: Center of the window.
* `CIRCLE_RADIUS`: 150 pixels.
* `BALL_RADIUS`: 5 pixels.
* `GRAVITY`: 0.2 units per frame.
* `spinning_speed`: 0.01 radians per frame.
* `arc_degree`: Initial opening of the arc is 60 degrees.
