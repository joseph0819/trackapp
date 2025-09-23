<script>
  export let entries = [];
  export let goals = {};
  export const settings = {};

  // Average value for numeric fields
  function avg(key) {
    if (!entries?.length) return 0;
    return (
      entries.reduce((sum, e) => sum + Number(e[key] || 0), 0) / entries.length
    ).toFixed(1);
  }

  // Success rate against goals
  function successRate(key, goal) {
    if (!goal || !entries?.length) return 0;
    let success = entries.filter((e) => Number(e[key] || 0) >= goal).length;
    return Math.round((success / entries.length) * 100);
  }

  // Most common mood
  function mostCommonMood() {
    if (!entries?.length) return "N/A";
    let moodCounts = entries.reduce((acc, e) => {
      acc[e.mood] = (acc[e.mood] || 0) + 1;
      return acc;
    }, {});
    return Object.entries(moodCounts).sort((a, b) => b[1] - a[1])[0][0];
  }
</script>

<section class="bg-blue-700 rounded-xl shadow-lg p-8 text-white">
  <div class="flex items-center justify-between mb-6">
    <h2 class="text-xl font-bold">📊 Overview</h2>
    <!-- 🔹 Total Entries as styled text -->
    <p class="text-sm font-semibold text-green-300">
      🗂️ {entries.length} total entries logged
    </p>
  </div>

  <div class="grid md:grid-cols-3 gap-6 text-sm">
    <!-- Sleep -->
    <div
      class="relative p-4 rounded-lg transition
                {goals.sleep
        ? 'bg-blue-600 hover:bg-blue-500'
        : 'bg-blue-300 opacity-70'}"
    >
      <span
        class="absolute top-2 right-2 text-xs font-semibold
                  {goals.sleep ? 'text-green-300' : 'text-gray-200'}"
      >
        {goals.sleep ? "✅ Tracking" : "🚫 Not tracking"}
      </span>
      <p class="font-semibold">😴 Sleep</p>
      {#if goals.sleep}
        <p class="text-white font-bold">{avg("sleep")} hrs avg</p>
        <p class="text-xs text-blue-200">
          {successRate("sleep", goals.sleep)}% met goal ({goals.sleep} hrs)
        </p>
        <div class="w-full bg-blue-400 rounded-full h-2 mt-1">
          <div
            class="bg-green-400 h-2 rounded-full transition-all duration-500"
            style="width: {successRate('sleep', goals.sleep)}%"
          ></div>
        </div>
      {:else}
        <p class="text-blue-200 text-sm italic">Not tracking sleep</p>
      {/if}
    </div>

    <!-- Water -->
    <div
      class="relative p-4 rounded-lg transition
                {goals.water
        ? 'bg-blue-600 hover:bg-blue-500'
        : 'bg-blue-300 opacity-70'}"
    >
      <span
        class="absolute top-2 right-2 text-xs font-semibold
                  {goals.water ? 'text-green-300' : 'text-gray-200'}"
      >
        {goals.water ? "✅ Tracking" : "🚫 Not tracking"}
      </span>
      <p class="font-semibold">💧 Water</p>
      {#if goals.water}
        <p class="text-white font-bold">{avg("water")} cups avg</p>
        <p class="text-xs text-blue-200">
          {successRate("water", goals.water)}% met goal ({goals.water} cups)
        </p>
        <div class="w-full bg-blue-400 rounded-full h-2 mt-1">
          <div
            class="bg-green-400 h-2 rounded-full transition-all duration-500"
            style="width: {successRate('water', goals.water)}%"
          ></div>
        </div>
      {:else}
        <p class="text-blue-200 text-sm italic">Not tracking water</p>
      {/if}
    </div>

    <!-- Vegetables -->
    <div
      class="relative p-4 rounded-lg transition
                {goals.vegetables
        ? 'bg-blue-600 hover:bg-blue-500'
        : 'bg-blue-300 opacity-70'}"
    >
      <span
        class="absolute top-2 right-2 text-xs font-semibold
                  {goals.vegetables ? 'text-green-300' : 'text-gray-200'}"
      >
        {goals.vegetables ? "✅ Tracking" : "🚫 Not tracking"}
      </span>
      <p class="font-semibold">🥗 Vegetables</p>
      {#if goals.vegetables}
        <p class="text-green-300 font-bold">
          {entries.filter((e) => e.vegetables).length} days
        </p>
        <p class="text-xs text-blue-200">
          {Math.round(
            (entries.filter((e) => e.vegetables).length / entries.length) * 100
          )}% of days
        </p>
        <div class="w-full bg-blue-400 rounded-full h-2 mt-1">
          <div
            class="bg-green-400 h-2 rounded-full transition-all duration-500"
            style="width: {Math.min(
              (entries.filter((e) => e.vegetables).length / entries.length) *
                100 || 0,
              100
            )}%"
          ></div>
        </div>
      {:else}
        <p class="text-blue-200 text-sm italic">Not tracking vegetables</p>
      {/if}
    </div>

    <!-- Fruits -->
    <div
      class="relative p-4 rounded-lg transition
                {goals.fruits
        ? 'bg-blue-600 hover:bg-blue-500'
        : 'bg-blue-300 opacity-70'}"
    >
      <span
        class="absolute top-2 right-2 text-xs font-semibold
                  {goals.fruits ? 'text-green-300' : 'text-gray-200'}"
      >
        {goals.fruits ? "✅ Tracking" : "🚫 Not tracking"}
      </span>
      <p class="font-semibold">🍎 Fruits</p>
      {#if goals.fruits}
        <p class="text-red-300 font-bold">
          {entries.filter((e) => e.fruits).length} days
        </p>
        <p class="text-xs text-blue-200">
          {Math.round(
            (entries.filter((e) => e.fruits).length / entries.length) * 100
          )}% of days
        </p>
        <div class="w-full bg-blue-400 rounded-full h-2 mt-1">
          <div
            class="bg-red-400 h-2 rounded-full transition-all duration-500"
            style="width: {Math.min(
              (entries.filter((e) => e.fruits).length / entries.length) * 100 ||
                0,
              100
            )}%"
          ></div>
        </div>
      {:else}
        <p class="text-blue-200 text-sm italic">Not tracking fruits</p>
      {/if}
    </div>

    <!-- Exercise -->
    <div
      class="relative p-4 rounded-lg transition
                {goals.exercise
        ? 'bg-blue-600 hover:bg-blue-500'
        : 'bg-blue-300 opacity-70'}"
    >
      <span
        class="absolute top-2 right-2 text-xs font-semibold
                  {goals.exercise ? 'text-green-300' : 'text-gray-200'}"
      >
        {goals.exercise ? "✅ Tracking" : "🚫 Not tracking"}
      </span>
      <p class="font-semibold">🏃 Exercise</p>
      {#if goals.exercise}
        <p class="text-orange-300 font-bold">
          {entries.filter((e) => e.exercises && e.exercises.length > 0).length} days
          active
        </p>
        <p class="text-xs text-blue-200">
          {Math.round(
            (entries.filter((e) => e.exercises && e.exercises.length > 0)
              .length /
              entries.length) *
              100
          )}% of days
        </p>
        <div class="w-full bg-blue-400 rounded-full h-2 mt-1">
          <div
            class="bg-orange-400 h-2 rounded-full transition-all duration-500"
            style="width: {Math.min(
              (entries.filter((e) => e.exercises && e.exercises.length > 0)
                .length /
                entries.length) *
                100 || 0,
              100
            )}%"
          ></div>
        </div>
      {:else}
        <p class="text-blue-200 text-sm italic">Not tracking exercise</p>
      {/if}
    </div>

    <!-- Mood -->
    <div
      class="relative p-4 rounded-lg transition
                {goals.mood
        ? 'bg-blue-600 hover:bg-blue-500'
        : 'bg-blue-300 opacity-70'}"
    >
      <span
        class="absolute top-2 right-2 text-xs font-semibold
                  {goals.mood ? 'text-green-300' : 'text-gray-200'}"
      >
        {goals.mood ? "✅ Tracking" : "🚫 Not tracking"}
      </span>
      <p class="font-semibold">😊 Mood</p>
      {#if goals.mood}
        <p class="text-purple-300 font-bold">{mostCommonMood()}</p>
        <p class="text-xs text-blue-200">Based on {entries.length} entries</p>
      {:else}
        <p class="text-blue-200 text-sm italic">Not tracking mood</p>
      {/if}
    </div>
  </div>
</section>
