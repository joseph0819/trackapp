<script>
  import { createEventDispatcher } from "svelte";
  import { toast } from "svelte-sonner"; 

  export let settings = {};

  const dispatch = createEventDispatcher();
  let saved = false;
  let open = false;

  const activities = [
    { key: "trackSleep", label: "😴 Sleep" },
    { key: "trackWater", label: "💧 Water" },
    { key: "trackVegetables", label: "🥗 Vegetables" },
    { key: "trackFruits", label: "🍎 Fruits" },
    { key: "trackExercise", label: "🏃 Exercise" },
    { key: "trackMovements", label: "🚶 Movements" },
    { key: "trackFastFood", label: "🍔 Meal Type" },
    { key: "trackEndOfDay", label: "🌙 End of Day" },
    { key: "trackMood", label: "😊 Mood" },
    { key: "trackNotes", label: "📝 Notes" },
  ];

  $: activeCount = Object.values(settings).filter(Boolean).length;

  function saveSettings() {
    if (activeCount < 4) {
      toast.error("⚠ Please select at least 4 activities before saving.");
      return;
    }
    localStorage.setItem("settings", JSON.stringify(settings));
    dispatch("saveSettings", settings);
    saved = true;
    toast.success("✅ Settings saved successfully!");
    setTimeout(() => (saved = false), 2000);
  }

  function resetSettings() {
    settings = {
      trackSleep: true,
      trackWater: true,
      trackVegetables: true,
      trackFruits: true,
      trackExercise: true,
      trackMovements: true,
      trackFastFood: true,
      trackEndOfDay: true,
      trackMood: true,
      trackNotes: true,
    };
    saveSettings();
  }
</script>

<div class="mb-4">
  <button
    class="px-5 py-2 bg-blue-600 text-white text-sm font-semibold rounded-lg shadow hover:bg-blue-700 transition cursor-pointer"
    on:click={() => (open = !open)}
  >
    {open ? "✖ Close Settings" : "⚙️ Select Entries to Track"}
  </button>
</div>

{#if open}
  <div class="bg-blue-700 rounded-2xl shadow-lg p-6 text-white">
    <h2 class="text-lg font-bold mb-4">✨ Choose Activities</h2>
    <p class="text-sm text-blue-200 mb-4">
      Pick what you want to log today (at least 4):
    </p>

    <!-- Activities grid -->
    <div class="grid md:grid-cols-2 gap-4 mb-6">
      {#each activities as act}
        <label
          class="relative flex items-center justify-between p-4 rounded-lg transition
                   {settings[act.key]
            ? 'bg-blue-600 hover:bg-blue-500'
            : 'bg-blue-300 opacity-70'}"
        >
          <span class="font-semibold">{act.label}</span>
          <span
            class="text-xs font-semibold
                         {settings[act.key]
              ? 'text-green-300'
              : 'text-red-200'}"
          >
            {settings[act.key] ? "✅ Tracking" : "🚫 Not tracking"}
          </span>
          <input
            type="checkbox"
            bind:checked={settings[act.key]}
            class="hidden"
          />
        </label>
      {/each}
    </div>

    <div class="flex justify-between items-center">
      <button
        on:click={resetSettings}
        class="px-4 py-2 bg-gray-200 hover:bg-gray-300 text-gray-800 text-sm rounded-lg"
      >
        Reset Defaults
      </button>
      <button
        on:click={saveSettings}
        class="px-5 py-2 text-sm font-semibold rounded-lg shadow transition
                 {activeCount < 4
          ? 'bg-gray-400 text-gray-200 cursor-not-allowed'
          : 'bg-green-500 text-white hover:bg-green-600'}"
      >
        Save
      </button>
    </div>
  </div>
{/if}
