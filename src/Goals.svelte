<script>
  import { createEventDispatcher } from "svelte";
  import { toast } from "svelte-sonner"; 

  export let goals = {
    sleep: 7,
    water: 8,
    vegetables: true,
    fruits: true,
    exercise: true,
    mood: true,
  };

  const dispatch = createEventDispatcher();
  let saved = false;

  let originalGoals = JSON.parse(localStorage.getItem("goals") || "null");

  function saveGoals() {
    let activeCount = 0;

    if (Number(goals.sleep) > 0) activeCount++;
    if (Number(goals.water) > 0) activeCount++;

    if (goals.vegetables) activeCount++;
    if (goals.fruits) activeCount++;
    if (goals.exercise) activeCount++;
    if (goals.mood) activeCount++;

    if (activeCount < 4) {
      toast.error("Please set at least 4 goals before saving.");
      return;
    }

    localStorage.setItem("goals", JSON.stringify(goals));
    originalGoals = JSON.parse(JSON.stringify(goals)); // update baseline
    dispatch("setGoals", goals);

    saved = true;
    toast.success(" Goals updated!");
    setTimeout(() => (saved = false), 2000);
  }

  function toggleGoal(key) {
    goals[key] = !goals[key];
  }

  $: hasChanged = JSON.stringify(goals) !== JSON.stringify(originalGoals);
</script>

<div class="bg-white rounded-2xl shadow-lg p-5">
  <h2 class="text-xl font-bold text-blue-800 mb-4">🎯 Set Your Goals</h2>

  <div class="grid md:grid-cols-3 gap-4">
    <!-- Sleep Goal (Slider) -->
    <div
      class="h-28 p-3 border rounded-xl bg-gray-50 shadow-sm hover:shadow-md transition flex flex-col justify-center"
    >
      <p class="font-semibold text-gray-700 mb-2">😴 Sleep Goal</p>
      <div class="flex items-center gap-3">
        <input
          type="range"
          min="2"
          max="11"
          step="1"
          bind:value={goals.sleep}
          class="w-full accent-blue-700"
        />
        <span class="text-sm font-semibold text-blue-700">
          {goals.sleep === 2 ? "<3" : goals.sleep === 11 ? "10+" : goals.sleep}
        </span>
      </div>
    </div>

    <!-- Water Goal (Slider) -->
    <div
      class="h-28 p-3 border rounded-xl bg-gray-50 shadow-sm hover:shadow-md transition flex flex-col justify-center"
    >
      <p class="font-semibold text-gray-700 mb-2">💧 Water Goal</p>
      <div class="flex items-center gap-3">
        <input
          type="range"
          min="4"
          max="11"
          step="1"
          bind:value={goals.water}
          class="w-full accent-blue-700"
        />
        <span class="text-sm font-semibold text-blue-700">
          {goals.water === 4 ? "<5" : goals.water === 11 ? "10+" : goals.water}
        </span>
      </div>
    </div>

    <!-- Vegetables Goal -->
    <button
      type="button"
      class="h-28 p-3 border rounded-xl bg-gray-50 shadow-sm hover:shadow-md hover:bg-blue-50 transition flex items-center"
      on:click={() => toggleGoal("vegetables")}
    >
      <input
        type="checkbox"
        bind:checked={goals.vegetables}
        class="accent-blue-700 pointer-events-none"
      />
      <span class="ml-2 text-sm font-semibold text-gray-700"
        >🥗 Track vegetables daily</span
      >
    </button>

    <!-- Fruits Goal -->
    <button
      type="button"
      class="h-28 p-3 border rounded-xl bg-gray-50 shadow-sm hover:shadow-md hover:bg-blue-50 transition flex items-center"
      on:click={() => toggleGoal("fruits")}
    >
      <input
        type="checkbox"
        bind:checked={goals.fruits}
        class="accent-blue-700 pointer-events-none"
      />
      <span class="ml-2 text-sm font-semibold text-gray-700"
        >🍎 Track fruits daily</span
      >
    </button>

    <!-- Exercise Goal -->
    <button
      type="button"
      class="h-28 p-3 border rounded-xl bg-gray-50 shadow-sm hover:shadow-md hover:bg-blue-50 transition flex items-center"
      on:click={() => toggleGoal("exercise")}
    >
      <input
        type="checkbox"
        bind:checked={goals.exercise}
        class="accent-blue-700 pointer-events-none"
      />
      <span class="ml-2 text-sm font-semibold text-gray-700"
        >🏃 Track exercise daily</span
      >
    </button>

    <!-- Mood Goal -->
    <button
      type="button"
      class="h-28 p-3 border rounded-xl bg-gray-50 shadow-sm hover:shadow-md hover:bg-blue-50 transition flex items-center"
      on:click={() => toggleGoal("mood")}
    >
      <input
        type="checkbox"
        bind:checked={goals.mood}
        class="accent-blue-700 pointer-events-none"
      />
      <span class="ml-2 text-sm font-semibold text-gray-700"
        >😊 Track mood daily</span
      >
    </button>
  </div>

  <!-- Save button -->
  <div class="mt-5 text-right">
    <button
      on:click={saveGoals}
      class="px-5 py-2 bg-blue-700 text-white text-sm font-semibold rounded-lg shadow hover:bg-blue-800 transition disabled:opacity-50 disabled:cursor-not-allowed"
      disabled={!hasChanged}
    >
      Save Goals
    </button>
  </div>
</div>
