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
            <match v-for="(match, key) in item" :matchData="match" :selectWinner="selectWinner" :key="key"/>
          </div>
        </div>
        <div v-if="typeElemintion === 'double'">
          <div class="type-bracket">LOWER BRACKET</div>
          <div class="bracket-chart double-chart">
            <div
              v-bind:class="`round ` + `round-` + index"
              v-for="(item, index) in roundNumberLower"
              :key="index"
            >
              <match v-for="(match, key) in getMatchNumberLowerRound(index)" :key="key"/>
            </div>
          </div>
        </div>
      </div>
      <div v-if="typeElemintion === 'double'" class="final-block">
        <match/>
      </div>
    </div>
  </div>
</template>

<script>
import Match from "./components/Match.vue";

export default {
  name: "app",
  components: {
    Match
  },
  data() {
    return {
      matchData: [
        {
          created: "2019-04-02T03:07:56.469950Z",
          modified: "2019-04-02T03:07:56.469991Z",
          uuid: "d7d5ae9b-d554-4dbf-b0c1-8260bbfa1db4",
          competition: "b0143b9a-fc01-4e12-87e5-3dc6e60dfcc7",
          home_team: null,
          away_team: null,
          home_player: null,
          away_player: null,
          match_date_time: null,
          home_declared_winner_uuid: null,
          away_declared_winner_uuid: null,
          organiser_declared_winner_uuid: null,
          round: 3,
          bracket_type: "Upper Bracket",
          parent: null,
          home_comment: null,
          away_comment: null
        },
        {
          created: "2019-04-02T03:07:56.514142Z",
          modified: "2019-04-02T03:07:56.514175Z",
          uuid: "5e0e9b65-bc7d-4c77-89d3-1428987ca112",
          competition: "b0143b9a-fc01-4e12-87e5-3dc6e60dfcc7",
          home_team: null,
          away_team: null,
          home_player: null,
          away_player: null,
          match_date_time: null,
          home_declared_winner_uuid: null,
          away_declared_winner_uuid: null,
          organiser_declared_winner_uuid: null,
          round: 2,
          bracket_type: "Upper Bracket",
          parent: "d7d5ae9b-d554-4dbf-b0c1-8260bbfa1db4",
          home_comment: null,
          away_comment: null
        },
        {
          created: "2019-04-02T03:07:56.608837Z",
          modified: "2019-04-02T03:07:56.608875Z",
          uuid: "20d47e84-4df8-41d8-ba4d-2b660b44902d",
          competition: "b0143b9a-fc01-4e12-87e5-3dc6e60dfcc7",
          home_team: "team 1",
          away_team: "team 2",
          home_player: null,
          away_player: null,
          match_date_time: "2019-03-18T21:22:00Z",
          home_declared_winner_uuid: null,
          away_declared_winner_uuid: null,
          organiser_declared_winner_uuid: null,
          round: 1,
          bracket_type: "Upper Bracket",
          parent: "5e0e9b65-bc7d-4c77-89d3-1428987ca112",
          home_comment: null,
          away_comment: null
        },
        {
          created: "2019-04-02T03:07:56.638034Z",
          modified: "2019-04-02T03:07:56.638072Z",
          uuid: "4aa7de4a-79d2-4f5f-8955-6211d556f48e",
          competition: "b0143b9a-fc01-4e12-87e5-3dc6e60dfcc7",
          home_team: "team 3",
          away_team: "team 4",
          home_player: null,
          away_player: null,
          match_date_time: "2019-03-26T20:13:00Z",
          home_declared_winner_uuid: null,
          away_declared_winner_uuid: null,
          organiser_declared_winner_uuid: null,
          round: 1,
          bracket_type: "Upper Bracket",
          parent: "5e0e9b65-bc7d-4c77-89d3-1428987ca112",
          home_comment: null,
          away_comment: null
        },
        {
          created: "2019-04-02T03:07:56.485367Z",
          modified: "2019-04-02T03:07:56.485398Z",
          uuid: "94a2cfa4-0a04-46a0-b8ac-990d72b557b2",
          competition: "b0143b9a-fc01-4e12-87e5-3dc6e60dfcc7",
          home_team: null,
          away_team: null,
          home_player: null,
          away_player: null,
          match_date_time: null,
          home_declared_winner_uuid: null,
          away_declared_winner_uuid: null,
          organiser_declared_winner_uuid: null,
          round: 2,
          bracket_type: "Upper Bracket",
          parent: "d7d5ae9b-d554-4dbf-b0c1-8260bbfa1db4",
          home_comment: null,
          away_comment: null
        },
        {
          created: "2019-04-02T03:07:56.548689Z",
          modified: "2019-04-02T03:07:56.548734Z",
          uuid: "67811e3a-9687-4fdf-ad9e-6eff7bdeacf6",
          competition: "b0143b9a-fc01-4e12-87e5-3dc6e60dfcc7",
          home_team: "team 5",
          away_team: "team 6",
          home_player: null,
          away_player: null,
          match_date_time: "2019-03-24T21:21:00Z",
          home_declared_winner_uuid: null,
          away_declared_winner_uuid: null,
          organiser_declared_winner_uuid: null,
          round: 1,
          bracket_type: "Upper Bracket",
          parent: "94a2cfa4-0a04-46a0-b8ac-990d72b557b2",
          home_comment: null,
          away_comment: null
        },
        {
          created: "2019-04-02T03:07:56.573022Z",
          modified: "2019-04-02T03:07:56.573062Z",
          uuid: "99746c31-79f4-4ce1-88ea-d39fb142faa7",
          competition: "b0143b9a-fc01-4e12-87e5-3dc6e60dfcc7",
          home_team: "team 7",
          away_team: "team 8",
          home_player: null,
          away_player: null,
          match_date_time: "2019-03-28T21:21:00Z",
          home_declared_winner_uuid: null,
          away_declared_winner_uuid: null,
          organiser_declared_winner_uuid: null,
          round: 1,
          bracket_type: "Upper Bracket",
          parent: "94a2cfa4-0a04-46a0-b8ac-990d72b557b2",
          home_comment: null,
          away_comment: null
        }
      ],
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
        },
        {
          value: "128",
          label: "128 Teams"
        }
      ],
      dataFormatUpper: []
    };
  },
  methods: {
    //Ham tinh so tran tai round thu INDEX o nhanh thang

    getMatchNumberUpperRound(index) {
      return this.teamNumber / Math.pow(2, index + 1);
    },

    //Ham tinh so tran tai round thu INDEX o nhanh thua supported by VYQUOCVU

    getMatchNumberLowerRound(index) {
      return Math.pow(2, Math.floor((this.roundNumberLower - (index + 1)) / 2));
    },

    selectWinner(teamWinner, data) {
      this.matchData.find((item, index) => {
        if(data.parent === item.uuid) {
          if(data.home_team === this.matchData[index].home_team) {
            return this.matchData[index].home_team = teamWinner;
          }
          if(data.home_team === this.matchData[index].away_team) {
            return this.matchData[index].away_team = teamWinner;
          }
          if(data.away_team === this.matchData[index].home_team) {
            return this.matchData[index].home_team = teamWinner;
          }
          if(data.away_team === this.matchData[index].away_team) {
            return this.matchData[index].away_team = teamWinner;
          }
          if(this.matchData[index].home_team === null) {
            this.matchData[index].home_team = teamWinner;
          } else {
            this.matchData[index].away_team = teamWinner;
          }
        }
      })
    }
  },
  created() {
    //Tinh so round o nhanh thang

    this.roundNumber = Math.log2(this.teamNumber);

    //Tinh so round o nhanh thua

    this.roundNumberLower = 2 * (Math.log2(this.teamNumber) - 1);

    //Get data upper round 1

    for (let i = 1; i <= this.roundNumber; i++) {
      this.dataFormatUpper.push(
        this.matchData.filter(item => item.round === i)
      );
    }
  },
  watch: {
    teamNumber: function() {
      this.roundNumber = Math.log2(this.teamNumber);
      this.roundNumberLower = 2 * (Math.log2(this.teamNumber) - 1);
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
</style>