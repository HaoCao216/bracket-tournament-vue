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
        <div v-bind:class="['bracket-chart single-chart', typeElemintion === 'double' ? 'double-elemintion' : '']"
          v-bind:style="[typeElemintion === 'single' ? {} : {marginLeft: 242*(Math.log2(this.teamNumber) - 2) + 'px'}]"
        >
          <div
            v-bind:class="`round ` + `round-` + index"
            v-for="(item, index) in roundNumber"
            :key="index"
          >
            <match v-for="(match, key) in getMatchNumberUpperRound(index)" :key="key"/>
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
        <match />
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
      teamNumber: 8,
      typeElemintion: 'single',
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
      ]
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
    }
  },
  created() {
    //Tinh so round o nhanh thang

    this.roundNumber = Math.log2(this.teamNumber);

    //Tinh so round o nhanh thua 

    this.roundNumberLower = 2 * (Math.log2(this.teamNumber) - 1);
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