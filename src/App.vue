
//import HelloWorld from "./components/HelloWorld.vue";
import Vue from 'vue'
import App from './components/App/App';


new Vue({
  el: "#app",
  render: h => h(App),
  data: {
    sales: [
      [
        { Year: 2018, Sale: 512 },
        { Year: 2018, Sale: 1025 },
        { Year: 2018, Sale: 70 }
      ],
      [
        { Year: 2017, Sale: 155 },
        { Year: 2017, Sale: 12 },
        { Year: 2017, Sale: 69 }
      ],
      [
        { Year: 2016, Sale: 8767 },
        { Year: 2016, Sale: 29 },
        { Year: 2016, Sale: 72 }
      ]
    ]
  },
  methods: {
    clickThatMothafucka: function(some) {
      console.log("shit was clicked", some);
    }
  }
});

