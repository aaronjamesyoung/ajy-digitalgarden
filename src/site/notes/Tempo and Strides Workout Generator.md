---
{"dg-publish":true,"grade":2,"permalink":"/tempo-and-strides-workout-generator/","dgPassFrontmatter":true}
---

[[📘 Running\|📘 Running]]

Use this form to create tempo & stride workouts. This tool doesn't account for taper weeks or race weeks, so exercise your own judgement:

<div><form>
<label>
  This is week number
  <input type="number" id="gen-week" style="width: 100px;" />
  in my training season.
</label>
<label>
  My long run this week will be
  <input type="number" id="gen-miles" style="width: 100px;" />
  miles.
</label>
<a id="gen-workout">Get a workout &rarr;</a>
</form>
<ul id="gen-display"></ul>
<script>
const button = document.getElementById('gen-workout');
const outputter = document.getElementById('gen-display');
button.onclick = function(e) {
    e.preventDefault();
    const miles = Number(document.getElementById('gen-miles').value);
    const week = Number(document.getElementById('gen-week').value);
    const longerInt = Math.round(miles * 0.6);
    const shorterInt = miles - longerInt;
    const workoutarr = [];
    workoutarr.push('15 minutes warm up');
    if(week % 2 === 0) {
        workoutarr.push(`${shorterInt} minutes tempo, ${shorterInt / 2} minutes easy`);
        workoutarr.push(`${longerInt} minutes tempo, then straight into:`);
        workoutarr.push('5 x (60 seconds fast stride, 2 minutes easy)');
    } else {
        workoutarr.push(`${longerInt} minutes tempo, ${longerInt / 2} minutes easy`);
        workoutarr.push(`${shorterInt} minutes tempo, then straight into:`);
        workoutarr.push('5 x (90 seconds fast stride, 3 minutes easy)');
    }
    workoutarr.push('15 minutes cool down');
    outputter.innerHTML =`<li>${workoutarr.join('</li><li>')}</li>`;
};
</script></div>