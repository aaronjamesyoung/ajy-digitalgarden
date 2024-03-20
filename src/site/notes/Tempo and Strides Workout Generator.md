---
{"dg-publish":true,"grade":2,"permalink":"/tempo-and-strides-workout-generator/","dgPassFrontmatter":true}
---


<form>
<label>
  This is week number
  <input type="number" id="gen-week" style="width: 100px;" />
  in my training season.
</label>
<label>
  My long run this week will be
  <input type="number" id="gen-miles" style="width: 100px;" />
  miles
</label>
<button id="gen-workout">Get a workout</button>
</form>
<ul id="gen-display"></ul>
<script>
const miles = document.getElementById('gen-miles').value;
const week = document.getElementById('gen-week').value;

const button = document.getElementById('gen-workout');
const outputter = document.getElementById('gen-display');

button.onclick = function() {
    const longerInt = Math.round(miles * 0.6);
    const shorterInt = miles - longerInt;
    let speedString = '5 x (90 seconds fast, 3 minutes rest)';
    const workoutarr = [];
    if(week % 2 === 0) {
        workoutarr.push(`${shorterInt} minutes tempo, ${shorterInt / 2} minutes rest`);
        workoutarr.push(`${longerInt} minutes tempo, then straight into:`);
        workoutarr.push('5 x (60 seconds fast, 2 minutes rest)');
    } else {
        workoutarr.push(`${longerInt} minutes tempo, ${longerInt / 2} minutes rest`);
        workoutarr.push(`${shorterInt} minutes tempo, then straight into:`);
        workoutarr.push('5 x (90 seconds fast, 3 minutes rest)');
    }
    outputter.html(`<li>${workoutarr.join('</li><li>')}</li>`);
};
</script>