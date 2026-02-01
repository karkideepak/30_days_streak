const DAYS = 30;
const STORAGE_KEY = "workoutProgress";

const grid = document.getElementById("grid");
const streakEl = document.getElementById("streak");
const resetBtn = document.getElementById("resetBtn");

let progress = JSON.parse(localStorage.getItem(STORAGE_KEY)) 
  || Array(DAYS).fill(false);

function render() {
  grid.innerHTML = "";

  progress.forEach((done, index) => {
    const day = document.createElement("div");
    day.className = `day ${done ? "done" : ""}`;
    day.textContent = index + 1;

    day.addEventListener("click", () => toggleDay(index));
    grid.appendChild(day);
  });

  updateStreak();
}

function toggleDay(index) {
  progress[index] = !progress[index];
  localStorage.setItem(STORAGE_KEY, JSON.stringify(progress));
  render();
}

function updateStreak() {
  let streak = 0;
  for (const day of progress) {
    if (day) streak++;
    else break;
  }
  streakEl.textContent = streak;
}

resetBtn.addEventListener("click", () => {
  if (confirm("Are you sure you want to reset your progress?")) {
    progress = Array(DAYS).fill(false);
    localStorage.removeItem(STORAGE_KEY);
    render();
  }
});

render();
