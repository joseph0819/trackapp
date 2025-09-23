<script>
  import { createEventDispatcher } from "svelte";
  let year = new Date().getFullYear();
  const dispatch = createEventDispatcher();
</script>

<footer class="bg-blue-700 mt-10">
  <div class="max-w-6xl mx-auto px-6 py-6 text-center text-white space-y-3">
    <p class="font-semibold">🌟 Keep going, you’re doing great!</p>

    <p class="text-xs opacity-80">
      © {year} JournalApp
    </p>
  </div>
</footer>
