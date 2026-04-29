<template class = "template">
  <div class="grid">
    <div 
      v-for="(tile, index) in tiles" 
      :key="index"
      class="tile"
      :class="{ empty: tile === null }"
      :style="tileStyle(tile)"
      @click="moveTile(index)"
    >
     <div 
  v-for="(tile, index) in tiles" 
  :key="index"
  class="tile"
  :class="{ empty: tile === null }"
  :style="tileStyle(tile)"
  @click="moveTile(index)"
>
</div>
<span v-if="tile !== null && !won" class="number">{{ tile }}</span>
    </div>
  </div>

  <p>Moves: {{ moves }}</p>

  <button @click="shuffle" class="puzzlebtns">Restart</button>

 <p>Level: {{ level + 1 }} / {{ images.length }}</p>

<div v-if="won" class="win">
  <h2>👍 Puzzle Completed!</h2>
  <p>Moves: {{ moves }}</p>
  <p>{{ facts[level] }}</p>

  <button @click="nextLevel">
    Next Level
  </button>
</div>
</template>

<script>
export default {
  data() {
  return {
    tiles: [],
    moves: 0,
    won: false,
    level: 0,

    images: [
      '/images/gicanda.jpg',
      '/images/Musinga.jpg',
      '/images/Mutara-III-RUDAHIGWA.jpg',
      '/images/Mutudzi.jpg',
      '/images/Queen-Bakayishonga.jpg',
      '/images/agathe-uwilingiyimana.jpeg',
      '/images/Kigeli.webp',
      '/images/Michel.png',
      '/images/Mwami_Kigeri_IV_Rwabugiri.jpg',
      '/images/King_Gihanga.png'
    ],

    facts: [
      "He became King at age 12: Musinga took the throne in December 1896, shortly after his father, King Kigeli IV Rwabugiri, died, following a coup spearheaded by his mother, Kanjogera, and his uncles.",
      "The Chosen One: According to local legend, she was selected for the King during a national gathering of girls in Nyanza because of her exceptional beauty and physical perfection.",
      "Africa's Tallest Leader: Standing at 2.1 meters (6'9\"), he is widely cited as Africa`s tallest leader in history. Historical photos often show him towering over Belgian officials during his visits to Europe.",
      "The Ultimate Status Symbol: This wasn't just a haircut; it was a complex architectural style that signaled a person's role in society. For men, it represented strength, bravery, and nobility.",
      "Fashion Icon: She is often remembered for her impeccable style, frequently photographed wearing the icyanganga (a beaded headband exclusive to royalty) and the umushanana.",
      "The Chemistry Professor: Long before she was a politician, she was a scientist. She taught chemistry at the National University of Rwanda and was one of the first women in the country to earn a bachelor's degree in science (1985).",
      "Single by Custom: Kigeli never married or had children. This was a deliberate choice to follow a royal tradition that forbade a King from marrying while in exile.",
      "Rwanda values resilience and unity.",
      "Historical leaders shaped modern Rwanda.",
      "Heritage connects all generations."
    ]
  }
},

  methods: {

    tileStyle(tile) {
  if (tile === null) {
    return { background: 'transparent' }
  }

  return {
    backgroundImage: `url('${this.images[this.level]}')`,
    backgroundSize: '300px 300px',
    backgroundPosition: this.getPosition(tile)
  }
},

getPosition(tile) {
  const pos = tile - 1
  const x = (pos % 3) * 100
  const y = Math.floor(pos / 3) * 100
  return `-${x}px -${y}px`
},

   shuffle() {
  this.tiles = [1,2,3,4,5,6,7,8,null]
  this.moves = 0
  this.won = false

  for (let i = 0; i < 100; i++) {
    let empty = this.tiles.indexOf(null)
    let neighbors = this.getNeighbors(empty)
    let rand = neighbors[Math.floor(Math.random() * neighbors.length)]
    this.swap(empty, rand)
  }
},

    moveTile(index) {
      let empty = this.tiles.indexOf(null)
      let neighbors = this.getNeighbors(empty)

      if (neighbors.includes(index)) {
        this.swap(index, empty)
        this.moves++
        this.checkWin()
      }
    },

    swap(i, j) {
      [this.tiles[i], this.tiles[j]] = [this.tiles[j], this.tiles[i]]
    },

    getNeighbors(i) {
      let n = []
      let r = Math.floor(i / 3)
      let c = i % 3

      if (r > 0) n.push(i - 3)
      if (r < 2) n.push(i + 3)
      if (c > 0) n.push(i - 1)
      if (c < 2) n.push(i + 1)

      return n
    },

    checkWin() {
  const correct = [1,2,3,4,5,6,7,8,null]

  if (JSON.stringify(this.tiles) === JSON.stringify(correct)) {
    this.won = true
  }
},
nextLevel() {
  this.level++

  if (this.level >= this.images.length) {
    this.level = 0 // restart or end game
  }

  this.won = false
  this.moves = 0
  this.tiles = []

  this.$nextTick(() => {
    this.shuffle()
  })
}
  }
,

  mounted() {
    this.shuffle()
  }
}
</script>

<style>
.template {
    background: black;
}
.grid {
  width: 400px;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 5px;
  margin: auto;
}
.number {
  position: absolute;
  width: 25px;
  height: 25px;
  transform: translate(-100%, -100%);
  color: white;
  font-size: 14px;
  font-weight: bold;
  background: rgba(0, 0, 0, 0.7);
  border-radius: 50%;
  padding: 2px;
}
.tile {
  height: 150px;
  background: #1449c6;
  display: flex;
  justify-content: center;
  align-items: center;
  color: white;
  font-size: 24px;
  cursor: pointer;
}
.puzzlebtns {
  font-family: inherit; 
  font-size: .88rem; 
  font-weight: 600;
  border: 1.5px solid #3d1c00;
  border-radius: 10px;
  padding: 10px 18px;
  cursor: pointer; 
  transition: opacity .15s, transform .1s;
}
.puzzlebtns:hover { opacity: .85; }
.puzzlebtns:active { transform: scale(.97); }
.puzzlebtns--primary { 
  background: #d4af37; 
  color: #0f0900; 
}
.puzzlebtns--danger { 
  background: #b01c30; 
  color: #fff; 
}
.puzzlebtns--ghost{ 
  background: rgba(255,255,255,.07); 
  color: #fff3d6; 
  border: 1.5px solid #3d1c00; 
}
.empty {
  background: transparent;
}
</style>