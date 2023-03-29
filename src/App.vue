<template>
  <div>
    <div class="pole">
      <div style="margin: auto">
        <table>
          <tr
            v-for="trCell in trCellALl"
            :key="trCell"
            :id="trCell + trCell * 15 - 15"
          >
            <td
              class="cell"
              v-for="tdcell in tdCellAll"
              :key="tdcell + trCell * 15 - 15"
              :id="'id' + (tdcell + trCell * 15 - 15)"
              :class="[
                ...gameover(),
                { redClass: tdcell + trCell * 15 - 15 == food },
                { greyClass: tdcell + trCell * 15 - 15 == localStart },
                ...aaaah(tdcell, trCell),
              ]"
            ></td>
          </tr>
        </table>
      </div>
      <input
        v-if="!activate"
        ref="input"
        @keyup.w="watchering"
        @keyup.d="watchering"
        @keyup.s="watchering"
        @keyup.a="watchering"
      />
      <div v-if="activate">
        Похоже, что ты проиграл ~>
        <button @click="methodRefreshPage">Перезапустить игру</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      activate: false,
      zmeey: 0,
      lengthZmeey: [
        101,
        /* 1, 2, 3, 4, 5, 6, 7, 8, 9, 12, 13, 15, 16, 55, 44, 33, 55, 66, 34 */
      ],
      arrMove: [],
      trCellALl: 15,
      tdCellAll: 15,
      isActiveCell: false,
      localStart: 100,
      toMove: "w",
      food: 0,
      classZmey: [],
      i: 0,
      j: 0,
    };
  },

  methods: {
    focusInput() {
      this.$refs.input.focus();
    },

    methodRefreshPage() {
      location.reload();
    },
    aaaah(tdcell, trcell) {
      return this.lengthZmeey.map((cell) => ({
        greyClass: tdcell + trcell * 15 - 15 == cell,
      }));
    },
    gameover() {
      if (this.classZmey.length > 3) {
        return this.classZmey.map((elem, i) => ({
          yellowClass:
            this.classZmey.length && this.classZmey.indexOf(elem) !== i,
        }));
      } else {
        return [];
      }
    },

    movingNow() {
      let interv = setInterval(() => {
        this.aaaah();

        if (this.localStart == this.food) {
          this.randomFood();
          this.arrMove.push(this.food);
          console.log(this.arrMove.length);
        }

        if (this.arrMove.length == 0) {
          this.lengthZmeey.push(this.localStart);
          this.lengthZmeey.shift();
          console.log(this.lengthZmeey);
        }

        if (this.arrMove.length !== 0) {
          if (this.lengthZmeey.length == this.arrMove.length) {
            this.lengthZmeey.push(this.localStart);
          }
          if (this.lengthZmeey.length == this.arrMove.length + 1) {
            this.lengthZmeey.push(this.localStart);
            this.lengthZmeey.shift();
          }

          console.log(this.lengthZmeey);
        }

        if (this.toMove == "w") {
          if (this.localStart < 16) {
            this.localStart = this.localStart + 210;
          } else {
            this.localStart = this.localStart - 15;
          }
          if (this.toMove !== "w") {
            this.watchering();
          }
        } else if (this.toMove == "d") {
          if (this.localStart == 15) {
            this.localStart = 1;
          } else if (
            this.localStart == 30 ||
            this.localStart == 45 ||
            this.localStart == 60 ||
            this.localStart == 75 ||
            this.localStart == 90 ||
            this.localStart == 105 ||
            this.localStart == 120 ||
            this.localStart == 135 ||
            this.localStart == 150 ||
            this.localStart == 165 ||
            this.localStart == 180 ||
            this.localStart == 195 ||
            this.localStart == 210 ||
            this.localStart == 225
          ) {
            this.localStart = this.localStart - 14;
          } else {
            this.localStart = this.localStart + 1;
          }

          if (this.toMove !== "d") {
            this.watchering();
          }
        } else if (this.toMove == "s") {
          if (this.localStart > 210) {
            this.localStart = this.localStart - 210;
          } else {
            this.localStart = this.localStart + 15;
          }

          if (this.toMove !== "s") {
            this.watchering();
          }
        } else if (this.toMove == "a") {
          if (this.localStart == 1) {
            this.localStart = 15;
          } else if (
            this.localStart == 16 ||
            this.localStart == 31 ||
            this.localStart == 46 ||
            this.localStart == 61 ||
            this.localStart == 76 ||
            this.localStart == 91 ||
            this.localStart == 106 ||
            this.localStart == 121 ||
            this.localStart == 136 ||
            this.localStart == 151 ||
            this.localStart == 166 ||
            this.localStart == 181 ||
            this.localStart == 196 ||
            this.localStart == 211
          ) {
            this.localStart = this.localStart + 14;
          } else {
            this.localStart = this.localStart - 1;
          }

          if (this.toMove !== "a") {
            this.watchering();
          }
        }

        this.classZmey = this.lengthZmeey.map((elem) => elem);

        this.classZmey.map((elem, i) => {
          if (
            this.classZmey.length > 3 &&
            this.classZmey[i] == this.classZmey[i + 1]
          ) {
            this.classZmey.splice(i, 1);
          }
          if (this.classZmey.length > 3 && this.classZmey.indexOf(elem) !== i) {
            this.activate = true;
            this.gameover();
            clearInterval(interv);
          }
        });
      }, 300);
    },
    watchering(e) {
      this.toMove = e.key;
    },
    randomFood() {
      this.food = Math.ceil(1 - 0.5 + Math.random() * (225 - 1 + 1));
      this.lengthZmeey.forEach((el) => {
        if (el == this.food) {
          this.food = Math.ceil(1 - 0.5 + Math.random() * (225 - 1 + 1));
          console.log("!!!!!!!!!!!___________!!!!!!!!!!!!");
          this.randomFood();
        }
      });
      console.log(this.food);
    },
  },
  mounted() {
    this.movingNow();
    this.randomFood();
    this.focusInput();
  },
};
</script>

<style>
button {
  background-color: aqua;
  padding: 7px;
}
.yellowClass {
  background-color: yellow !important;
}
.redClass {
  background-color: red !important;
}
.greyClass {
  background-color: grey;
}
* {
  box-sizing: border-box;
}
.pole {
  margin: 0;
  padding: 0;
  margin-top: 100px;
  margin-left: 500px;
}
.cell {
  border: 1px solid black;
  width: 30px;
  height: 30px;
}
</style>
