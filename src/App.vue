<template>
  <div :class="styles.header">
    <div :class="styles.title">
      <b>Wordle</b>
    </div>
    <div :class="styles.switch_btn">
      <i class="fas fa-sun" v-if="theme === 'light'"></i>
      <i class="fas fa-moon" v-if="theme === 'dark'"></i>
      <Switch v-model="theme" />
    </div>
  </div>
  <div :class="styles.container">
    <WordTiles :rows="rows" :word="word" />
    <KeyBoard
      @onKeyBoard="addWordInTile"
      @onBackSpace="handleBackSpace"
      @onEnter="handleEnter"
    />
  </div>
  <Modal :isOpen="win" :toggle="reset">
    <div :class="styles.popup_msg">
      <span>Won the game!</span>
      <button @click="reset">Restart</button>
    </div>
  </Modal>
  <Modal :isOpen="lose" :toggle="reset">
    <div :class="styles.popup_msg">
      <span>Lost the game!</span>
      <span>The word is {{ word }}</span>
      <button @click="reset">Restart</button>
    </div>
  </Modal>
  <Alert :message="message" @onClose="message = ''" />
</template>

<script setup>
import { onMounted, reactive, toRefs, watch } from "vue";
import KeyBoard from "@/components/KeyBoard.vue";
import WordTiles from "@/components/WordTiles.vue";
import Modal from "@/components/Modal.vue";
import Alert from "@/components/Alert.vue";
import Switch from "./components/Switch.vue";
import words from "@/utils/words.json";

const wordList = new Set(words);

const state = reactive({
  rows: ["", "", "", "", "", ""],
  activeRow: 0,
  theme: null,
  word: null,
  lose: false,
  win: false,
  message: "",
  showAlert: false,
});

watch(
  () => state.theme,
  (newValue) => {
    setTheme(newValue);
  }
);

const setTheme = (theme) => {
  localStorage.setItem("theme", theme);
  let root = document.querySelector(":root");
  root.setAttribute("data-theme", theme);
};

onMounted(() => {
  window.addEventListener("keydown", handleKeyUp);
  const theme = localStorage.getItem("theme") || "dark";
  setTheme(theme);
  state.theme = theme;
  state.word = getRandomWord();
});

const getRandomWord = () => {
  return words[Math.floor(Math.random() * words.length)];
};

const handleKeyUp = ({ key }) => {
  const regex = /^[a-z]$/;

  if (key === "Enter") {
    handleEnter();
    return;
  }

  if (key === "Backspace") {
    handleBackSpace();
    return;
  }

  if (!regex.test(key)) return;

  addWordInTile(key);
};

const handleState = (element, index) => {
  let { word } = state;
  let letter = element.textContent.toLowerCase();

  if (word[index] === letter) {
    element.setAttribute("data-state", "correct");
  } else if (word.includes(letter)) {
    element.setAttribute("data-state", "present");
  } else {
    element.setAttribute("data-state", "absent");
  }
};

const handleBackSpace = () => {
  let { rows, activeRow } = state;

  state.rows = rows.map((row, index) => {
    if (activeRow === index) {
      return row.slice(0, -1);
    } else {
      return row;
    }
  });
};

const handleEnter = () => {
  let { rows, activeRow, word } = state;

  let typedWord = rows[activeRow];

  if (typedWord.length === 5) {
    let row = [...document.querySelectorAll(`[data-row='${activeRow}']`)];
    let keys = [...document.querySelectorAll("[data-key]")];

    if (wordList.has(typedWord)) {
      keys
        .filter(({ textContent }) =>
          typedWord.includes(textContent.toLowerCase())
        )
        .map((ele) => {
          let { textContent } = ele;
          handleState(ele, typedWord.indexOf(textContent.toLowerCase()));
        });

      row.forEach((ele, index) => {
        ele.setAttribute("style", `--animation-delay: ${index * 500}ms`);
        ele.classList.add("flip");

        ele.onanimationend = ({ animationName, target }) => {
          let {
            dataset: { id },
          } = target;

          if (animationName === "upDown") {
            target.classList.remove("upDown");
            if (id === "5") {
              state.win = true;
            }
          } else if (animationName === "flip") {
            target.classList.remove("flip");

            handleState(target, index);

            // Check win or not
            if (id === "5") {
              let typedWord = rows[activeRow].toLowerCase();

              if (word === typedWord) {
                row.forEach((tile, i) => {
                  tile.setAttribute("style", `--animation-delay: ${i * 250}ms`);
                  tile.classList.add("upDown");
                });
              } else {
                if (activeRow < 5) {
                  state.activeRow = activeRow + 1;
                } else {
                  state.lose = true;
                }
              }
            }
          }
        };
      });
    } else {
      state.message = "Not in word list";
      row.forEach((ele, index) => {
        ele.setAttribute("style", `--animation-delay: ${index * 250}ms`);
        ele.classList.add("shake");
        ele.onanimationend = ({ animationName, target }) => {
          if (animationName === "shake") {
            target.classList.remove("shake");
          }
        };
      });
    }
  } else {
    state.message = "Not enough letters";
  }
};

const reset = () => {
  let rows = [...document.querySelectorAll(`[data-row]`)];
  let keys = [...document.querySelectorAll("[data-key]")];

  rows.forEach((ele) => {
    ele.removeAttribute("data-state");
  });

  keys.forEach((ele) => {
    ele.removeAttribute("data-state");
  });

  state.rows = ["", "", "", "", "", ""];
  state.word = getRandomWord();
  state.activeRow = 0;
  state.win = false;
  state.lose = false;
  state.showAlert = false;
};

const addWordInTile = (letter) => {
  let { rows, activeRow } = state;

  if (rows[activeRow].length >= 5) return;

  state.rows = rows.map((row, index) => {
    if (activeRow === index) {
      return row.concat(letter);
    } else {
      return row;
    }
  });
};

let { rows, word, win, lose, theme, message } = toRefs(state);
</script>

<style lang="scss" module="styles">
.header {
  padding: 7px 0px;
  border-bottom: 1px solid var(--tile-border-color);
  display: flex;
  justify-content: center;
  position: relative;
  .title {
    color: var(--text-color);
    font-size: 32px;
  }
  i {
    font-size: 24px;
    color: var(--text-color);
  }
  .switch_btn {
    display: flex;
    gap: 10px;
    position: absolute;
    right: 10px;
    top: 50%;
    transform: translateY(-50%);
    color: var(--text-color);
  }
}

.container {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  gap: 20px;
  max-width: 600px;
  width: 100%;
  height: calc(100vh - 63px);
  margin: 0px auto;
  padding: 15px 0px;
  user-select: none;
}

.popup_msg {
  display: flex;
  flex-direction: column;
  gap: 15px;
  justify-content: center;
  align-items: center;
  padding: 20px 0px;
  font-size: 24px;
  button {
    background-color: #4185f4;
    color: white;
    padding: 10px 0px;
    width: 120px;
    font-size: 18px;
    border-radius: 4px;
    border: none;
    cursor: pointer;
  }
}
</style>
