<script>
let rows = [];
let score = 0;
let gameover = false;

function generateRow() {
	let row = new Array(4).fill("white");
	let pos = Math.trunc(Math.random()*4);
	row[pos] = "black"
return row;
}

function fillRows(){
	for(let i = 0; i < 4; i++){
	rows.push(generateRow());
	}
}

function tapped(i, j){
	if(i != rows.length - 1 || rows[i][j] == "white"){
	gameover = true;
	rows[i][j] = "red";
	}else{
	rows.splice(i);
	rows = [generateRow(), ...rows];
	score++;
	}
}

function restart(){
score = 0;
gameover = false;
rows=[];
fillRows();
}

fillRows();

</script>

<style>
/* Ensure the app container scales properly */
.app {
  position: relative;
  width: 100%;
  height: 100vh;
  max-width: 400px;
  margin: 0 auto;
}

/* Header styling with sticky positioning */
.app .header {
  position: sticky;
  top: 0;
  width: 100%;
  height: 50px;
  background: #0badea;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 20px;
  color: #fff;
}

/* Game area styling */
.app .game {
  width: 100%;
  height: calc(100vh - 50px); /* Adjust based on header height */
  background: #fff;
  display: flex;
  flex-direction: column;
}

/* Row styling to handle scaling properly */
.app .game .row {
  display: flex;
  width: 100%;
  height: 25%; /* Each row takes up 25% of the game area height */
}

/* Box styling for each tile */
.app .game .row .box {
  flex: 1;
  border: 1px solid #555;
  cursor: pointer;
  background: #fff; /* Ensure default background is white */
  box-sizing: border-box; /* Include padding and border in width and height */
}

/* Black tile styling */
.app .game .row .box.black {
  background: black;
}

/* Red tile animation */
.app .game .row .box.red {
  animation: blinkRed 500ms ease-in-out infinite;
}

/* Animation for red tile */
@keyframes blinkRed {
  0%, 100% {
    background: #fff;
  }
  50% {
    background: tomato;
  }
}

/* Result overlay styling */
.result {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.8);
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  gap: 10px;
}

.result h2 {
  font-size: 2.5rem; /* Responsive font size */
  color: #fff;
}

.result p {
  font-size: 1.5rem; /* Responsive font size */
  margin-bottom: 10px;
  color: #fff;
}

.result button {
  padding: 10px 20px;
  cursor: pointer;
  font-size: 1rem; /* Responsive font size */
  border: 2px solid #fff; /* Adjust border size */
  color: #fff;
  background: transparent;
  font-weight: 500;
  outline: none;
}

</style>

<main class="app">
	<div class="header">
	<h4> PianoTiles </h4>
	<p> Score: {score} </p>
	</div>
	<div class="game">
    {#each rows as row, i}
	<div class="row">
	{#each row as box, j}
	<div class={"box "+box}
			on:click={()=>tapped(i, j)}>
	</div>
	{/each}
	</div>
	{/each}
	</div>
	
	{#if gameover}
	<div class="result">
	<h2> Game Over </h2>
	<p> Score: {score}</p>
	<button on:click={restart}>Restart</button>
	</div>
	{/if}
</main>
