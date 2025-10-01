<script>
  import Header from "./Header.svelte";
  import Overview from "./Overview.svelte";
  import Goals from "./Goals.svelte";
  import Settings from "./Settings.svelte";
  import NewEntry from "./NewEntry.svelte";
  import PastEntries from "./PastEntries.svelte";
  import AllEntries from "./AllEntries.svelte";
  import Footer from "./Footer.svelte";

  import { toast, Toaster } from "svelte-sonner";

  let userName = "Benjamin Frank";
  let startDate = new Date("2025-09-01");

  let view = "main";

  let goals = {
    sleep: 7,
    water: 8,
    vegetables: true,
    fruits: true,
    exercise: true,
    mood: true,
  };

  let settings = {
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

  let entries = [
    { timestamp: "2025-09-20T07:30:00", sleep: "7", water: "8", vegetables: true, fruits: true, exercises: ["Run"], mood: "Happy 🙂", notes: "Morning jog felt great!" },
    { timestamp: "2025-09-19T20:00:00", sleep: "6", water: "5", vegetables: false, fruits: true, exercises: [], mood: "Tired 😫", notes: "Busy work day." },
    { timestamp: "2025-09-18T21:15:00", sleep: "8", water: "9", vegetables: true, fruits: false, exercises: ["Walk"], mood: "Calm 😌", notes: "Relaxing evening." },
    { timestamp: "2025-09-17T07:20:00", sleep: "7", water: "6", vegetables: true, fruits: true, exercises: [], mood: "Neutral 😐", notes: "Steady day." },
    { timestamp: "2025-09-16T06:45:00", sleep: "5", water: "4", vegetables: false, fruits: false, exercises: [], mood: "Stressed 😟", notes: "Tough workload." },
    { timestamp: "2025-09-15T08:10:00", sleep: "9", water: "10", vegetables: true, fruits: true, exercises: ["Yoga"], mood: "Energized ⚡", notes: "Great sleep!" },
    { timestamp: "2025-09-14T07:00:00", sleep: "6", water: "7", vegetables: true, fruits: false, exercises: ["Weights"], mood: "Happy 🙂", notes: "Good balance." },
    { timestamp: "2025-09-13T21:20:00", sleep: "7", water: "6", vegetables: false, fruits: true, exercises: [], mood: "Tired 😴", notes: "Skipped workout." },
    { timestamp: "2025-09-12T22:00:00", sleep: "8", water: "8", vegetables: true, fruits: true, exercises: ["Run"], mood: "Calm 😌", notes: "Peaceful." },
    { timestamp: "2025-09-11T07:45:00", sleep: "6", water: "5", vegetables: false, fruits: false, exercises: [], mood: "Neutral 😐", notes: "Average day." },
    { timestamp: "2025-09-10T20:15:00", sleep: "7", water: "9", vegetables: true, fruits: true, exercises: ["Walk"], mood: "Happy 🙂", notes: "Strong finish." },
    { timestamp: "2025-09-09T07:05:00", sleep: "8", water: "7", vegetables: true, fruits: true, exercises: ["Yoga"], mood: "Calm 😌", notes: "Smooth routine." },
    { timestamp: "2025-09-08T21:00:00", sleep: "5", water: "4", vegetables: false, fruits: false, exercises: [], mood: "Stressed 😟", notes: "Hard day." },
    { timestamp: "2025-09-07T08:30:00", sleep: "9", water: "11", vegetables: true, fruits: true, exercises: ["Gym"], mood: "Energized ⚡", notes: "Productive!" },
    { timestamp: "2025-09-06T07:50:00", sleep: "6", water: "5", vegetables: false, fruits: true, exercises: [], mood: "Neutral 😐", notes: "Felt normal." },
    { timestamp: "2025-09-05T07:25:00", sleep: "8", water: "9", vegetables: true, fruits: true, exercises: ["Run"], mood: "Happy 🙂", notes: "Workout success." },
    { timestamp: "2025-09-04T20:40:00", sleep: "7", water: "6", vegetables: true, fruits: false, exercises: ["Weights"], mood: "Calm 😌", notes: "Relaxed evening." },
    { timestamp: "2025-09-03T21:05:00", sleep: "6", water: "7", vegetables: false, fruits: true, exercises: [], mood: "Tired 😴", notes: "Worked late." },
    { timestamp: "2025-09-02T07:15:00", sleep: "7", water: "8", vegetables: true, fruits: true, exercises: ["Yoga"], mood: "Energized ⚡", notes: "Fresh start." },
    { timestamp: "2025-09-01T20:55:00", sleep: "5", water: "4", vegetables: false, fruits: false, exercises: [], mood: "Stressed 😟", notes: "Drained out." }
  ];

  function handleSetGoals(event) {
    goals = event.detail;
  }

  function handleSave(event) {
    const newEntry = event.detail;
    entries = [...entries, newEntry];
    toast.success("✅ New entry saved!");
  }

  function handleUpdateEntry({ detail }) {
    entries[detail.index] = detail.entry;
    entries = [...entries];
    toast.success("✏ Entry updated!");
  }

  function handleSeeAll() {
    view = "all";
  }

  function handleBack() {
    view = "main";
  }

  function handleNavigate(section) {
    const el = document.getElementById(section);
    if (el) el.scrollIntoView({ behavior: "smooth" });
  }
</script>

<main class="bg-gray-100 min-h-screen">
  <div class="max-w-6xl mx-auto w-full space-y-8">
    {#if view === "main"}
      <Header {userName} {startDate} {entries} />

      <section id="overview" class="scroll-mt-28">
        <Overview {entries} {goals} {settings} />
      </section>

      <section id="goals" class="scroll-mt-28">
        <Goals {goals} on:setGoals={handleSetGoals} />
      </section>

      <section id="settings" class="scroll-mt-28">
        <Settings {settings} on:saveSettings={(e) => (settings = e.detail)} />
      </section>

      <section id="newentry" class="scroll-mt-28">
        <NewEntry {settings} on:save={handleSave} />
      </section>

      <section id="pastentries" class="scroll-mt-28">
        <PastEntries
          {entries}
          on:updateEntry={handleUpdateEntry}
          on:seeAll={handleSeeAll}
        />
      </section>

      <Footer on:navigate={handleNavigate} />
    {:else if view === "all"}
      <AllEntries
        {entries}
        on:updateEntry={handleUpdateEntry}
        on:back={handleBack}
      />
    {/if}
  </div>

  <!-- ✅ Sonner toaster -->
  <Toaster position="top-right" />
</main>
