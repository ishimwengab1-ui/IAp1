<script>
import PuzzleBoard from './components/PuzzleBoard.vue'

const FIGURES = [
  {
    id: 0,
    name: 'Queen Rosablie Gicanda',
    years: 'c. 1853 – 1895',
    color: '#8B0000',
    symbol: '⚔️',
    funFact: 'The Chosen One: According to local legend, she was selected for the King during a national gathering of girls in Nyanza because of her exceptional beauty and physical perfection.',
    info: 'One of Rwanda\'s greatest warrior queens, she expanded the kingdom to its largest territorial extent and reorganized both the army and royal administration — transforming Rwanda into the dominant regional power of her era.'
  },
  {
    id: 1,
    name: 'King Yuhi V Musinga',
    years: '1912 – 1981',
    color: '#1A2B5C',
    symbol: '📖',
    funFact: 'He became King at age 12: Musinga took the throne in December 1896, shortly after his father, King Kigeli IV Rwabugiri, died, following a coup spearheaded by his mother, Kanjogera, and his uncles.',
    info: 'A Catholic priest, philosopher, historian, and poet, Kagame was the first person to record Rwandan oral traditions and royal poetry in written form. His work in African philosophy earned him international acclaim.'
  },
  {
    id: 2,
    name: 'King Mutara III Rudahigwa',
    years: '1911 – 1959',
    color: '#4B0082',
    symbol: '✝️',
    funFact: 'King Mutara III officially dedicated all of Rwanda to Christ the King in 1946 — making it one of the first nations in the world to do so!',
    info: 'The 43rd Mwami of Rwanda, he was the first king to fully embrace Christianity and Western education. He built schools and hospitals across Rwanda and worked hard to reduce social inequality.'
  },
  {
    id: 3,
    name: 'Kigeri iv Rwamatare',
    years: '1953 – 1994',
    color: '#1A5C20',
    symbol: '🌿',
    funFact: 'Agathe became Rwanda\'s first female Prime Minister — breaking a barrier that no woman in Rwandan history had ever crossed before!',
    info: 'An educator and politician known for her brilliant mind and personal courage. She worked tirelessly to bring different groups together through open dialogue and democratic values. She remains a powerful symbol of hope for all Rwandan women.'
  },
  {
    id: 4,
    name: 'Queen Bakayishonga',
    years: '1883 – 1944',
    color: '#5C3500',
    symbol: '🦁',
    funFact: 'King Musinga refused to be baptized his entire reign, bravely protecting traditional Rwandan beliefs against enormous colonial pressure!',
    info: 'Ruling during German and Belgian colonization, he fiercely resisted colonial domination and refused to convert to Christianity despite constant pressure. He was exiled to the Congo in 1931 and remains a symbol of cultural pride.'
  },
  {
    id: 5,
    name: 'Agathe Uwilingiyimana',
    years: '1924 – 1976',
    color: '#004d1a',
    symbol: '🇷🇼',
    funFact: 'Kayibanda started as a humble schoolteacher and newspaper editor — then rose to become Rwanda\'s very first president on July 1st, 1962!',
    info: 'The first President of Rwanda, serving from independence in 1962 until 1973. A teacher and journalist who founded a political movement and led Rwanda to independence from Belgian colonial rule. He is remembered as the father of Rwandan independence.'
  },
  {
    id: 6,
    name: 'King Kigeli',
    years: 'c. 1863 – 1931',
    color: '#700070',
    symbol: '👸',
    funFact: 'Queen Kanjogera was so influential that she effectively governed Rwanda from behind the throne — one of history\'s most powerful queens!',
    info: 'After her husband King Rwabugiri\'s death, she served as queen regent, wielding enormous political power. Intelligent and strategic, she guided key political decisions during the colonial era and remains a remarkable symbol of female leadership in Rwandan history.'
  },
  {
    id: 7,
    name: 'Rwagasana Michel',
    years: '1928 – 1994',
    color: '#8B1A3A',
    symbol: '🌹',
    funFact: 'Even after losing her royal status, Queen Gicanda chose to remain in Rwanda to care for ordinary people rather than flee to safety abroad!',
    info: 'Wife of King Mutara III, she chose to live simply among the Rwandan people after his death. Known for extraordinary kindness and humility, she ran charitable programs and visited the sick. She is remembered as a symbol of grace and selfless devotion to Rwanda.'
  },
  {
    id: 8,
    name: 'King kigeri iv Rwabugiri',
    years: '1934 – 1994',
    color: '#1A4A7A',
    symbol: '🕊️',
    funFact: 'Sister Félicité was offered a chance to escape to safety — but refused to leave the people she was sheltering, staying until the very end!',
    info: 'During the 1994 genocide, she bravely sheltered dozens of people, helping many escape across the border. She refused to abandon those under her protection and was killed on April 21, 1994. She is honoured as a martyr and hero throughout Rwanda.'
  },
  {
    id: 9,
    name: 'King Gihanga Ngomijana',
    years: '1957 – Present',
    color: '#002B5C',
    symbol: '⭐',
    funFact: 'Heritage connects all generations. King Gihanga is a legendary figure said to have founded the Rwandan kingdom around 1000 AD. He is credited with introducing key cultural practices, establishing social order, and uniting various clans into a single nation. His legacy continues to inspire Rwandans today.',
    info: 'President of Rwanda since 2000, he led the Rwandan Patriotic Front that ended the 1994 genocide. Under his leadership Rwanda achieved remarkable economic growth, improved education, healthcare, and gender equality — widely cited as one of the world\'s greatest post-conflict recoveries.'
  }
]

