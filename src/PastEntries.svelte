<script>
  import { createEventDispatcher } from "svelte";
  import EditEntry from "./EditEntry.svelte";

  export let entries = [];
  const dispatch = createEventDispatcher();

  let modalOpen = false;
  let editingIndex = null;
  let editingEntry = null;

  function startEditing(index, entry) {
    editingIndex = index;
    editingEntry = { ...entry };
    modalOpen = true;
  }

  function saveEditedEntry(updated) {
    dispatch("updateEntry", { index: editingIndex, entry: updated });
    modalOpen = false;
  }

  function handleSeeMore() {
    dispatch("seeAll");
  }

  function parseDate(d) {
    if (!d) return new Date(0);

    if (d.includes("-")) {
      const [y, m, day] = d.split("-");
      return new Date(y, m - 1, day);
    }

    if (d.includes("/")) {
      const [day, m, y] = d.split("/");
      return new Date(y, m - 1, day);
    }

    return new Date(d);
  }

  $: sortedEntries = [...entries].sort(
    (a, b) => parseDate(b.date).getTime() - parseDate(a.date).getTime()
  );
</script>

<section class="bg-gray-50 rounded-2xl shadow p-6">
  <h2 class="text-xl font-bold text-blue-800 mb-6">📒 Recent Entries</h2>

  {#if sortedEntries.length === 0}
    <p class="text-gray-600 italic">No entries yet.</p>
  {:else}
    <div class="grid md:grid-cols-3 gap-6">
      {#each sortedEntries.slice(0, 3) as entry, i}
        <div
          class="p-5 bg-white rounded-xl shadow-sm border border-gray-200
                 hover:shadow-md transition-shadow duration-200
                 flex flex-col justify-between"
        >
          <div class="flex justify-between items-center mb-2">
            <p class="font-semibold">{entry.date}</p>
            <p class="text-sm text-gray-500">{entry.time}</p>
          </div>

          <!-- All saved details -->
          <div class="text-sm space-y-1 mb-3">
            {#if entry.sleep}<p>😴 Sleep: {entry.sleep} hrs</p>{/if}
            {#if entry.water}<p>💧 Water: {entry.water} cups</p>{/if}
            <p>🥗 Vegetables: {entry.vegetables ? "Yes" : "No"}</p>
            <p>🍎 Fruits: {entry.fruits ? "Yes" : "No"}</p>
            {#if entry.exercises?.length}<p>
                🏃 {entry.exercises.join(", ")}
              </p>{/if}
            {#if entry.movements?.length}<p>
                🚶 {entry.movements.join(", ")}
              </p>{/if}
            {#if entry.fastFood}<p>🍔 {entry.fastFood}</p>{/if}
            {#if entry.endOfDay}<p>🌙 {entry.endOfDay}</p>{/if}
            {#if entry.mood}<p>😊 {entry.mood}</p>{/if}
            {#if entry.notes}<p class="italic text-gray-600">
                📝 {entry.notes}
              </p>{/if}
          </div>

          <!-- Edit -->
          <div class="text-right">
            <button
              on:click={() => startEditing(i, entry)}
              class="px-3 py-1 border border-blue-500 text-blue-600 rounded
                     hover:bg-blue-50 cursor-pointer transition-colors duration-200"
            >
              Edit
            </button>
          </div>
        </div>
      {/each}
    </div>
  {/if}

  <!-- See More button -->
  {#if sortedEntries.length > 0}
    <div class="mt-8 text-center">
      <button
        on:click={handleSeeMore}
        class="px-5 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700
               cursor-pointer transition-colors duration-200"
      >
        📖 See All Entries →
      </button>
    </div>
  {/if}

  <!-- Edit Modal -->
  <EditEntry
    bind:open={modalOpen}
    entry={editingEntry}
    index={editingIndex}
    on:save={(e) => saveEditedEntry(e.detail)}
    on:close={() => (modalOpen = false)}
  />
</section>
