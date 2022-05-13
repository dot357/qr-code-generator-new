<template>
    <div class="single-qr">
        
        <div class="content-box">
        
        <p @click="computedItem.selected = !computedItem.selected" :class="computedItem.selected ? 'active' : null" class="idBox">{{id < 9 ? `0${id+1}` : id+1}}</p>
        <p class="data-holder"><strong>{{computedItem.data}}</strong></p>
        </div>
        <div class="image-box">
            <img @click="computedItem.selected = !computedItem.selected" :src="computedItem.qr" alt="">
        </div>
    </div>
</template>

<script>
export default {
    props : {
        item : {
            type : Object,
            required : true
        },
        id : {
            type : Number ,
            required : true
        }
    },
    computed : {
        computedItem : {
            get(){
                return this.item
            },
            set(value){
                console.log(value)
                this.$emit('prop-changed',value)
            }
        }
    }
}
</script>

<style scoped>
.single-qr{
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  position: relative;
  padding: 2rem 0;
}

.single-qr .content-box{
    width: 80%;
    
}

.single-qr .image-box{
    width: 20%;
    
    height: 100%;
}

.single-qr  .image-box img {
    width: 100%;
    object-fit: contain;
    cursor: pointer;
   
}


.single-qr img:hover + div .idBox{
    color: #00D121;
}

.single-qr .idBox{
    position: absolute;
    top: 0;
    left: 0;
    color: #DAD5D5;
    font-weight: 900;
    font-size: 2.25rem;
    cursor: pointer;
    transition: color 0.4s ease-in-out;
}

.single-qr .idBox.active{
    color: #00D121;
}

.single-qr .idBox:hover{
    color: #00D121;
}


.single-qr:not(:last-child){
    margin-bottom: 4.188rem;
}

.data-holder{
    width: 90%;
    text-align: left;
  
}
</style>