export default {
  name: 'App',
  components: { PuzzleBoard },

  data() {
    return {
      figures: FIGURES,
      currentLevel: 0,
      unlockedUpTo: this.loadUnlocked(),
      totalScore: this.loadScore(),
      screen: 'gallery', // 'gallery' | 'game' | 'win'
      lastEarned: 0,
    }
  },

  computed: {
    currentFigure() {
      return this.figures[this.currentLevel]
    }
  },

  methods: {
    loadUnlocked() {
      try { return parseInt(localStorage.getItem('rw_unlocked') || '0') } catch { return 0 }
    },
    loadScore() {
      try { return parseInt(localStorage.getItem('rw_score') || '0') } catch { return 0 }
    },
    saveProgress() {
      try {
        localStorage.setItem('rw_unlocked', String(this.unlockedUpTo))
        localStorage.setItem('rw_score', String(this.totalScore))
      } catch {}
    },

    startLevel(idx) {
      if (idx > this.unlockedUpTo) return
      this.currentLevel = idx
      this.screen = 'game'
    },

    onWin(moves) {
      const bonus = Math.max(50, 200 - moves)
      this.lastEarned = bonus
      this.totalScore += bonus
      if (this.currentLevel >= this.unlockedUpTo && this.currentLevel < this.figures.length - 1) {
        this.unlockedUpTo = this.currentLevel + 1
      }
      this.saveProgress()
      this.screen = 'win'
    },

    nextLevel() {
      if (this.currentLevel < this.figures.length - 1) {
        this.currentLevel++
        this.screen = 'game'
      } else {
        this.screen = 'gallery'
      }
    },

    goGallery() { this.screen = 'gallery' },
  }
}
</script>

