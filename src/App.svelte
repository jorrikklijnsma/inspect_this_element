<script lang="ts">
  // Get the root element
  const rootElement: HTMLElement = document.querySelector("body");

  // Create a function for setting a variable value
  const setVariable = (varName: string, varValue: string) => {
    // Set the value of variable {{varName}} to the value of {{varValue}}
    rootElement.style.setProperty(varName, varValue);
  };
  const height = 30; // total height
  let position = Math.random() * (height / 4) + height / 2; // random initial position
  let velocity = 0; // initial velocity
  const gravity = -0.2; // increase gravity to make the ball fall faster
  const bounce = -0.9; // bouncing energy loss factor
  let lastTime = performance.now(); // time of the last frame

  const calculateNextPosition = () => {
    const now = performance.now();
    let dt = (now - lastTime) / 1000.0; // delta time in seconds
    lastTime = now;

    // update velocity based on gravity
    velocity += gravity;
    // update position based on velocity
    position += velocity * dt;

    // bouncing off the ceiling
    if (position >= height) {
      // reverse the velocity (and apply bouncing energy loss)
      velocity = velocity * bounce;
      // set the position to the height (to not exceed it)
      position = height;
    }

    // bouncing off the floor
    if (position < 0) {
      // reverse the velocity (and apply bouncing energy loss)
      velocity = velocity * bounce;
      // set the position to zero (to not go below it)
      position = 0;
      // reset the position to a new random height when the bouncing has stopped
      if (Math.abs(velocity) < 0.001) {
        // reduce this threshold to consider the ball 'not moving' sooner
        position = Math.random() * (height / 4) + height / 2;
        velocity = 0;
      }
    }

    const ballPosition = Math.round(position);
    // ballPosition now contains the position of the Ball on the axis
    for (let i = height; i > 0; i--) {
      setVariable(
        `--color-${i.toString().padStart(2, "0")}`,
        `rgb(${ballPosition === i ? 255 : 25},10,50)`
      );
    }

    requestAnimationFrame(calculateNextPosition);
  };

  // start the animation
  calculateNextPosition();

  setInterval(() => {
    position = Math.random() * (height / 4) + height / 2;
  }, 20000);
</script>

<h1>Inspect element me!</h1>

<style>
  h1 {
    font-size: 3.2em;
    line-height: 1.1;
    pointer-events: none;
  }
</style>