<script setup>
import { ref, computed, onMounted } from "vue";

const wins = ref(0);
const losses = ref(0);
const draws = ref(0);

const choice = ref(null);
const computerChoice = ref(null);
const verdict = ref(null);

const outcomes = {
  rock: {
    rock: "draw",
    paper: "loss",
    scissors: "win",

  },
  paper: {
    rock: "win",
    paper: "draw",
    scissors: "loss",
  },
  scissors: {
    rock: "loss",
    paper: "win",
    scissors: "draw",
  },
};

const winPercentage = computed(() => {
  const total = wins.value + losses.value + draws.value;
  return total ? (wins.value / total) * 100 : 0;
});

const play = (c) => {
  choice.value = c;

  const choices = ["rock", "paper", "scissors"];
  const random = Math.floor(Math.random() * choices.length);
  computerChoice.value = choices[random];

  const outcome = outcomes[c][computerChoice.value];

  if (outcome === "win") {
    wins.value++;
    verdict.value = "You win!";
  } else if (outcome === "loss") {
    losses.value++;
    verdict.value = "You looses!";
  } else {
    draws.value++;
    verdict.value = "It is a draw!";
  }
  SaveGame();
};

const SaveGame = () => {
  localStorage.setItem("wins", wins.value);
  localStorage.setItem("draws", draws.value);
  localStorage.setItem("losses", losses.value);
};

const LoadGame = () => {
  wins.value = parseInt(localStorage.getItem("wins")) || 0
  draws.value = parseInt(localStorage.getItem("draws")) || 0
  losses.value = parseInt(localStorage.getItem("losses")) || 0
};

const ResetRound = () => {
  choice.value = null;
  computerChoice.value = null;
  verdict.value = null;
};

onMounted(() => {
  LoadGame();

  window.addEventListener("keypress", (e) => {
    if (e.key == " ") {
      ResetRound();
    }
  });
});
</script>

<template class="overflow-hidden">
  <div class="bg-gray-700 text-white text-center min-h-screen flex flex-col">
    <header>
      <h1
        class="text-3xl sm:text-xl md:text-3xl lg:text-6xl font-extrabold cursor-pointer text-gray-50 opacity-30 hover:opacity-90 transition duration-200 py-8 sm:py-10 md:py-12 lg:py-20 hover:text-white">
        Rock Paper Scissor
      </h1>
    </header>
    <main class="container mx-auto flex-1 lg:mt-20 mt-10">
      <div v-if="choice === null" class="flex items-center justify-center">
        <button @click="play('rock')"
          class="bg-white rounded-full shadow-lg w-64 p-2 mx-6 transition-colors duration-300 hover:bg-orange-300">
          <img src="./assets/rock.svg" alt="Rock" srcset="" />
        </button>
        <button @click="play('paper')"
          class="bg-white rounded-full shadow-lg w-64 p-2 mx-6 transition-colors duration-300 hover:bg-green-400">
          <img src="./assets/paper.svg" alt="paper" srcset="" />
        </button>
        <button @click="play('scissors')"
          class="bg-white rounded-full shadow-lg w-64 p-2 mx-6 transition-colors duration-300 hover:bg-orange-500">
          <img src="./assets/scissor.svg" alt="scissors" srcset="" />
        </button>
      </div>
      <div v-else>
        <div class="text-3xl mb-4">
          You Picked: <span class="text-green-500 font-bold capitalize">'{{choice}}'</span>
        </div>
        <div class="text-3xl mb-4">
          Computer Picked: <span class="text-red-500 font-bold capitalize">'{{computerChoice}}'</span>
        </div>
        <div class="text-6xl font-bold text-blue-500 mb-12">
          {{verdict}}
        </div>
        <button @click="ResetRound" class="bg-pink-500 rounded text-lg py-4 px-10 hover:opacity-80">Reset</button>
      </div>

      <!-- Win Draw Loss Table -->
      <div class=" flex justify-center mx-auto py-20">
        <div class="flex flex-col">
          <div class="w-full">
            <div class="border-b border-gray-200 shadow">
              <table class="divide-y divide-gray-300 ">
                <thead class="bg-gray-900 ">
                  <tr>

                    <th class="px-6 py-2 text-xl sm:text-xs md:text-2xl lg:text-3xl font-extrabold text-gray-500">
                      Win
                    </th>
                    <th class="px-6 py-2 text-xl sm:text-xs md:text-2xl lg:text-3xl font-extrabold text-gray-500">
                      Draw
                    </th>
                    <th class="px-6 py-2 text-xl sm:text-xs md:text-2xl lg:text-3xl font-extrabold text-gray-500">
                      Loss
                    </th>

                  </tr>
                </thead>
                <tbody class="bg-white divide-y divide-gray-300">


                  <tr class="whitespace-nowrap bg-gray-800">

                    <td class="px-6 py- border-r-2">
                      <div class="text-xl sm:text-xs md:text-2xl lg:text-3xl font-extrabold">
                        <span class="text-green-400"> {{wins}}</span>
                      </div>
                    </td>
                    <td class="px-6 py-4 border-r-2">
                      <div class="text-xl sm:text-xs md:text-2xl lg:text-3xl font-extrabold">
                        <span class="text-blue-500">{{draws}}</span>
                      </div>
                    </td>
                    <td class="px-6 py-4 text-xl sm:text-xs md:text-2xl lg:text-3xl font-extrabold">
                      <span class="text-red-500">{{losses}}</span>
                    </td>

                  </tr>
                </tbody>
              </table>
            </div>
          </div>
        </div>
      </div>
      <!-- End Win Draw Loss Table -->


      <!-- Percentage -->
      <div class="text-xl sm:text-xl md:text-3xl lg:text-6xl font-extrabold text-green-500">
        Win Rate: <span class="text-white">{{Math.round(winPercentage)}}%</span>
      </div>
      <!--End Percentage -->

    </main>
  </div>
</template>
