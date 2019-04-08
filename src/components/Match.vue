<template>
  <div class="match">
    <div class="match-team">
      <div
        :draggable="matchData.round === 1 && matchData.bracket_type === 'Upper Bracket'"
        @drop="drop(matchData.home_team, 'home_team', slotMatch, checkedAway, checkedHome)"
        @dragover="allowDrop"
        :id="matchData.home_team"
        @dragstart="drag(matchData.home_team, 'home_team', slotMatch, checkedAway, checkedHome)"
        class="select-team-winner"
      >
        <div
          v-bind:class="['home-team article', checkedHome ? 'is-winner' : '']"
        >{{matchData.home_team || 'Up coming'}}</div>
        <el-checkbox v-model="checkedHome" :disabled="!matchData.home_team || value === ''"/>
      </div>
      <div
        :draggable="matchData.round === 1 && matchData.bracket_type === 'Upper Bracket'"
        @drop="drop(matchData.away_team, 'away_team', slotMatch, checkedAway, checkedHome)"
        @dragover="allowDrop"
        :id="matchData.away_team"
        @dragstart="drag(matchData.away_team, 'away_team', slotMatch, checkedAway, checkedHome)"
        class="select-team-winner away-group"
      >
        <div
          v-bind:class="['away-team article', checkedAway ? 'is-winner' : '']"
        >{{matchData.away_team || 'Up coming'}}</div>
        <el-checkbox v-model="checkedAway" :disabled="!matchData.away_team || value === ''"/>
      </div>
    </div>
    <div class="date-time">
      <span v-if="value === ''">Start Time</span>
      <span v-else>{{ value | moment("dd, MM YY, h:mm a") }}</span>
      <el-date-picker
        :disabled="!matchData.home_team || !matchData.away_team"
        v-model="value"
        type="datetime"
        placeholder="Select date and time"
        default-time="12:00:00"
      ></el-date-picker>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    allowDrop: Function,
    drag: Function,
    drop: Function,
    matchData: {
      type: Object,
      default: () => {}
    },
    slotMatch: Number,
    selectWinner: Function
  },
  data() {
    return {
      value: "",
      checkedHome: false,
      checkedAway: false,
    };
  },
  watch: {
    checkedHome: function(value) {
      if (value) {
        this.checkedAway = false;
        this.selectWinner(
          this.matchData.home_team,
          this.matchData.away_team,
          this.matchData,
          this.slotMatch
        );
      }
    },
    checkedAway: function(value) {
      if (value) {
        this.checkedHome = false;
        this.selectWinner(
          this.matchData.away_team,
          this.matchData.home_team,
          this.matchData,
          this.slotMatch
        );
      }
    }
  }
};
</script>

<style lang="scss">
.match {
  .el-checkbox {
    margin-right: 7px;
  }
}
.date-time {
  height: 57px;
  width: 100%;
  color: #fff;
  position: relative;
  .el-date-editor {
    opacity: 0;
    position: absolute;
    left: 0px;
  }
}
</style>


<style lang="scss" scoped>
.match {
  padding: 20px 7px 20px 25px;
  font-size: 12px;
  display: flex;
  align-items: center;
  position: relative;
  .is-winner {
    font-weight: bold;
    font-size: 12px;
    color: green;
  }
  .match-team {
    background-color: #e5e5e5;
    width: 140px;
    position: relative;
    z-index: 3;
    .select-team-winner {
      display: flex;
      align-items: center;
      width: 100%;
    }
    .article {
      width: 100%;
      padding: 7px 0px;
    }
  }
  .date-time {
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: #4a90e2;
    width: 70px;
    font-size: 11px;
    font-weight: bold;
    overflow: hidden;
    z-index: 3;
  }
}
.away-group {
  border-top: 1px solid #fff;
}
</style>
