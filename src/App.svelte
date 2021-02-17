<!-- Based off of: https://codepen.io/hchiam/pen/poNeZZx -->
<script>
  let numberOfGroups = 2;
  let enableMakeGroupsButton = false;
  const namesPlaceholder = `Andrew
Barbara
Charlie
David
Eli
Fred`;
  let names = namesPlaceholder;

  $: namesArray = shuffle(names.split("\n").filter((name) => name !== ""));
  $: numberOfNames = namesArray.length;
  $: namesPerGroup = Math.ceil(numberOfNames / numberOfGroups);
  let showGroups = false;

  $: if (numberOfGroups < 2) numberOfGroups = 2;
  $: enableMakeGroupsButton = names !== "";

  function shuffle(array) {
    /**
     * Fisher-Yates/Knuth shuffle algorithm,
     * with in-place swap,
     * and a growing "shuffled" section:
     */
    for (let i = array.length - 1; i > 0; i--) {
      const j = Math.floor(Math.random() * (i + 1));
      [array[i], array[j]] = [array[j], array[i]];
    }
    return array;
  }

  function shuffleGroups() {
    namesArray = shuffle(names.split("\n").filter((name) => name !== ""));
  }
</script>

<div id="container">
  <h1>Random Group Generator</h1>
  <main>
    <section>
      <div id="groups">
        {#if showGroups}
          {#each Array(numberOfGroups) as _, i}
            <div class="group" id="group{i + 1}">
              <h2>Group {i + 1}</h2>
              {#each Array(namesPerGroup) as _, j}
                {#if i * namesPerGroup + j < namesArray.length}
                  <p>{namesArray[i * namesPerGroup + j]}</p>
                {/if}
              {/each}
            </div>
          {/each}
        {/if}
      </div>
      <div id="main_controls">
        <p>
          Number of groups: <input
            bind:value={numberOfGroups}
            id="number_of_groups"
            type="number"
            step="1"
            min="2"
          />
        </p>
        {#if enableMakeGroupsButton}
          <button
            id="make_groups"
            on:click={() => {
              setTimeout(() => {
                showGroups = true;
                shuffleGroups();
                enableMakeGroupsButton = false;
              }, 300);
            }}
            disabled={!enableMakeGroupsButton}>Make Groups</button
          >
        {:else}
          <p class="button-note">
            (To create new groups, edit the names list.)
          </p>
        {/if}
        <p>
          <textarea
            bind:value={names}
            on:keyup={() => {
              showGroups = false;
            }}
            placeholder={namesPlaceholder}
            name="names"
            id="names"
            cols="30"
            rows="10"
          />
        </p>
      </div>
    </section>
  </main>
</div>

<link
  rel="stylesheet"
  href="https://cdn.jsdelivr.net/gh/hchiam/css-boilerplate@3.2.0/style.css"
  integrity="sha384-bNDIb02CEQaNkOgyN7XrbxvBunWNmemBNBgPwavxWanjP/lnmSKEZaufDDgOEUuG"
  crossorigin="anonymous"
/>

<style>
  h1 {
    text-align: center;
  }

  main {
    font-family: avenir, tahoma;
    padding: 1em;
    padding-bottom: 5em;
    display: flex;
    align-content: center;
    justify-content: center;
  }

  #number_of_groups {
    background: cyan;
    color: black;
    width: 5ch;
    border: none;
  }

  #number_of_groups:hover,
  #number_of_groups:focus {
    font-weight: bold;
    background: lime;
    box-shadow: 0 0 10px black;
  }

  section {
    display: flex;
  }

  #main_controls {
    margin-top: -1em;
  }

  .group {
    display: inline-block;
    vertical-align: top;
    background: #555;
    padding: 1em;
    margin: 0 1em 1em 0;
  }

  .group h2,
  .group p {
    background: inherit;
    color: inherit;
  }

  #names:hover,
  #names:focus {
    background: black;
    color: white;
    box-shadow: 0 0 10px black;
  }

  .button-note {
    font-size: small;
    font-style: italic;
    margin-top: -0.5em;
    color: yellow;
  }

  @media (max-width: 425px) {
    section {
      flex-direction: column-reverse;
    }
  }
</style>
