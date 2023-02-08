<script>
let dist = 5.0; // km
let time = 25.0; // min
let paceKmh = 12.0; // kmh
let paceSec = 300; // sec/km

let old = {
	dist: dist,
	time: time,
	paceKmh: paceKmh,
	paceSec: paceSec
};

let paceItems = [];
for (let min = 2; min < 8; min++) {
	for (let sec = 0; sec < 60; sec+=5) {
		paceItems.push({key: min*60 + sec, value: min+(sec < 10 ? ":0" : ":")+sec});
	}
}

const updateOldValues = () => {
	old.dist = dist;
	old.time = time;
	old.paceKmh = paceKmh;
	old.paceSec = paceSec;
}

const calcTime = () => {
	time = (dist / paceKmh * 60).toFixed(2);
	updateOldValues();
}
const calcDist = () => {
	dist = (paceKmh * time / 60).toFixed(2);
	updateOldValues();
}
const calcPace = () => {
	if (old.dist != dist || old.time != time) {
		paceKmh = (dist / time * 60).toFixed(2);
		paceSec = Math.round(3600 / paceKmh)
		if (paceSec % 5 != 0) paceSec -= paceSec % 5;
	} else if (old.paceKmh != paceKmh) {
		paceSec = Math.round(3600 / paceKmh)
		if (paceSec % 5 != 0) paceSec -= paceSec % 5;
		time = (dist / paceKmh * 60).toFixed(2);
	} else if (old.paceSec != paceSec) {
		paceKmh = (3600.0 / paceSec).toFixed(2);
		time = (dist / paceKmh * 60).toFixed(2);
	}
	updateOldValues();
}

</script>

<button on:click={calcTime}>Calculate Time</button>
<button on:click={calcDist}>Calculate Distance</button>
<button on:click={calcPace}>Calculate Pace</button>

<br>

<p align="left">Distance (km):

	<input type="number" placeholder="Distance" bind:value={dist}/>

</p>

<p align="left">Time (min):

	<input type="number" placeholder="Time" bind:value={time}/>

</p>

<p align="left">Pace (min/km):
	
	<select bind:value={paceSec}>
		{#each paceItems as item}
			<option value="{item.key}">{item.value}</option>
		{/each}
		}
	</select>
</p>

<p align="left">Pace (km/h):


	<input type="number" placeholder="Pace" bind:value={paceKmh}/>

</p>