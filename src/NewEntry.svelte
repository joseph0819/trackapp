<script>
  import { createEventDispatcher } from "svelte";
  import { toast } from "svelte-sonner"; 

  export let settings;
  const dispatch = createEventDispatcher();

  // form state
  let sleep = "";
  let water = "";
  let vegetables = false;
  let fruits = false;
  let exercises = [];
  let movements = [];
  let fastFood = "";
  let endOfDay = "";
  let mood = "";
  let notes = "";

  $: filledCount =
    (sleep ? 1 : 0) +
    (water ? 1 : 0) +
    (vegetables ? 1 : 0) +
    (fruits ? 1 : 0) +
    (exercises.length ? 1 : 0) +
    (movements.length ? 1 : 0) +
    (fastFood ? 1 : 0) +
    (endOfDay ? 1 : 0) +
    (mood ? 1 : 0) +
    (notes ? 1 : 0);

  function saveEntry() {
    if (filledCount < 4) {
      toast.error("⚠️ Please fill at least 4 activities before saving.");
      return;
    }

    const now = new Date();
    const newEntry = {
      timestamp: now.toISOString(), 
      sleep,
      water,
      vegetables,
      fruits,
      exercises,
      movements,
      fastFood,
      endOfDay,
      mood,
      notes,
    };

    dispatch("save", newEntry);
    toast.success(" Entry saved successfully!");
    resetForm();
  }

  function resetForm() {
    sleep = "";
    water = "";
    vegetables = false;
    fruits = false;
    exercises = [];
    movements = [];
    fastFood = "";
    endOfDay = "";
    mood = "";
    notes = "";
  }

  function handleDisabledClick() {
    if (filledCount < 4) {
      toast.error(" You need to log at least 4 items before saving.");
    }
  }
</script>

