<script lang="ts">
  const numbers = ['1', '2', '3', '4', '5', '6', '7', '8', '9', '0', '.'];
  const operations = ['/', 'x', '-', '+', '='];

  let previous = '';
  let current = '';
  let selectedOperation = '';

  const handleOperationClick = (operation: string) => {
    if (!current) return;

    let results = '';
    const oldNum = parseFloat(previous);
    const newNum = parseFloat(current);
    previous = current;

    if (operation === '=') {
      if (!previous) return;

      switch (selectedOperation) {
        case '/':
          results = parseFloat((oldNum / newNum).toFixed(4)).toString();
          break;

        case 'x':
          results = parseFloat((oldNum * newNum).toFixed(4)).toString();
          break;

        case '+':
          results = parseFloat((oldNum + newNum).toFixed(4)).toString();
          break;

        case '-':
          results = parseFloat((oldNum - newNum).toFixed(4)).toString();
          break;
      }

      current = results;
    } else {
      if (selectedOperation && previous) {
        // Evaluate the expression before proceeding with the new operation
        handleOperationClick('=');
      }

      current = '';
    }

    selectedOperation = operation;
  };

  const handleClear = () => {
    selectedOperation = '';
    previous = '';
    current = '';
  };

  const handleNumberClick = (number: string) => {
    if (number === '.' && current.includes('.')) return;
    if (number === '.' && current == '') return (current += '0.');

    current += number;
  };
</script>

<main>
  <div class="calculator">
    <div class="results">
      {current}
    </div>
    <div class="digits">
      <div class="numbers">
        <button class="btn btn-xlg" on:click={handleClear}> C </button>
        {#each numbers as number (number)}
          <button
            class={`btn ${number === '0' ? 'btn-lg' : null}`}
            on:click={() => handleNumberClick(number)}
          >
            {number}
          </button>
        {/each}
      </div>
      <div class="operations">
        {#each operations as operation (operation)}
          <button
            class={`btn ${
              operation === selectedOperation ? 'btn-silver' : 'btn-orange'
            }`}
            on:click={() => handleOperationClick(operation)}
          >
            {operation}
          </button>
        {/each}
      </div>
    </div>
  </div>
</main>

<style>
  main {
    width: 100vw;
    height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .calculator {
    background-color: rgb(28, 28, 28);
    width: 240px;
    padding: 15px;
    border-radius: 7px;
  }
  .digits {
    display: flex;
  }
  .operations {
    display: flex;
    flex-direction: column;
  }
  .numbers {
    display: flex;
    flex-wrap: wrap;
    width: 200px;
  }
  .btn {
    width: 50px;
    height: 50px;
    border-radius: 100px;
    background-color: rgb(114, 113, 113);
    font-size: 20px;
    font-weight: bold;
    color: white;
    margin: 5px;
    border: none;
  }
  .btn-lg {
    width: 110px;
  }
  .btn-orange {
    background-color: orange;
  }
  .btn-silver {
    background-color: silver;
  }
  .btn-xlg {
    width: 170px;
  }
  .results {
    height: 60px;
    color: white;
    font-size: 30px;
    display: flex;
    flex-direction: row-reverse;
    margin-right: 10px;
    line-height: 2;
  }
</style>