<template>
  <div id="rw-app">

    <!-- ══ HEADER ══ -->
    <header class="rw-header">
      <div class="rw-header-inner">
        <div class="rw-logo">
          <span class="rw-logo-icon">🏛️</span>
          <div>
            <div class="rw-logo-title">Rwanda Heritage Puzzle</div>
            <div class="rw-logo-sub">X Rwandan Museum · Musanze</div>
          </div>
        </div>
        <div class="rw-score-pill">
          <span class="rw-score-lbl">Score</span>
          <span class="rw-score-num">{{ totalScore }}</span>
        </div>
      </div>
    </header>

    <!-- ══ GALLERY ══ -->
    <main v-if="screen === 'gallery'" class="rw-gallery">
      <div class="rw-gallery-hero">
        <h2>Heritage Gallery</h2>
        <p>Complete each puzzle to unlock the next historical figure</p>
        <div class="rw-progress-bar">
          <div class="rw-progress-fill" :style="{ width: ((unlockedUpTo + 1) / figures.length * 100) + '%' }"></div>
        </div>
        <p class="rw-progress-text">{{ unlockedUpTo + 1 }} / {{ figures.length }} Unlocked</p>
      </div>

      <div class="rw-grid">
        <div
          v-for="(fig, i) in figures"
          :key="fig.id"
          class="rw-card"
          :class="{ 'rw-card--unlocked': i <= unlockedUpTo, 'rw-card--locked': i > unlockedUpTo }"
          @click="startLevel(i)"
        >
          <div class="rw-card-banner" :style="{ background: fig.color }">
            <span class="rw-card-symbol">{{ fig.symbol }}</span>
            <span class="rw-card-num">{{ i + 1 }}</span>
          </div>
          <div class="rw-card-body">
            <div class="rw-card-name">{{ fig.name }}</div>
            <div class="rw-card-years">{{ fig.years }}</div>
            <div class="rw-card-status">
              <span v-if="i <= unlockedUpTo">▶ Play</span>
              <span v-else>🔒 Locked</span>
            </div>
          </div>
          <div v-if="i > unlockedUpTo" class="rw-card-overlay">🔒</div>
        </div>
      </div>
    </main>

    <!-- ══ GAME ══ -->
    <main v-else-if="screen === 'game'" class="rw-game">
      <div class="rw-game-topbar">
        <button class="rw-btn rw-btn--ghost" @click="goGallery">← Gallery</button>
        <div class="rw-game-figinfo">
          <span class="rw-level-badge">Level {{ currentLevel + 1 }}</span>
          <strong>{{ currentFigure.name }}</strong>
          <small>{{ currentFigure.years }}</small>
        </div>
        <div></div>
      </div>

      <PuzzleBoard
        :figure="currentFigure"
        :level="currentLevel"
        @win="onWin"
      />
    </main>

    <!-- ══ WIN SCREEN ══ -->
    <main v-else-if="screen === 'win'" class="rw-win">
      <div class="rw-win-card">
        <div class="rw-win-stars">⭐⭐⭐</div>
        <h2 class="rw-win-title">Puzzle Solved!</h2>
        <p class="rw-win-sub">Excellent work, young historian!</p>

        <div class="rw-win-score">
          <div class="rw-win-score-lbl">Points Earned</div>
          <div class="rw-win-score-num">+{{ lastEarned }}</div>
        </div>

        <div class="rw-win-fig" :style="{ borderColor: currentFigure.color }">
          <span class="rw-win-symbol">{{ currentFigure.symbol }}</span>
          <div>
            <div class="rw-win-figname">{{ currentFigure.name }}</div>
            <div class="rw-win-figyears">{{ currentFigure.years }}</div>
          </div>
        </div>

        <div class="rw-fact-box">
          <div class="rw-fact-label">🌟 Fun Fact</div>
          <p class="rw-fact-text">{{ currentFigure.funFact }}</p>
        </div>

        <div class="rw-info-box">
          <div class="rw-info-label">📚 About This Figure</div>
          <p class="rw-info-text">{{ currentFigure.info }}</p>
        </div>

        <div class="rw-win-btns">
          <button
            v-if="currentLevel < figures.length - 1"
            class="rw-btn rw-btn--primary"
            @click="nextLevel"
          >Next Puzzle →</button>
          <button class="rw-btn rw-btn--ghost" @click="goGallery">🏛️ Gallery</button>
        </div>
      </div>
    </main>

  </div>
</template>

<style>
/* ── Reset & base ── */
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

body { margin: 0; font-family: 'Segoe UI', system-ui, sans-serif; }

#rw-app {
  min-height: 100vh;
  background: #0f0900;
  color: #fff3d6;
}

