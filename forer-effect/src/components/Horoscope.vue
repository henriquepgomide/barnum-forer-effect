<template>
  <div class="hello">
    <div class="container">
      <h1 class="title">{{ msg }}</h1>

      <div class="columns is-centered">
	<div class="column is-half">

	  <b-tabs
	    v-model="activeTab"
	    type="is-boxed"
	    position="is-centered is-medium"
	    class="block">
	    <b-tab-item label="Introdução">
	      <h2>Sexo</h2>
	      <b-field>
		<div class="is-flex is-horizontal-center">
		  <b-radio-button v-model="sexRadio"
				  native-value="female"
				  type="is-primary is-light is-outlined">
		    <span>Feminino</span>
		  </b-radio-button>
		  <b-radio-button v-model="sexRadio"
				  native-value="male"
				  type="is-primary is-light is-outlined">
		    <span>Masculino</span>
		  </b-radio-button>
		</div>
	      </b-field>
	      <h2>Data de nascimento</h2>
	      <b-field>
		<b-datepicker
		  v-model="selected"
		  placeholder="Click to select..."
		  icon="calendar-today"
		  :locale="locale"
		  editable
		  trap-focus>
		</b-datepicker>
	      </b-field>
	      <b-button 
		type="is-primary is-rounded"
		@click="submit"
		>
		Avaliar Personalidade
	      </b-button>
	    </b-tab-item>
	    <b-tab-item label="Minha Personalidade">
	      <div v-if="!!sexRadio && selected.getYear() < 120">
		<section>
		  <div class="is-flex is-horizontal-center">
		    <figure class="image is-128x128">
		      <img 
			    :src="zodiac_img"
			    ratio="1by1">
		    </figure>
		  </div>
		  <h1 class="title">{{ zodiac }}</h1>
		  <div class="content is-medium">
		    <p>{{ horoscope }}</p>
		    <h2>Por favor, responda o que você achou desta análise</h2>
		    <a class="button is-large is-primary" href="https://forms.gle/HfqkqKtY3irExL8k8" target="_">Pesquisa</a>
		    <p></p>
		  </div>
		</section>
	      </div>
	      <div v-else>
		<h1>Humm! Você ainda precisa completar suas informações.</h1>
	      </div>
	    </b-tab-item>

	    <b-tab-item label="Como funciona">
	      <div class="content is-medium">
		<h2 class="title">Efeito Forer ou Barnum</h2>
		<p>O efeito Forer, também conhecido como Barnum, está ligado ao fato de acreditarmos em avaliações genéricas sobre nós, assumindo que as mesmas são acuradas e personalizadas. Ele explica, pelo menos parcialmente, porque a astrologia e os testes de personalidade são bastante aceitos pela população.</p>
		<p>Usamos este efeito para mostrar a diferença entre Psicologia Científica da Psicologia do Senso Comum. A teoria da personalidade do horóscopo ocidental carece das características do método científico, como definido por Popper. Entretanto, parte do fenômeno pode ser estudado de forma sistemática e falseável.</p>

		<h3>Referências</h3>
		<p>Bunchaft, G., & Krüger, H. (2010). Credulidade e efeito Barnum ou Forer. Temas em Psicologia, 18(2), 469–479. <a href="https://www.redalyc.org/pdf/5137/513751436020.pdf">[Link]</a></p>


	      </div>
	    </b-tab-item>
	  </b-tabs>
	</div>
      </div>  


    </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return{
      selected: new Date(),
      locale: undefined, // Browswer locale
      sexRadio: null,
      activeTab: 0,
    }
  },
  props: {
    msg: String
  },
  methods: {
    shuffle: function(s) {
      for (var i = s.length - 1; i > 0; i--) {  
	var j = Math.floor(Math.random() * (i + 1)); 

	var temp = s[i]; 
	s[i] = s[j]; 
	s[j] = temp; 
      } 
      return s
    },
    submit: function() {
      if (!this.sexRadio || this.selected.getYear() > 120) {
	this.$buefy.toast.open({
	  message: 'Ajude o Místico, você se esqueceu de adicionar seu sexo ou sua data de nascimento.',
	  type: 'is-danger',
	  duration: 3000,
	})
      } else {
	this.activeTab = 1
      }
    }
  },
  computed: {
    zodiac: function () {
      var day = this.selected.getDate()
      var month = this.selected.getMonth()
      var zodiac

      if (month == 11) {
	zodiac = (day < 22) ? "Sagitário" : "Capricórnio"
      } else if (month == 0) {
	zodiac = (day < 20) ? "Capricórnio" : "Aquário"
      } else if (month == 1) {
	zodiac = (day < 19) ? "Aquário" : "Peixes"
      } else if (month == 2) {
	zodiac = (day < 21) ? "Peixes" : "Áries"
      } else if (month == 3) {
	zodiac = (day < 20) ? "Áries" : "Touro"
      } else if (month == 4) {
	zodiac = (day < 21) ? "Touro" : "Gêmeos"
      } else if (month == 5) {
	zodiac = (day < 23) ? "Gêmeos" : "Câncer"
      } else if (month == 6) {
	zodiac = (day < 23) ? "Câncer" : "Leão"
      } else if (month == 7) {
	zodiac = (day < 23) ? "Leão" : "Virgem"
      } else if (month == 8) {
	zodiac = (day < 23) ? "Virgem" : "Libra"
      } else if (month == 9) {
	zodiac = (day < 23) ? "Libra" : "Escorpião"
      } else if (month == 10) {
	zodiac = (day < 23) ? "Escorpião" : "Sagitário"
      } 
      return zodiac
    },
    zodiac_img: function () {
      return require('../assets/zodiac/' + this.zodiac.toLowerCase().normalize('NFD').replace(/[\u0300-\u036f]/g, "") + '.svg')
    },
    horoscope: function () {
      var sex = this.sexRadio
      var sentence
      var finalResult
      var defaultTextMale = 'Algumas de suas aspirações tendem a ser bastante irrealistas. Às vezes você é extrovertido, social e amável; enquanto também pode reservado e ser introvertido. Você já descobriu que não é sábio ser tão franco em se revelar para outras pessoas. Você tem orgulho em se considerar um pensador independente e não aceitar opiniões de outras pessoas sem prova satisfatória. Você prefere uma certa quantidade de mudança e variedade e se irrita com restrições e limitações. Às vezes você possui sérias dúvidas se você fez a decisão certa ou feito a coisa certa. Disciplinado e controlado, você tende a se preocupar e ter um pouco de insegurança. Embora você tenha fraquezas, você geralmente é capaz de compensá-las. Você possui uma grande capacidade para desenvolvimento ainda não explorada para seu benefício. Você tem a tendência de ser crítico a você mesmo. Também, tem uma necessidade de que outras pessoas gostem e admirem você.'
      var defaultTextFemale = 'Algumas de suas aspirações tendem a ser bastante irrealistas. Às vezes você é extrovertida, social e amável; enquanto também pode reservada e ser introvertida. Você já descobriu que não é sábio ser tão franca em se revelar para outras pessoas. Você tem orgulho em se considerar uma pensadora independente e não aceitar opiniões de outras pessoas sem prova satisfatória. Você prefere uma certa quantidade de mudança e variedade e se irrita com restrições e limitações. Às vezes você possui sérias dúvidas se você fez a decisão certa ou feito a coisa certa. Disciplinada e controlada, você tende a se preocupar e ter um pouco de insegurança. Embora você tenha fraquezas, você geralmente é capaz de compensá-las. Você possui uma grande capacidade para desenvolvimento ainda não explorada para seu benefício. Você tem a tendência de ser crítica a você mesma. Também, tem uma necessidade de que outras pessoas gostem e admirem você.'

      if (sex == 'male') {
	sentence = defaultTextMale.split('. ')
	finalResult = this.shuffle(sentence).join('. ')
	return finalResult
      } else if (sex == 'female') {
	sentence = defaultTextFemale.split('. ')
	finalResult = this.shuffle(sentence).join('. ')
	return finalResult
      } else {
	return '...'
      }
    }

  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1 {
  font-family: 'Mystery Quest', cursive;
  font-size: 2.5rem;
}

h2 {
  font-family: 'Mystery Quest', cursive;
  font-size: 1.8rem;
}

h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

.is-horizontal-center {
  justify-content: center;
}
</style>
