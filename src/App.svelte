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
    {
      date: "2025-09-19",
      time: "07:30 AM",
      sleep: "7",
      water: "8",
      vegetables: true,
      fruits: true,
      exercises: ["Run"],
      mood: "Happy 🙂",
      notes: "Morning jog felt great!",
    },
    {
      date: "2025-09-18",
      time: "08:00 PM",
      sleep: "6",
      water: "5",
      vegetables: false,
      fruits: true,
      exercises: [],
      mood: "Tired 😫",
      notes: "Busy work day.",
    },
    {
      date: "2025-09-17",
      time: "09:15 PM",
      sleep: "8",
      water: "9",
      vegetables: true,
      fruits: false,
      exercises: ["Walk"],
      mood: "Calm 😌",
      notes: "Relaxing evening.",
    },
    // … rest of entries stay as-is …
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
