<script>
  const Dice = new Map([[1]]);
  const Options = {
    LIKELY: "likely",
    BALANCED: "balanced",
    UNLIKELY: "unlikely"
  };
  let likelihood = Options.BALANCED;
  let result = [];

  function d6() {
    const min = 1;
    const max = 6;
    return Math.floor(Math.random() * (max - min + 1) + min);
  }
  function rollWithAdvantage() {
    return Math.max(d6(), d6());
  }
  function handleRollClick() {
    let black = null;
    let white = null;

    if (likelihood === Options.LIKELY) {
      black = d6();
      white = rollWithAdvantage();
    } else if (likelihood === Options.UNLIKELY) {
      black = rollWithAdvantage();
      white = d6();
    } else {
      black = d6();
      white = d6();
    }
    console.log("die results:", black, white);

    const outcome = [];
    if (black === white) {
      outcome.push("Plot Twist");
    } else {
      if (black > white) {
        outcome.push("No");
      } else {
        outcome.push("Yes");
      }
      if (black <= 3 && white <= 3) {
        outcome.push("But");
      } else if (black >= 4 && white >= 4) {
        outcome.push("And");
      }
    }

    result = outcome;
  }
</script>

<style>
  main {
    display: flex;
    flex-direction: column;
    align-items: center;
    row-gap: 1rem;
  }
  .likelihood {
    display: flex;
    flex-direction: column;
  }
  .result {
    font-size: 2rem;
  }
</style>

<main>
	<h1>Recluse</h1>
	
  <article>
    <section class="likelihood">
      <label class="pure-radio">
        <input type="radio" bind:group={likelihood} name="likelihood" value={Options.LIKELY}>
        Likely
      </label>
      <label class="pure-radio">
        <input type="radio" bind:group={likelihood} name="likelihood" value={Options.BALANCED}>
        Balanced
      </label>
      <label class="pure-radio">
        <input type="radio" bind:group={likelihood} name="likelihood" value={Options.UNLIKELY}>
        Unlikely
      </label>
    </section>

    <br />
    <button class="pure-button" on:click={handleRollClick}>Roll</button>
  </article>

  <article class="result">
    <strong>{result.join(", ")}</strong>
  </article>
</main>