<section class="bg-white rounded-2xl shadow p-5">
  <h2 class="text-xl font-bold text-blue-700 mb-4">📝 Log Today’s Entry</h2>
  <p class="text-xs text-gray-500 mb-4">
    You must log at least <span class="font-semibold">4 items</span> before saving.
  </p>

  <form on:submit|preventDefault={saveEntry} class="space-y-6 text-sm">
    <!-- Sleep & Water -->
    {#if settings.trackSleep || settings.trackWater}
      <div class="grid md:grid-cols-2 gap-6 pb-4 border-b border-gray-200">
        {#if settings.trackSleep}
          <div>
            <p class="font-semibold text-blue-700 mb-2">😴 Sleep (hrs)</p>
            <div class="flex flex-wrap gap-3">
              {#each ["<5", "5", "6", "7", "8", "9", "10", "10+"] as hr}
                <label class="flex items-center gap-1 cursor-pointer">
                  <input type="radio" bind:group={sleep} value={hr} />
                  {hr}
                </label>
              {/each}
            </div>
          </div>
        {/if}

        {#if settings.trackWater}
          <div>
            <p class="font-semibold text-blue-700 mb-2">💧 Water (cups)</p>
            <div class="flex flex-wrap gap-3">
              {#each ["<5", "5", "6", "7", "8", "9", "10", "10+"] as glass}
                <label class="flex items-center gap-1 cursor-pointer">
                  <input type="radio" bind:group={water} value={glass} />
                  {glass}
                </label>
              {/each}
            </div>
          </div>
        {/if}
      </div>
    {/if}

    <!-- Veggies & Fruits -->
    {#if settings.trackVegetables || settings.trackFruits}
      <div class="grid md:grid-cols-2 gap-6 pb-4 border-b border-gray-200">
        {#if settings.trackVegetables}
          <label class="flex items-center gap-2 cursor-pointer">
            <input type="checkbox" bind:checked={vegetables} />
            🥗 Ate vegetables
          </label>
        {/if}
        {#if settings.trackFruits}
          <label class="flex items-center gap-2 cursor-pointer">
            <input type="checkbox" bind:checked={fruits} />
            🍎 Ate fruits
          </label>
        {/if}
      </div>
    {/if}

    <!-- Exercise & Movement -->
    {#if settings.trackExercise || settings.trackMovements}
      <div class="grid md:grid-cols-2 gap-6 pb-4 border-b border-gray-200">
        {#if settings.trackExercise}
          <div>
            <p class="font-semibold text-blue-700 mb-2">🏃 Exercise</p>
            <div class="flex flex-wrap gap-3">
              {#each ["Walk", "Run", "Yoga", "Gym", "Weights"] as ex}
                <label class="flex items-center gap-1 cursor-pointer">
                  <input type="checkbox" value={ex} bind:group={exercises} />
                  {ex}
                </label>
              {/each}
            </div>
          </div>
        {/if}

        {#if settings.trackMovements}
          <div>
            <p class="font-semibold text-blue-700 mb-2">🚶 Movement</p>
            <div class="flex flex-wrap gap-3">
              {#each ["Stretching", "Chores", "Short Walk", "House Walk"] as mv}
                <label class="flex items-center gap-1 cursor-pointer">
                  <input type="checkbox" value={mv} bind:group={movements} />
                  {mv}
                </label>
              {/each}
            </div>
          </div>
        {/if}
      </div>
    {/if}

    <!-- Meal & End of Day -->
    {#if settings.trackFastFood || settings.trackEndOfDay}
      <div class="grid md:grid-cols-2 gap-6 pb-4 border-b border-gray-200">
        {#if settings.trackFastFood}
          <div>
            <p class="font-semibold text-blue-700 mb-2">🍔 Meal Type</p>
            <label class="flex items-center gap-1 mb-2">
              <input type="radio" bind:group={fastFood} value="Healthy Meal" />
              Healthy
            </label>
            <label class="flex items-center gap-1">
              <input type="radio" bind:group={fastFood} value="Fast Food" />
              Fast Food
            </label>
          </div>
        {/if}

        {#if settings.trackEndOfDay}
          <div>
            <p class="font-semibold text-blue-700 mb-2">🌙 End of Day Feeling</p>
            <div class="flex flex-wrap gap-3">
              {#each ["Energized ⚡", "Okay 😌", "Tired 😴"] as feeling}
                <label class="flex items-center gap-1 cursor-pointer">
                  <input type="radio" bind:group={endOfDay} value={feeling} />
                  {feeling}
                </label>
              {/each}
            </div>
          </div>
        {/if}
      </div>
    {/if}

    <!-- Mood -->
    {#if settings.trackMood}
      <div class="pb-4 border-b border-gray-200">
        <p class="font-semibold text-blue-700 mb-2">😊 Mood</p>
        <div class="flex flex-wrap gap-4">
          {#each ["Happy 🙂", "Calm 😌", "Neutral 😐", "Tired 😫", "Stressed 😟"] as m}
            <label class="flex items-center gap-1 cursor-pointer">
              <input type="radio" bind:group={mood} value={m} />
              {m}
            </label>
          {/each}
        </div>
      </div>
    {/if}

    <!-- Notes -->
    {#if settings.trackNotes}
      <div>
        <p class="font-semibold text-blue-700 mb-2">📝 Notes</p>
        <textarea
          bind:value={notes}
          rows="3"
          placeholder="Write your reflections..."
          class="border p-2 w-full rounded-lg focus:ring-2 focus:ring-blue-500 text-sm"
        ></textarea>
      </div>
    {/if}

    <!-- Save Button -->
    <div>
      <button
      type="button"
      on:click={() => {
        if (filledCount < 4) {
          toast.error(" You need to log at least 4 items before saving.");
        } else {
          saveEntry();
        }
      }}
      class="px-5 py-2 text-sm font-semibold rounded-lg shadow transition
        {filledCount < 4
          ? 'bg-gray-400 text-gray-200 cursor-not-allowed'
          : 'bg-blue-600 text-white hover:bg-blue-700 cursor-pointer'}"
    >
      💾 Save Entry
    </button>
    
    </div>
  </form>
</section>
