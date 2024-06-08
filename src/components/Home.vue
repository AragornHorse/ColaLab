<template>

  <div class="home">

    <div class="search-bar">
      <input type="text" v-model="search_content" placeholder="Search for models or datasets..." />
      <button @click="search_home(search_content)">Search</button>
    </div>

    <div class="ai-open">
      <div>
        <img src="../assets/ai.png" alt="" @click="open_ai">
        <div style="font-size: 12px;">
          Artificial Idiot
        </div>
      </div>

      <div class="ad">

        <div style="height: 5%;"</div>

        <div class="ad-item">
          <img src="../assets/cola.jpg" alt="">
          <p style="width: 70%; margin-left: 12%;">
            <b> Torch Cola </b><br>
            The Torch brand cola is your only choice!
          </p>
        </div>

        <div style="height: 5%;"</div>

        <div class="ad-item">
          <img src="../assets/torch.jpg" alt="">
          <p style="width: 70%; margin-left: 12%;">
            <b> Cola Torch </b><br>
            The Colar brand torch is your only choice!
          </p>
        </div>

      </div>

      <div class="chat" id="message">

        <div class="message-box">
          <div v-for="message in messages" :class="message.usr">
            <div> {{ message.msg }} </div>
          </div>
        </div>

        <div style="height: 1%;"></div>
        <div style="display: flex;">
          <textarea name="" id=""></textarea>
          <button @click="get_message">sent</button>
        </div>
      </div>

    </div>

    <div class="cards">
      <Card v-for="(card, idx) in cards" :key="idx" :type="card.type" :name="card.name" :description="card.description"
      :image="card.image" :url="card.url"/>
    </div>

    <div style="height: 50px;"></div>

    <div class="bottle">
      <img src="../assets/hit.png" alt="">
      <img src="../assets/cs.png" alt="">
      <img src="../assets/sair.png" alt="">
      <img src="../assets/mdc.png" alt="">
    </div>

  </div>

</template>

<script>
import axios from 'axios';
import Card from './Card.vue';

const requireContext = require.context('@/assets/model_images', false, /\.(png|jpe?g|svg)$/);

export default {
  name: 'Home',
  data() {
    return {
      search_content: "",
      cards: [
        {
          id: 5,
          type: "Model",
          name: "ChatGPT",
          description: "generative pretrained transformer based on HW algorithm.",
          image: "https://nimg.ws.126.net/?url=http%3A%2F%2Fdingyue.ws.126.net%2F2024%2F0510%2F1449b493j00sd9c8f00n1d000rc00i9m.jpg&thumbnail=660x2147483647&quality=80&type=jpg",
          url: "http://www.baidu.com"
        }, {
          id: 6,
          type: "Dataset",
          name: "HW-100k",
          description: "dataset for HW algorithm.",
          image: null
        },{
          id: 7,
          type: "Model",
          name: "HW-GPT",
          description: "generative pretrained transformer based on HW algorithm.",
          image: "https://nimg.ws.126.net/?url=http%3A%2F%2Fdingyue.ws.126.net%2F2024%2F0510%2F1449b493j00sd9c8f00n1d000rc00i9m.jpg&thumbnail=660x2147483647&quality=80&type=jpg"
        }
      ],
      messages: [
        {
          usr: "ai",
          msg: "love you"
        }, {
          usr: "me",
          msg: "no way"
        }
      ]
    };
  },
  
  components: {
    Card
  },

  mounted() {
    this.loadImages();
    this.search_home(this.search_content);
  },

  methods: {
    loadImages() {
      requireContext.keys().forEach((fileName) => {
        const imageName = fileName.replace('./', '').replace(/\.\w+$/, '');
        for (var i=0; i<this.cards.length; i++){
          if(this.cards[i]['name'] == imageName){
            this.cards[i]['image'] = requireContext(fileName);
          }
        }
      });

      for (var i=0; i<this.cards.length; i++){
          if(this.cards[i]['image'] == null || this.cards[i]['image'].length == 0){
            this.cards[i]['image'] = "https://nimg.ws.126.net/?url=http%3A%2F%2Fdingyue.ws.126.net%2F2024%2F0510%2F1449b493j00sd9c8f00n1d000rc00i9m.jpg&thumbnail=660x2147483647&quality=80&type=jpg";
          }
        }
    },
    async search_home(content){
      try {
        const response = await axios.get('http://localhost:8081/api/home/select?home_key=' + content);
        var cards = response.data;
        this.$nextTick(() => {
          this.cards = cards;
          this.loadImages();
        });
      } catch (error) {
        console.error('Error fetching categories:', error);
      }
    },
    open_ai(){
      if(document.getElementById("message").style.display == 'none'){
        document.getElementById("message").style.display = 'block';
      }else{
        document.getElementById("message").style.display = 'none';
      }
    },
    async get_message() {
      
    }
  }
};
</script>

<style scoped>
.bottle {
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
}

.bottle img {
  align-items: center;
  text-align: center;
  width: 8%;
  padding: 3%;
}

.home {
padding: 2rem;
}
.cards {
display: flex;
flex-wrap: wrap;
gap: 1rem;
justify-content: center;
}
.search-bar {
display: flex;
justify-content: center;
margin-bottom: 2rem;
}
.search-bar input {
width: 50%;
padding: 0.5rem;
margin-right: 1rem;
border: 1px solid #ccc;
border-radius: 4px;
}
.search-bar button {
padding: 0.5rem 1rem;
border: 1px solid #ccc;
border-radius: 4px;
background-color: #007bff;
color: white;
cursor: pointer;
}
.search-bar button:hover {
background-color: #0056b3;
}
.ai-open {
  top: 15%;
  left: 1%;
  position: fixed;
  width: 10%;
  height: 100%;
  cursor: pointer;
}

.ai-open img {
  width: 40%;
}

.chat {
  margin-left: 3%;
  height: 100%;
  display: none;
}

.message-box {
  height: 40%;
  border: 1px solid black;
  padding: 5px;
}
.me {
  width: 50%;
  background-color: rgba(67, 216, 100, 0.919);
  margin-top: 3%;
  margin-left: 50%;
}

.ai {
  width: 50%;
  background-color: rgba(220, 220, 220, 0.829);
  margin-top: 3%;
}

.ad {
  width: 15%;
  left: 85%;
  top: 10%;
  height: 90%;
  position: fixed;
}

.ad-item {
  width: 90%;
  margin-left: 5%;
  margin-right: 5%;
  margin-top: 5%;
}

</style>
