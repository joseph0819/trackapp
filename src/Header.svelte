<script>
  export let userName = "";
  export let startDate;
  export let entries = [];

  let time = new Date().toLocaleTimeString([], {
    hour: "2-digit",
    minute: "2-digit",
  });
  let date = new Date().toLocaleDateString(undefined, {
    weekday: "long",
    month: "long",
    day: "numeric",
  });

  // update time every 30s
  const interval = setInterval(() => {
    time = new Date().toLocaleTimeString([], {
      hour: "2-digit",
      minute: "2-digit",
    });
  }, 30000);

  import { onDestroy } from "svelte";
  onDestroy(() => clearInterval(interval));

  $: start = new Date(startDate);
  $: daysSinceBeginning = Math.ceil(
    (Date.now() - start.getTime()) / (1000 * 60 * 60 * 24)
  );
  $: daysActive = new Set(entries.map((e) => e.date)).size;
</script>

<header class="sticky top-0 bg-white shadow-md z-30">
  <div class="max-w-7xl mx-auto px-6 py-6 flex items-center justify-between">
    <!-- Profile -->
    <div class="flex items-center gap-4">
      <div
        class="w-14 h-14 rounded-full bg-blue-600 flex items-center justify-center text-white font-bold text-xl shadow"
      >
        {userName[0]}
      </div>
      <div>
        <p class="text-xl font-bold text-gray-700">
          Welcome back,
          <span class="text-2xl font-extrabold text-blue-600"> {userName}</span>
        </p>
      </div>
    </div>

    <!-- Navbar -->
    <nav>
      <div
        class="max-w-6xl mx-auto px-6 py-3 flex flex-wrap gap-6 text-sm font-medium text-gray-700"
      >
        <a href="#overview" class="hover:text-blue-600">Overview</a>
        <a href="#goals" class="hover:text-blue-600">Goals</a>
        <a href="#newentry" class="hover:text-blue-600">New Entry</a>
        <a href="#pastentries" class="hover:text-blue-600">Past Entries</a>
      </div>
    </nav>
  </div>

  <!-- ✅ Date and Time -->
  <div class="bg-gray-50 border-t border-gray-200">
    <div
      class="max-w-7xl mx-auto px-6 py-3 flex items-center justify-between text-sm text-gray-700"
    >
      <div class="flex gap-6">
        <p>
          ✅ Days active: <span class="font-semibold">{daysActive}</span>
        </p>
      </div>
      <div class="text-right">
        <p class="text-sm font-medium text-gray-500">{date}</p>
        <p class="text-lg font-semibold text-blue-700">{time}</p>
      </div>
    </div>
  </div>
</header>