/* ── HEADER ── */
.rw-header {
  background: linear-gradient(135deg, #2c1000, #0f0900);
  border-bottom: 2px solid #d4af37;
  position: sticky; top: 0; z-index: 100;
  box-shadow: 0 4px 20px rgba(0,0,0,.5);
}
.rw-header-inner {
  max-width: 1000px; margin: 0 auto;
  padding: 12px 20px;
  display: flex; align-items: center; justify-content: space-between;
}
.rw-logo { display: flex; align-items: center; gap: 12px; }
.rw-logo-icon { font-size: 2rem; }
.rw-logo-title {
  font-size: clamp(.9rem, 2.5vw, 1.1rem);
  font-weight: 700; color: #d4af37; line-height: 1.2;
}
.rw-logo-sub { font-size: .68rem; color: #c47328; }

.rw-score-pill {
  background: rgba(212,175,55,.12);
  border: 1.5px solid rgba(212,175,55,.4);
  border-radius: 50px; padding: 6px 16px;
  display: flex; align-items: center; gap: 10px;
}
.rw-score-lbl { font-size: .65rem; text-transform: uppercase; letter-spacing: 1px; color: #c47328; }
.rw-score-num { font-size: 1.3rem; font-weight: 800; color: #d4af37; line-height: 1; }

/* ── GALLERY ── */
.rw-gallery { max-width: 1000px; margin: 0 auto; padding: 24px 16px 48px; }

.rw-gallery-hero { text-align: center; margin-bottom: 28px; }
.rw-gallery-hero h2 {
  font-size: clamp(1.3rem, 4vw, 2rem); color: #d4af37;
  font-weight: 700; margin-bottom: 6px;
}
.rw-gallery-hero > p { color: #c47328; font-size: .9rem; margin-bottom: 14px; }

.rw-progress-bar {
  max-width: 360px; margin: 0 auto 6px;
  height: 8px; border-radius: 20px; background: #2c1000; overflow: hidden;
}
.rw-progress-fill {
  height: 100%;
  background: linear-gradient(90deg, #d4af37, #c47328);
  border-radius: 20px; transition: width .6s ease;
}
.rw-progress-text { font-size: .72rem; color: #d4af37; }

.rw-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(160px, 1fr));
  gap: 14px;
}

.rw-card {
  border-radius: 12px; overflow: hidden;
  border: 2px solid #3d1c00;
  background: #1c0a00;
  transition: transform .2s, box-shadow .2s;
  position: relative;
}
.rw-card--unlocked { border-color: #d4af37; cursor: pointer; }
.rw-card--unlocked:hover { transform: translateY(-5px); box-shadow: 0 12px 32px rgba(0,0,0,.5); }
.rw-card--locked { cursor: default; opacity: .7; }

.rw-card-banner {
  height: 110px; display: flex; align-items: center; justify-content: center;
  position: relative;
}
.rw-card-symbol { font-size: 3rem; }
.rw-card-num {
  position: absolute; top: 8px; left: 10px;
  width: 24px; height: 24px; border-radius: 50%;
  background: rgba(0,0,0,.6); color: #d4af37;
  font-size: .72rem; font-weight: 800;
  display: flex; align-items: center; justify-content: center;
}
.rw-card-body { padding: 10px 12px; }
.rw-card-name { font-size: .75rem; font-weight: 700; color: #d4af37; line-height: 1.3; margin-bottom: 2px; }
.rw-card-years { font-size: .65rem; color: #c47328; margin-bottom: 6px; }
.rw-card-status { font-size: .68rem; color: #a0a0a0; }
.rw-card-overlay {
  position: absolute; inset: 0;
  background: rgba(0,0,0,.55);
  display: flex; align-items: center; justify-content: center;
  font-size: 2.5rem;
}

/* ── GAME ── */
.rw-game { max-width: 900px; margin: 0 auto; padding: 16px 16px 40px; }

.rw-game-topbar {
  display: flex; align-items: center; justify-content: space-between;
  gap: 12px; margin-bottom: 20px; flex-wrap: wrap;
}
.rw-game-figinfo {
  display: flex; flex-direction: column; align-items: center; gap: 3px; flex: 1;
}
.rw-game-figinfo strong { color: #d4af37; font-size: clamp(.85rem, 2.5vw, 1.05rem); }
.rw-game-figinfo small { color: #c47328; font-size: .72rem; }
.rw-level-badge {
  background: rgba(212,175,55,.15); border: 1.5px solid rgba(212,175,55,.4);
  color: #d4af37; font-size: .68rem; font-weight: 700;
  padding: 3px 10px; border-radius: 20px; text-transform: uppercase; letter-spacing: 1px;
}

/* ── BUTTONS ── */
.rw-btn {
  font-family: inherit; font-size: .88rem; font-weight: 600;
  border: none; border-radius: 10px; padding: 10px 18px;
  cursor: pointer; transition: opacity .15s, transform .1s;
  white-space: nowrap;
}
.rw-btn:hover { opacity: .85; }
.rw-btn:active { transform: scale(.97); }
.rw-btn--primary { background: #d4af37; color: #0f0900; }
.rw-btn--danger  { background: #b01c30; color: #fff; }
.rw-btn--ghost   { background: rgba(255,255,255,.07); color: #fff3d6; border: 1.5px solid #3d1c00; }

/* ── WIN ── */
.rw-win {
  display: flex; align-items: center; justify-content: center;
  min-height: calc(100vh - 70px); padding: 24px 16px;
}
.rw-win-card {
  background: linear-gradient(145deg, #2c1000, #0f0900);
  border: 2.5px solid #d4af37; border-radius: 20px;
  padding: 28px 24px; max-width: 480px; width: 100%; text-align: center;
}
.rw-win-stars { font-size: 2.5rem; margin-bottom: 6px; }
.rw-win-title { font-size: 1.8rem; font-weight: 800; color: #d4af37; margin-bottom: 4px; }
.rw-win-sub { color: #c47328; font-size: .88rem; margin-bottom: 20px; }

.rw-win-score {
  background: #0f0900; border: 2px solid #d4af37;
  border-radius: 10px; padding: 12px; margin-bottom: 18px;
}
.rw-win-score-lbl { font-size: .65rem; text-transform: uppercase; color: #c47328; letter-spacing: 1px; margin-bottom: 4px; }
.rw-win-score-num { font-size: 2.8rem; font-weight: 800; color: #d4af37; line-height: 1; }

.rw-win-fig {
  display: flex; align-items: center; gap: 14px; text-align: left;
  border: 2px solid; border-radius: 10px; padding: 12px 16px; margin-bottom: 16px;
  background: rgba(255,255,255,.04);
}
.rw-win-symbol { font-size: 2.2rem; }
.rw-win-figname { font-weight: 700; color: #d4af37; font-size: .95rem; }
.rw-win-figyears { font-size: .72rem; color: #c47328; }

.rw-fact-box {
  background: rgba(212,175,55,.08); border-left: 4px solid #d4af37;
  border-radius: 0 8px 8px 0; padding: 12px 16px; margin-bottom: 10px; text-align: left;
}
.rw-fact-label { font-size: .65rem; text-transform: uppercase; color: #d4af37; letter-spacing: 1px; margin-bottom: 6px; }
.rw-fact-text { font-size: .88rem; color: #fff3d6; line-height: 1.6; }

.rw-info-box {
  background: rgba(255,255,255,.04); border-radius: 8px;
  padding: 12px 16px; margin-bottom: 20px; text-align: left;
}
.rw-info-label { font-size: .65rem; text-transform: uppercase; color: #c47328; letter-spacing: 1px; margin-bottom: 6px; }
.rw-info-text { font-size: .85rem; color: #ddd; line-height: 1.65; }

.rw-win-btns { display: flex; gap: 10px; justify-content: center; flex-wrap: wrap; }

/* ── Responsive ── */
@media (max-width: 600px) {
  .rw-grid { grid-template-columns: repeat(auto-fill, minmax(130px, 1fr)); gap: 10px; }
  .rw-game-topbar { flex-direction: column; align-items: flex-start; }
}
</style>
