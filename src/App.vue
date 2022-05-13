<template>
  <section class="main-content">
  
    <div class="container">
      <div class="content">
        <div class="header">
         
          <h1><strong>QR CODE</strong> generator</h1>
         <Transition name="fade">
            <button @click="generateQRCodes" class="btn-classic greenish" v-if="splittedInput" :class="loading.qrCodes && qrCodeCount - gotQRCodes.length >0 ? 'work-in-progress'  : null">{{loading.qrCodes && qrCodeCount - gotQRCodes.length > 0 ?    `${qrCodeCount - gotQRCodes.length} remaining` : `Generate` }}</button>
         </Transition>
        </div>
        <textarea class="cst-textartea" v-model.lazy.trim="inputData"></textarea>
      </div>
    </div>
    <section >
      <MainHolder :qrCodes="gotQRCodes" @prop-has-changed="gotQRCodes = $event" />
    </section>
    <FooterBar />
  </section>
</template>

<script>
import DOMPurify from 'dompurify';
import FooterBar from './components/FooterBar.vue'
import MainHolder from './components/QR/MainHolder.vue'
import axios from 'axios'

export default {
  data (){
    return {
      inputData : null,
      splittedInput : null,
      qrCodeCount : 0,
      loading : {
        qrCodes : false
      },
      gotQRCodes : []
    }
  },
  components : {
    FooterBar,
    MainHolder
  },
  methods : {
    async generateQRCodes(){
      
      const pushData = []
     
        this.loading.qrCodes = true
      
        this.splittedInput.forEach(e => {
         
          if(e !== '' && e) pushData.push( DOMPurify.sanitize(e) )
          
        })

        this.qrCodeCount = pushData.length
       

        this.splittedInput.forEach( (e,index) => {
          this.fetchQR(e,index).then(data => this.gotQRCodes.push(data))
          
        })


        this.gotQRCodes.sort((a, b) => {
          a.id - b.id
        })

       

       
      
      this.inputData = null

      setTimeout(() => {
        window.scrollTo(0,550)
      },1500)

      
    },
    async fetchQR(data,index){
        // console.log(data)
       const url = 'https://qrcode-generator-api.herokuapp.com/qr'
       const payload = {
        "innerData": data,
        "light": "#fff",
        "dark": "#000",
        "width": "600"
      }

       const fetchData = await axios.post(url, payload)

       return {
         id : index,
         data : data,
         qr : fetchData.data?.qr,
         selected : false
       }
      
    }
  },
  watch : {
    inputData(nW){
      
      if(nW !== '' && nW){
        
        this.splittedInput = 0
        let data 
        data =  nW.split('\n')

        data = data.filter(e => e !== '')
        this.splittedInput = data
      }

    },
    gotQRCodes(newValue){
      console.log(newValue)
    }

  }
};
</script>

<style scoped>
.main-content {
  width: min(1200px, 90%);
  margin: 0 auto;

  min-height: 100vh;
  display: flex;
  flex-direction: column;
  padding-top: 10rem;
}

.container {
  background: white;
  border-radius: 4px;
  box-shadow: 0px 0px 111px -36px rgba(0, 0, 0, 0.13);
  padding: 4rem 2rem;
  height: 50vh;
}

.content {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  gap: 25px;
 
}

.content .header {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}

.content .header > button {
  padding: 6px 48px;
  font-size: 1.2rem;
}

.work-in-progress{
  filter: grayscale(1);
  pointer-events: none;
 
}

.cst-textartea{
  width: 100%;
  height: 100%;
  resize: none;
  border-radius: 4px;
  
  background: linear-gradient(180deg, #FFFFFF 0%, #F8F8F8 100%);
  box-shadow: inset 0px 0px 59px -28px rgba(0, 0, 0, 0.25);
  border: none;
  outline: none;
  padding: 2rem;
  box-sizing: border-box;
  font-family: 'Poppins', sans-serif;
  transition: all 0.4s ease-in-out;
  line-height: 2;
}

.cst-textartea:focus{
box-shadow: inset 0px 0px 59px -20px rgba(0, 0, 0, 0.35);
}

/* width */
.cst-textartea::-webkit-scrollbar {
  width: 7px;
  cursor: pointer;
  
}

/* Track */
.cst-textartea::-webkit-scrollbar-track {
  background: #cecece;
  border-left: 1px solid rgba(0, 0, 0, 0.268);
  
}

/* Handle */
.cst-textartea::-webkit-scrollbar-thumb {
  background: linear-gradient(180deg, #F8ED79 0%, #FAD87C 100%);
  border-radius: 999px;
  
}

/* Handle on hover */
.cst-textartea::-webkit-scrollbar-thumb:hover {
  box-shadow: inset 0px 0px 3px 0px rgba(0, 0, 0, 0.111);
  transition: all 0.3s ease-in-out;
}



</style>
