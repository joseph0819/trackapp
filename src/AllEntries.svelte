<script>
  import { createEventDispatcher } from "svelte";
  import EditEntry from "./EditEntry.svelte";
  import Footer from "./Footer.svelte";

  export let entries = [];
  const dispatch = createEventDispatcher();

  let modalOpen = false;
  let editingIndex = null;
  let editingEntry = null;
  let selectedDate = "";

  function startEditing(index, entry) {
    editingIndex = index;
    editingEntry = { ...entry };
    modalOpen = true;
  }

  function saveEditedEntry(updated) {
    dispatch("updateEntry", { index: editingIndex, entry: updated });
    modalOpen = false;
  }

  function goBack() {
    dispatch("back");
  }

  function parseDateTime(entry) {
    return entry?.timestamp ? new Date(entry.timestamp) : new Date(0);
  }

  
  $: sortedEntries = [...entries].sort(
    (a, b) => parseDateTime(b).getTime() - parseDateTime(a).getTime()
  );

  $: filteredEntries = selectedDate
    ? sortedEntries.filter(
        (e) => parseDateTime(e).toISOString().split("T")[0] === selectedDate
      )
    : sortedEntries;

  // Stats
  $: totalEntries = sortedEntries.length;
  $: latestEntry = sortedEntries.length
    ? new Date(sortedEntries[0].timestamp).toLocaleDateString("en-CA")
    : null;
  $: earliestEntry = sortedEntries.length
    ? new Date(sortedEntries[sortedEntries.length - 1].timestamp).toLocaleDateString("en-CA")
    : null;
  $: avgSleep = (
    sortedEntries.reduce((sum, e) => sum + Number(e.sleep || 0), 0) /
    (sortedEntries.length || 1)
  ).toFixed(1);
  $: avgWater = (
    sortedEntries.reduce((sum, e) => sum + Number(e.water || 0), 0) /
    (sortedEntries.length || 1)
  ).toFixed(1);
</script>

<div class="min-h-screen bg-gray-50 flex flex-col">
  <!-- Header -->
  <header class="bg-blue-600 text-white py-6 px-8 flex justify-between items-center">
    <h1 class="text-2xl font-extrabold">📊 All Entries</h1>
    <button
      on:click={goBack}
      class="px-4 py-2 bg-white text-blue-700 rounded hover:bg-gray-100"
    >
      ← Back
    </button>
  </header>

  <!-- Stats Board -->
  <section class="bg-blue-600 text-white py-8">
    <div class="max-w-6xl mx-auto px-6">
      <h2 class="text-xl font-bold mb-6">🌟 See how far you’ve come</h2>
      <div class="grid md:grid-cols-5 gap-6 text-center">
        <div class="bg-blue-700 rounded-xl p-5 shadow hover:scale-[1.02] transition">
          <h3 class="text-sm opacity-80">Total Entries</h3>
          <p class="text-2xl font-bold">{totalEntries}</p>
        </div>
        <div class="bg-blue-700 rounded-xl p-5 shadow hover:scale-[1.02] transition">
          <h3 class="text-sm opacity-80">Latest Entry</h3>
          <p class="text-lg font-semibold">{latestEntry || "N/A"}</p>
        </div>
        <div class="bg-blue-700 rounded-xl p-5 shadow hover:scale-[1.02] transition">
          <h3 class="text-sm opacity-80">Earliest Entry</h3>
          <p class="text-lg font-semibold">{earliestEntry || "N/A"}</p>
        </div>
        <div class="bg-blue-700 rounded-xl p-5 shadow hover:scale-[1.02] transition">
          <h3 class="text-sm opacity-80">Avg Sleep</h3>
          <p class="text-lg">{avgSleep} hrs</p>
        </div>
        <div class="bg-blue-700 rounded-xl p-5 shadow hover:scale-[1.02] transition">
          <h3 class="text-sm opacity-80">Avg Water</h3>
          <p class="text-lg">{avgWater} cups</p>
        </div>
      </div>
    </div>
  </section>

  <!-- Date Filter -->
  <div class="max-w-6xl px-6 py-6 flex items-center">
    <div>
      <div class="block mb-2 font-medium">📅 Filter by date:</div>
      <input type="date" bind:value={selectedDate} class="border rounded px-3 py-2" />
      {#if selectedDate}
        <button
          on:click={() => (selectedDate = "")}
          class="ml-3 text-blue-600 underline cursor-pointer"
        >
          Clear
        </button>
      {/if}
    </div>
  </div>

  <!-- Entries -->
  <main class="min-w-6xl mx-auto p-8 flex-1">
    {#if filteredEntries.length === 0}
      <p class="italic text-gray-600">No entries for this date.</p>
    {:else}
      <div class="grid md:grid-cols-3 gap-6">
        {#each filteredEntries as entry}
          <div
            class="p-5 bg-white rounded-xl shadow transition hover:shadow-lg flex flex-col justify-between"
          >
            <!-- Date + Time -->
            <div class="flex justify-between items-center mb-2">
              <p class="font-semibold">
                {new Date(entry.timestamp).toLocaleDateString("en-CA")}
              </p>
              <p class="text-sm text-gray-500">
                {new Date(entry.timestamp).toLocaleTimeString([], { hour: "2-digit", minute: "2-digit" })}
              </p>
            </div>

            <!-- Entry Details -->
            <div class="text-sm space-y-1 mb-3">
              {#if entry.sleep}<p>😴 Sleep: {entry.sleep} hrs</p>{/if}
              {#if entry.water}<p>💧 Water: {entry.water} cups</p>{/if}
              <p>🥗 Vegetables: {entry.vegetables ? "Yes" : "No"}</p>
              <p>🍎 Fruits: {entry.fruits ? "Yes" : "No"}</p>
              {#if entry.exercises?.length}<p>🏃 {entry.exercises.join(", ")}</p>{/if}
              {#if entry.movements?.length}<p>🚶 {entry.movements.join(", ")}</p>{/if}
              {#if entry.fastFood}<p>🍔 {entry.fastFood}</p>{/if}
              {#if entry.endOfDay}<p>🌙 {entry.endOfDay}</p>{/if}
              {#if entry.mood}<p>😊 {entry.mood}</p>{/if}
              {#if entry.notes}<p class="italic text-gray-600">📝 {entry.notes}</p>{/if}
            </div>

            <!-- Edit Button -->
            <div class="text-right">
              <button
                on:click={() => startEditing(entries.indexOf(entry), entry)}
                class="px-3 py-1 border border-blue-500 text-blue-600 rounded
                       hover:bg-blue-50 cursor-pointer transition"
              >
                Edit
              </button>
            </div>
          </div>
        {/each}
      </div>
    {/if}

    <!-- Back -->
    <div class="mt-10 text-center">
      <button
        on:click={goBack}
        class="px-5 py-2 bg-gray-200 text-gray-700 rounded-lg hover:bg-gray-300"
      >
        ← Back to Dashboard
      </button>
    </div>
  </main>

  <!-- Edit Modal -->
  <EditEntry
    bind:open={modalOpen}
    entry={editingEntry}
    index={editingIndex}
    on:save={(e) => saveEditedEntry(e.detail)}
    on:close={() => (modalOpen = false)}
  />

  <Footer />
</div>
