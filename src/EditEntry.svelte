<script>
  import { createEventDispatcher } from "svelte";

  export let open = false;
  export let entry = {};
  export const index = null;

  const dispatch = createEventDispatcher();

  let sleep,
    water,
    vegetables,
    fruits,
    exercises,
    movements,
    fastFood,
    endOfDay,
    mood,
    notes;

  $: if (entry) {
    sleep = entry.sleep ?? "";
    water = entry.water ?? "";
    vegetables = entry.vegetables ?? false;
    fruits = entry.fruits ?? false;
    exercises = entry.exercises ? [...entry.exercises] : [];
    movements = entry.movements ? [...entry.movements] : [];
    fastFood = entry.fastFood ?? "";
    endOfDay = entry.endOfDay ?? "";
    mood = entry.mood ?? "";
    notes = entry.notes ?? "";
  }

  function saveEdit() {
    const updated = {
      ...entry,
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
    dispatch("save", updated); // ✅ send back the updated entry
  }

  function close() {
    dispatch("close");
  }

  $: isEdited =
    entry &&
    (sleep !== (entry.sleep ?? "") ||
      water !== (entry.water ?? "") ||
      vegetables !== (entry.vegetables ?? false) ||
      fruits !== (entry.fruits ?? false) ||
      JSON.stringify(exercises) !== JSON.stringify(entry.exercises ?? []) ||
      JSON.stringify(movements) !== JSON.stringify(entry.movements ?? []) ||
      fastFood !== (entry.fastFood ?? "") ||
      endOfDay !== (entry.endOfDay ?? "") ||
      mood !== (entry.mood ?? "") ||
      notes !== (entry.notes ?? ""));
</script>

{#if open}
  <div class="fixed inset-0 bg-black/40 flex items-center justify-center z-50">
    <div class="bg-white w-full max-w-2xl rounded-2xl shadow-lg p-6 space-y-6">
      <h2 class="text-xl font-bold text-blue-800">✏ Edit Entry</h2>

      <div class="space-y-4 max-h-[70vh] overflow-y-auto pr-2">
        <!-- Sleep + Water -->
        <div class="grid grid-cols-2 gap-4">
          <div>
            <p class="font-semibold mb-2">😴 Sleep (hrs)</p>
            {#each ["<5", "5", "6", "7", "8", "9", "10", "10+"] as hr}
              <label class="inline-flex items-center gap-2 mr-2">
                <input type="radio" bind:group={sleep} value={hr} />
                {hr}
              </label>
            {/each}
          </div>
          <div>
            <p class="font-semibold mb-2">💧 Water (cups)</p>
            {#each ["<5", "5", "6", "7", "8", "9", "10", "10+"] as glass}
              <label class="inline-flex items-center gap-2 mr-2">
                <input type="radio" bind:group={water} value={glass} />
                {glass}
              </label>
            {/each}
          </div>
        </div>

        <!-- Vegetables + Fruits -->
        <div class="grid grid-cols-2 gap-4">
          <label class="flex items-center gap-2">
            <input type="checkbox" bind:checked={vegetables} />
            🥗 Ate vegetables
          </label>
          <label class="flex items-center gap-2">
            <input type="checkbox" bind:checked={fruits} />
            🍎 Ate fruits
          </label>
        </div>

        <!-- Exercise + Movement -->
        <div class="grid grid-cols-2 gap-4">
          <div>
            <p class="font-semibold mb-2">🏃 Exercise</p>
            {#each ["Walk", "Run", "Yoga", "Gym", "Weights"] as ex}
              <label class="flex items-center gap-2">
                <input type="checkbox" value={ex} bind:group={exercises} />
                {ex}
              </label>
            {/each}
          </div>
          <div>
            <p class="font-semibold mb-2">🚶 Movement</p>
            {#each ["Stretching", "Chores", "Short Walk", "House Walk"] as mv}
              <label class="flex items-center gap-2">
                <input type="checkbox" value={mv} bind:group={movements} />
                {mv}
              </label>
            {/each}
          </div>
        </div>

        <!-- Meal Type + End of Day -->
        <div class="grid grid-cols-2 gap-4">
          <div>
            <p class="font-semibold mb-2">🍔 Meal Type</p>
            <label class="flex items-center gap-2">
              <input type="radio" bind:group={fastFood} value="Healthy Meal" />
              Healthy
            </label>
            <label class="flex items-center gap-2">
              <input type="radio" bind:group={fastFood} value="Fast Food" />
              Fast Food
            </label>
          </div>
          <div>
            <p class="font-semibold mb-2">🌙 End of Day</p>
            {#each ["Energized ⚡", "Okay 😌", "Tired 😴"] as feeling}
              <label class="flex items-center gap-2">
                <input type="radio" bind:group={endOfDay} value={feeling} />
                {feeling}
              </label>
            {/each}
          </div>
        </div>

        <!-- Mood -->
        <div>
          <p class="font-semibold mb-2">😊 Mood</p>
          {#each ["Happy 🙂", "Calm 😌", "Neutral 😐", "Tired 😫", "Stressed 😟"] as m}
            <label class="inline-flex items-center gap-2 mr-3">
              <input type="radio" bind:group={mood} value={m} />
              {m}
            </label>
          {/each}
        </div>

        <!-- Notes -->
        <div>
          <p class="font-semibold">📝 Notes</p>
          <textarea
            bind:value={notes}
            rows="3"
            class="border p-2 w-full rounded-lg focus:ring-2 focus:ring-blue-500"
          ></textarea>
        </div>
      </div>

      <!-- Buttons -->
      <div class="flex justify-between pt-4 border-t">
        <button
          type="button"
          on:click={close}
          class="px-4 py-2 bg-gray-200 text-gray-700 rounded-lg hover:bg-gray-300"
        >
          Cancel
        </button>
        <button
          type="button"
          on:click={saveEdit}
          class="px-5 py-2 bg-blue-600 text-white font-semibold rounded-lg hover:bg-blue-700 disabled:opacity-50 disabled:cursor-not-allowed"
          disabled={!isEdited}
        >
          Save Changes
        </button>
      </div>
    </div>
  </div>
{/if}
