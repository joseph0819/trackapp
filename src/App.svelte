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

  // ✅ Exactly 20 dummy entries
  let entries = [
    { date: "2025-09-20", time: "07:30 AM", sleep: "7", water: "8", vegetables: true, fruits: true, exercises: ["Run"], mood: "Happy 🙂", notes: "Morning jog felt great!" },
    { date: "2025-09-19", time: "08:00 PM", sleep: "6", water: "5", vegetables: false, fruits: true, exercises: [], mood: "Tired 😫", notes: "Busy work day." },
    { date: "2025-09-18", time: "09:15 PM", sleep: "8", water: "9", vegetables: true, fruits: false, exercises: ["Walk"], mood: "Calm 😌", notes: "Relaxing evening." },
    { date: "2025-09-17", time: "07:20 AM", sleep: "7", water: "6", vegetables: true, fruits: true, exercises: [], mood: "Neutral 😐", notes: "Steady day." },
    { date: "2025-09-16", time: "06:45 AM", sleep: "5", water: "4", vegetables: false, fruits: false, exercises: [], mood: "Stressed 😟", notes: "Tough workload." },
    { date: "2025-09-15", time: "08:10 AM", sleep: "9", water: "10", vegetables: true, fruits: true, exercises: ["Yoga"], mood: "Energized ⚡", notes: "Great sleep!" },
    { date: "2025-09-14", time: "07:00 AM", sleep: "6", water: "7", vegetables: true, fruits: false, exercises: ["Weights"], mood: "Happy 🙂", notes: "Good balance." },
    { date: "2025-09-13", time: "09:20 PM", sleep: "7", water: "6", vegetables: false, fruits: true, exercises: [], mood: "Tired 😴", notes: "Skipped workout." },
    { date: "2025-09-12", time: "10:00 PM", sleep: "8", water: "8", vegetables: true, fruits: true, exercises: ["Run"], mood: "Calm 😌", notes: "Peaceful." },
    { date: "2025-09-11", time: "07:45 AM", sleep: "6", water: "5", vegetables: false, fruits: false, exercises: [], mood: "Neutral 😐", notes: "Average day." },
    { date: "2025-09-10", time: "08:15 PM", sleep: "7", water: "9", vegetables: true, fruits: true, exercises: ["Walk"], mood: "Happy 🙂", notes: "Strong finish." },
    { date: "2025-09-09", time: "07:05 AM", sleep: "8", water: "7", vegetables: true, fruits: true, exercises: ["Yoga"], mood: "Calm 😌", notes: "Smooth routine." },
    { date: "2025-09-08", time: "09:00 PM", sleep: "5", water: "4", vegetables: false, fruits: false, exercises: [], mood: "Stressed 😟", notes: "Hard day." },
    { date: "2025-09-07", time: "08:30 AM", sleep: "9", water: "11", vegetables: true, fruits: true, exercises: ["Gym"], mood: "Energized ⚡", notes: "Productive!" },
    { date: "2025-09-06", time: "07:50 AM", sleep: "6", water: "5", vegetables: false, fruits: true, exercises: [], mood: "Neutral 😐", notes: "Felt normal." },
    { date: "2025-09-05", time: "07:25 AM", sleep: "8", water: "9", vegetables: true, fruits: true, exercises: ["Run"], mood: "Happy 🙂", notes: "Workout success." },
    { date: "2025-09-04", time: "08:40 PM", sleep: "7", water: "6", vegetables: true, fruits: false, exercises: ["Weights"], mood: "Calm 😌", notes: "Relaxed evening." },
    { date: "2025-09-03", time: "09:05 PM", sleep: "6", water: "7", vegetables: false, fruits: true, exercises: [], mood: "Tired 😴", notes: "Worked late." },
    { date: "2025-09-02", time: "07:15 AM", sleep: "7", water: "8", vegetables: true, fruits: true, exercises: ["Yoga"], mood: "Energized ⚡", notes: "Fresh start." },
    { date: "2025-09-01", time: "08:55 PM", sleep: "5", water: "4", vegetables: false, fruits: false, exercises: [], mood: "Stressed 😟", notes: "Drained out." }
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
