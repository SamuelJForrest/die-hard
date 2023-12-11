<script>
  const mq = window.matchMedia("(min-width: 992px)");
  let completed = false;
  let handleMouseMove;
  let completeSurvey;
  let failSurvey;
  let x = 0;
  let y = 0;
  let yesBtnFullWidth = false;

  document.addEventListener('DOMContentLoaded', () => {
    const target = document.querySelector('.__no');
    
    completeSurvey = () => {
      completed = true;
    }
    
    if (mq.matches) {
      const minimumDistance = 50; // Set your desired minimum distance here
      const targetRect = target.getBoundingClientRect();
      const targetX = targetRect.left + targetRect.width / 2;
      let targetY = targetRect.top + targetRect.height / 2;

      handleMouseMove = (event) => {
        const cursorX = event.clientX;
        const cursorY = event.clientY;

        const distance = Math.sqrt(
          Math.pow(cursorX - targetX, 2) + Math.pow(cursorY - targetY, 2)
        );


        if (distance < 100) {
          const dx = cursorX - targetX;
          const dy = cursorY - targetY;
          
          const length = Math.sqrt(dx * dx + dy * dy);
          const normalizedDx = -dx / length; // Negate here
          const normalizedDy = -dy / length; // Negate here
          
          const adjustedDistance = Math.max(minimumDistance, distance);
          
          x = normalizedDx * adjustedDistance;
          y = normalizedDy * adjustedDistance;
        }
      };
    } else {
      failSurvey = () => {
        y = 5000;
        yesBtnFullWidth = true;
      }
    }
  });
</script>

<main on:mousemove={handleMouseMove}>
  <div class="container">
    <div class="row">
      {#if completed}
        <div class="col-12 text-center">
          <h1 class="__thankyou">Yipee Ki-Yay!</h1>
          <p>Your answer has been recorded. See how others voted:</p>
        </div><!-- col -->
        <div class="col-lg-4 offset-lg-2 text-center">
          <h2>Yes</h2>
          <p class="result __yes">100%</p>
        </div><!-- col -->
        <div class="col-lg-4 text-center">
          <h2>No</h2>
          <p class="result __no">0%</p>
        </div><!-- col -->
      {:else}
        <div class="col-lg-4 offset-lg-2 my-3 my-lg-0">
          <div class="image-wrap">
            <img src="/die-hard.jpg" alt="Die Hard movie poster">
          </div>
        </div><!-- col -->
        <div class="col-lg-4 d-flex flex-column justify-content-center align-items-center mb-3 mb-lg-0">
          <h1 class="title">Is Die Hard a Christmas Movie?</h1>
          <div class="buttons">
            <button class={`button __yes ${yesBtnFullWidth ? '__fullwidth' : ''}`} on:click={completeSurvey}>Yes</button>
            <button class="button __no" on:click={failSurvey} style={`transform: translate(${x}px, ${y}px);`}>No</button>
          </div>
        </div>
      {/if}
    </div><!-- row -->
  </div><!-- container -->
</main>

<style>
  main {
    background: linear-gradient(to bottom left, var(--color-black-2), var(--color-black));
    min-height: 100vh;
    width: auto;
    display: flex;
    justify-content: center;
    align-items: center;
    overflow: hidden;
  }

  .image-wrap img {
    width: 100%;
  }

  h1 {
    color: var(--color-white);
    text-transform: uppercase;
    text-align: center;
    font-size: 3.2rem;
    margin-bottom: 1rem;
  }

  h1.__thankyou {
    font-size: 5rem;
    margin-bottom: .6rem;
  }

  p {
    margin-bottom: 4rem;
  }

  .buttons {
    position: relative;
  }

  .button {
    border: none;
    padding: .6rem 2rem;
    border-radius: 4px;
    font-weight: 500;
    color: var(--color-white);
    font-size: 1.2rem;
    transition: all .3s;
  }

  .button.__yes {
    background: var(--color-green);
    margin-right: 104px;
    max-width: 92px;
    width: 100%;
  }

  .button.__yes.__fullwidth {
    max-width: 200px;
  }

  .button.__no {
    cursor: not-allowed;
    background: var(--color-red);
    position: absolute;
    right: 0;
    top: 0;
  }

  .button:hover,
  .button:focus {
    opacity: 0.7;
  }

  .result {
    font-size: 5rem;
  }

  .result.__yes {
    color: var(--color-green);
  }

  .result.__no {
    color: var(--color-red);
  }

  @media screen and (max-width: 992px) {
      .result {
        margin-bottom: 2rem;
      }
  }
</style>