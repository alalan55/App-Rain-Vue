<template>
  <div id="app" :class="typeof weather.main !='undefined' && weather.main.temp > 16 ? 'warm': ''">
    <main>

      <div class="elementos">
        <div class="caixa-pesquisa">
          <input
            type="text"
            class="entrada"
            placeholder="Pesquise um lugar.."
            v-model="query"
            @keypress="pesquisarLugar"
          />
        </div>

<div class="content">


        <div v-if="falhas == false">

        <div class="clima" v-if="typeof weather.main != 'undefined'">
          <div class="caixa-localizacao">
            <div class="localizacao">{{weather.name}}, {{weather.sys.country}}</div>
            <div class="data">{{ dataBuilder()}}</div>
          </div>

          <div class="caixa-tempo">
            <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
            <div class="tempo">{{ weather.weather[0].main }}</div>
          </div>
        </div>
        </div>

        <div class="falha" v-else>
        <h1>Desculpe, não encontramos a loalização digitada..</h1>
      </div>

      </div>

      </div>
      

    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      api_chave: "YOUR_KEY",
      url_base: "http://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
      falhas: false
    };
  },
  methods: {
    pesquisarLugar(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metriic&APPID=${this.api_chave}`
        )
          .then(res => {
            res.status == 404 ? this.falhas = true : this.falhas = false;
          
            return res.json();
          })
          .then(this.setResults);
      }
    },

    setResults(res) {
      this.weather = res;
      console.log(res);
    },
    dataBuilder() {
      let d = new Date();
      let months = [
        "Janeiro",
        "Fevereiro",
        "Março",
        "Abril",
        "Maio",
        "Junho",
        "Julho",
        "Agosto",
        "Setembro",
        "Outubro",
        "Novembro",
        "Dezembro"
      ];
      let days = [
        "Domingo",
        "Segunda-feira",
        "Terça-feira",
        "Quarta-feira",
        "Quinta-Feira",
        "Sexta-feira",
        "Sábado"
      ];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    }
  }
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Montserrat:wght@100;400;500&display=swap");

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: "Montserrat", sans-serif;
}

#app {
  background-image: url("./assets/cold-bg.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
#app.warm {
  background-image: url("./assets/warm-bg.jpg");
}
main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25) rgba(0, 0, 0, 0.75)
  );
}
.caixa-pesquisa {
  width: 100%;
  margin-bottom: 30px;
}
.caixa-pesquisa .entrada {
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgb(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.caixa-pesquisa .entrada:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}

.caixa-localizacao .localizacao {
  color: #fff;
  font-size: 35px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.caixa-localizacao .data {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}
.caixa-tempo {
  text-align: center;
}
.caixa-tempo .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.caixa-tempo .tempo {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.falha h1{
  text-align: center;
  color: #fff;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  font-weight: 700;
  background-color: rgba(255, 255, 255, 0.25);
  padding: 1%;
  border-radius: 15px;

}
.content{
  
  display: flex;
  align-items: center;
  justify-content: center;
  margin-top: 10%;
}
</style>
