<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <div>
      <el-select v-model="teamNumber" placeholder="Select">
        <el-option
          v-for="item in teamMax"
          :key="item.value"
          :label="item.label"
          :value="item.value"
        ></el-option>
      </el-select>
    </div>
    <div>
      <el-radio v-model="typeElemintion" label="single">Single Elemintions</el-radio>
      <el-radio v-model="typeElemintion" label="double">Double Elemintions</el-radio>
    </div>
    <div class="bracket">
      <div class="main-bracket">
        <div class="type-bracket">UPPER BRACKET</div>
        <div
          v-bind:class="['bracket-chart single-chart', typeElemintion === 'double' ? 'double-elemintion' : '']"
          v-bind:style="[typeElemintion === 'single' ? {} : {marginLeft: 242*(Math.log2(this.teamNumber) - 2) + 'px'}]"
        >
          <div
            v-bind:class="`round ` + `round-` + index"
            v-for="(item, index) in dataFormatUpper"
            :key="index"
          >
            <match
              :drag="drag"
              :drop="drop"
              :allowDrop="allowDrop"
              v-for="(match, key) in item"
              :matchData="match"
              :selectWinner="selectWinner"
              :key="key"
              :slotMatch="key"
            />
          </div>
        </div>
        <div v-if="typeElemintion === 'double'">
          <div class="type-bracket">LOWER BRACKET</div>
          <div class="bracket-chart double-chart">
            <div
              v-bind:class="`round ` + `round-` + index"
              v-for="(item, index) in dataFormatLower"
              :key="index"
            >
              <match
                v-for="(match, key) in item"
                :matchData="match"
                :key="key"
                :selectWinner="selectWinner"
                :slotMatch="key"
              />
            </div>
          </div>
        </div>
      </div>
      <div v-if="typeElemintion === 'double'" class="final-block">
        <match :selectWinner="selectWinner" :matchData="dataFormatFinal"/>
      </div>
    </div>
  </div>
</template>

<script>
import Match from "./components/Match.vue";
import draggable from "vuedraggable";
import { EightTeam, SixteenTeam, ThirtyThreeTeam } from "./seedData.js";

