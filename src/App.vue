<template>
  <div id="app">
    <div>
      <b-jumbotron class="vrh" header="WORLD TEAMS" lead="All football teams in The World ">
        <div>
          <b-dropdown show variant="primary" id="dropdown-1" text="Dropdown Button" class="m-md-2">
            <b-dropdown-item href="https://www.espn.com/soccer/table/_/league/eng.1">Table</b-dropdown-item>
            <b-dropdown-item href="https://www.espn.com/soccer/scoreboard">Results</b-dropdown-item>
            <b-dropdown-item href="https://www.espn.com/soccer/transfers">Transfers</b-dropdown-item>
          </b-dropdown>
        </div>
      </b-jumbotron>

      <div class="search-box">
        <b-form-input
          type="text"
          class="search-bar"
          placeholder="Pretraži"
          v-model="search"
          @keyup.enter="pretrazi"
        />
      </div>
    </div>

    <b-row>
      <b-col cols="2" v-for="team in timovi" v-bind:key="`team_` + team.idTeam">
        <b-card
          width="100px"
          :title="team.strTeam"
          :img-src="team.strTeamBadge"
          img-top
          tag="article"
          style="width: 14rem; margins: 20rem"
          class="mb-200"
        >
          <hr />
          <b-card-text>
            <h5>{{ team.strKeywords }}</h5>
          </b-card-text>
          <hr />
          <b-card-text>{{ team.strStadium }}</b-card-text>
          <hr />
          <b-button href="https://www.espn.com/soccer/" variant="primary">More</b-button>
        </b-card>
      </b-col>
    </b-row>

    <br />

    <b-row class="cal">
      <b-col md="auto">
        <b-calendar locale="en-US"></b-calendar>
      </b-col>
    </b-row>
  </div>
</template>

<script>
export default {
  name: "App",

  data() {
    return {
      search: ``,
      timovi: []
    };
  },
  methods: {
    pretrazi: function() {
      this.axios
        .get(
          "https://www.thesportsdb.com/api/v1/json/1/searchteams.php?t=" +
            this.search
        )
        .then(response => {
          console.log(response.data.teams);

          this.timovi = response.data.teams;

          for (let i = 0; i < this.length; i++) {
            if (this.timovi[i].strTeamBadge == null) {
              this.timovi[i].strTeamBadge =
                "https://i.pinimg.com/originals/44/ba/b4/44bab4d4ab30bd4663c1de34142c8c02.png";
            }
          }
        });
    }
  }
};
</script>

<style>
body,
html {
  height: 100%;
  background-image: url("https://populous.com/wp-content/uploads/2018/01/POP_98_1683_00_1683_WembleyStadium_Exterior_Night2_Hufton_Crow.jpg");
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
}

.vrh {
  color: whitesmoke;
  font-style: italic;
  background-image: url("https://media-assets-02.thedrum.com/cache/images/thedrum-prod/s3-news-tmp-90538-og-default--2x1--940.jpg");
  background-size: contain;
  background-position: bottom;
  transition: 0.5s;
  font-family: Impact, Haettenschweiler, "Arial Narrow Bold", sans-serif;
  border-bottom-width: thick, black;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;

  color: #313131;
  font-size: 20px;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}
</style>