export default {
  name: "app",
  components: {
    Match,
    draggable
  },
  data() {
    return {
      matchData: EightTeam,
      teamNumber: 8,
      typeElemintion: "single",
      roundNumber: 0,
      roundNumberLower: 0,
      count: 0,
      teamMax: [
        {
          value: "8",
          label: "8 Teams"
        },
        {
          value: "16",
          label: "16 Teams"
        },
        {
          value: "32",
          label: "32 Teams"
        }
      ],
      dataFormatUpper: [],
      dataFormatLower: [],
      dataFormatFinal: [],
      teamSelectSwap: '',
    };
  },
  methods: {
    allowDrop(ev) {
      ev.preventDefault();
    },
    drag(ev, type, slot, check_away, check_home) {
      if(check_away || check_home) return;
      this.teamSelectSwap = this.dataFormatUpper[0][slot][type];
    },
    drop(ev, type, slot, check_away, check_home) {
      if(check_away || check_home) return;
      this.dataFormatUpper[0].find(item => {
        if(item['home_team'] === this.teamSelectSwap) {
          item['home_team'] = this.dataFormatUpper[0][slot][type];
        } else if(item['away_team'] === this.teamSelectSwap) {
          item['away_team'] = this.dataFormatUpper[0][slot][type];
        }
      })
      this.dataFormatUpper[0][slot][type] = this.teamSelectSwap;
    },
    //Ham tinh so tran tai round thu INDEX o nhanh thang

    getMatchNumberUpperRound(index) {
      return this.teamNumber / Math.pow(2, index + 1);
    },

    //Ham tinh so tran tai round thu INDEX o nhanh thua supported by VYQUOCVU

    getMatchNumberLowerRound(index) {
      return Math.pow(2, Math.floor((this.roundNumberLower - (index + 1)) / 2));
    },

    selectWinner(teamWinner, teamLoser, data, slot) {
      if(data.bracket_type === 'Upper Bracket' && data.round === 1) {
        if(this.dataFormatLower[data.round - 1].length === this.dataFormatUpper[data.round - 1].length/2) {
          if(slot%2 === 0) {
            if(this.dataFormatLower[data.round - 1][Math.floor( slot/2 )].home_team === null) {
              this.dataFormatLower[data.round - 1][Math.floor( slot/2 )].home_team = teamLoser;
            } else {
              this.dataFormatLower[data.round - 1][Math.floor( slot/2 )].away_team = teamLoser;
            }
          } else {
            this.dataFormatLower[data.round - 1][Math.floor( slot/2 )].away_team = teamLoser;
          }
        }
        if(this.dataFormatLower[data.round - 1].length === this.dataFormatUpper[data.round - 1].length) {
          if(slot%2 === 0) {
            if(this.dataFormatLower[data.round - 1][Math.floor( slot )].home_team === null) {
              this.dataFormatLower[data.round - 1][Math.floor( slot )].home_team = teamLoser;
            } else {
              this.dataFormatLower[data.round - 1][Math.floor( slot )].away_team = teamLoser;
            }
          } else {
            this.dataFormatLower[data.round - 1][Math.floor( slot )].away_team = teamLoser;
          }
        }
      } else if(data.bracket_type === 'Upper Bracket') {
        if(this.dataFormatLower[data.round*2 - 3].length === this.dataFormatUpper[data.round - 1].length/2) {
          if(slot%2 === 0) {
            if(this.dataFormatLower[data.round*2 - 3][Math.floor( slot/2 )].home_team === null) {
              this.dataFormatLower[data.round*2 - 3][Math.floor( slot/2 )].home_team = teamLoser;
            } else {
              this.dataFormatLower[data.round*2 - 3][Math.floor( slot/2 )].away_team = teamLoser;
            }
          } else {
            this.dataFormatLower[data.round*2 - 3][Math.floor( slot/2 )].away_team = teamLoser;
          }
        }
        if(this.dataFormatLower[data.round*2 - 3].length === this.dataFormatUpper[data.round - 1].length) {
          if(slot%2 === 0) {
            if(this.dataFormatLower[data.round*2 - 3][Math.floor( slot )].home_team === null) {
              this.dataFormatLower[data.round*2 - 3][Math.floor( slot )].home_team = teamLoser;
            } else {
              this.dataFormatLower[data.round*2 - 3][Math.floor( slot )].away_team = teamLoser;
            }
          } else {
            this.dataFormatLower[data.round*2 - 3][Math.floor( slot )].away_team = teamLoser;
          }
        }
      }
      this.matchData.find((item, index) => {
        if (data.parent === item.uuid) {
          if (data.home_team === this.matchData[index].home_team) {
            return (this.matchData[index].home_team = teamWinner);
          }
          if (data.home_team === this.matchData[index].away_team) {
            return (this.matchData[index].away_team = teamWinner);
          }
          if (data.away_team === this.matchData[index].home_team) {
            return (this.matchData[index].home_team = teamWinner);
          }
          if (data.away_team === this.matchData[index].away_team) {
            return (this.matchData[index].away_team = teamWinner);
          }
          if (this.matchData[index].home_team === null) {
            return (this.matchData[index].home_team = teamWinner);
          } else {
            return (this.matchData[index].away_team = teamWinner);
          }
        }
      });
    },

    renderData() {
      this.dataFormatFinal = [],
      this.dataFormatUpper = [],
      this.dataFormatLower = [],
      this.dataFormatFinal = this.matchData.find(item => {
        return item.parent === null;
      });

      for (let i = 1; i <= this.roundNumber; i++) {
        this.dataFormatUpper.push(
          this.matchData.filter(
            item => item.round === i && item.bracket_type === "Upper Bracket"
          )
        );
      }

      for (let i = 1; i <= this.roundNumberLower; i++) {
        this.dataFormatLower.push(
          this.matchData.filter(
            item => item.round === i && item.bracket_type === "Lower Bracket"
          )
        );
      }
    }
  },
  created() {
    //Tinh so round o nhanh thang

    this.roundNumber = Math.log2(this.teamNumber);

    //Tinh so round o nhanh thua

    this.roundNumberLower = 2 * (Math.log2(this.teamNumber) - 1);

    this.renderData();
  },
  watch: {
    teamNumber: function(value) {
      this.roundNumber = Math.log2(this.teamNumber);
      this.roundNumberLower = 2 * (Math.log2(this.teamNumber) - 1);
      switch (value) {
        case "8":
          this.matchData = EightTeam;
          break;
        case "16":
          this.matchData = SixteenTeam;
          break;
        case "32":
          this.matchData = ThirtyThreeTeam;
          break;
      }
      this.renderData()
    }
  }
};
</script>

<style lang="scss">
.final-block {
  .match {
    top: 56%;
    &::after {
      content: "";
      position: absolute;
      width: 70px;
      z-index: 2;
      height: 2px;
      background-color: gray;
      left: 0px;
    }
  }
}
.double-chart {
  .round {
    &:nth-child(odd) {
      .match {
        &::after {
          content: "";
          position: absolute;
          width: 50%;
          z-index: 2;
          height: 2px;
          background-color: gray;
          right: -50%;
          transform: translateX(-50%);
        }
      }
    }
    &:nth-child(odd):not(:first-child) {
      .match {
        &::before {
          content: "";
          position: absolute;
          width: 50%;
          z-index: 2;
          height: 2px;
          background-color: gray;
          left: 0px;
        }
      }
    }
    &:last-child {
      .match {
        &::after {
          content: "";
          position: absolute;
          width: 100%;
          z-index: 2;
          height: 2px;
          background-color: gray;
          left: 0px;
        }
        &::before {
          content: "";
          position: absolute;
          width: 2px;
          z-index: 2;
          height: 100%;
          background-color: gray;
          right: 0px;
          top: -50%;
        }
      }
    }
    &:nth-child(even):not(:last-child) {
      .match {
        &::after {
          content: "";
          position: absolute;
          width: 100%;
          z-index: 2;
          height: 2px;
          background-color: gray;
          left: 0px;
        }
        &:nth-child(odd) {
          &::before {
            content: "";
            position: absolute;
            width: 2px;
            z-index: 2;
            height: 50%;
            background-color: gray;
            right: 0px;
            bottom: 0px;
          }
        }
        &:nth-child(even) {
          &::before {
            content: "";
            position: absolute;
            width: 2px;
            z-index: 2;
            height: 50%;
            background-color: gray;
            right: 0px;
            top: 0px;
          }
        }
      }
    }
  }
}

.single-chart {
  .round {
    &:first-child {
      .match {
        .match-team {
          &:hover {
            cursor: move; /* fallback if grab cursor is unsupported */
            cursor: grab;
            cursor: -moz-grab;
            cursor: -webkit-grab;
          }
          &:active {
            cursor: grabbing;
            cursor: -moz-grabbing;
            cursor: -webkit-grabbing;
          }
        }
        &::after {
          content: "";
          position: absolute;
          width: 20%;
          z-index: 2;
          height: 2px;
          background-color: gray;
          right: 0px;
        }
        &:nth-child(odd) {
          &::before {
            content: "";
            position: absolute;
            width: 2px;
            z-index: 2;
            height: 50%;
            background-color: gray;
            right: 0px;
            bottom: 0px;
          }
        }
        &:nth-child(even) {
          &::before {
            content: "";
            position: absolute;
            width: 2px;
            z-index: 2;
            height: 50%;
            background-color: gray;
            right: 0px;
            top: 0px;
          }
        }
      }
    }
    &:last-child {
      .match {
        &::after {
          content: "";
          position: absolute;
          width: 50%;
          z-index: 2;
          height: 2px;
          background-color: gray;
          left: 0px;
        }
      }
    }
    &:not(:first-child):not(:last-child) {
      .match {
        &::after {
          content: "";
          position: absolute;
          width: 100%;
          z-index: 2;
          height: 2px;
          background-color: gray;
          left: 0px;
        }
        &:nth-child(odd) {
          &::before {
            content: "";
            position: absolute;
            width: 2px;
            z-index: 2;
            height: 50%;
            background-color: gray;
            right: 0px;
            bottom: 0px;
          }
        }
        &:nth-child(even) {
          &::before {
            content: "";
            position: absolute;
            width: 2px;
            z-index: 2;
            height: 50%;
            background-color: gray;
            right: 0px;
            top: 0px;
          }
        }
      }
    }
  }
}
.double-elemintion {
  .round {
    &:last-child {
      .match {
        &::after {
          content: "";
          position: absolute;
          width: 100%;
          z-index: 2;
          height: 2px;
          background-color: gray;
          left: 0px;
        }
        &::before {
          content: "";
          position: absolute;
          width: 2px;
          z-index: 2;
          height: 50%;
          background-color: gray;
          right: 0px;
          bottom: 0px;
        }
      }
    }
  }
}
</style>


<style lang="scss" scoped>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  .type-bracket {
    font-weight: bold;
    padding: 30px;
    text-align: left;
  }
  .bracket {
    position: relative;
    display: flex;
    .main-bracket {
    }
    .final-block {
    }
    .bracket-chart {
      display: flex;
      .round {
        display: grid;
      }
    }
  }
}
.ghost {
  opacity: 0.5;
  background: #c8ebfb;
}
</style>