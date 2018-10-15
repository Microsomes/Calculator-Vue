<template>
  <div id="app">
    
    <div class="container">
      <div class="row">
        <div class="col m6 offset-m3">
    
    
    <div class="calc">
        <div class="calc_screen">
          {{screen}}
          </div>

          <div class="digits">
            <div v-for="n in digits" :key="n" class="digi" @click="inputDigi(n)">{{n}}</div>
             </div>
      </div>
     
     
      </div>
      </div>

      </div>

   </div>
</template>

<script>

export default{
  data:function(){
    return{
      digits:[0,1,2,3,5,6,7,8,9,"+","-","/","*","="],
      question:'',
      screen:'',
      isResult:false
    }
  },methods:{
    processResult(){
      //result is processed here
      
      var question=this.screen;
      //this is the question

      var operators=[
        "+",
        "-",
        "/",
        "*"
      ]
      //all the operators supported
      //you can add as many operaors as you want here
      var processs={}

      //scan which operator is used
      for(var i=0;i<operators.length;i++){
        var currentOp= operators[i];
        var sp= question.split(currentOp);
        if(sp.length==2){
          processs.op=currentOp;
          processs.left=sp[0];
          processs.right=sp[1];
        }
      }

      var resultcalc= (payload)=>{
        var left= parseFloat(payload.left);
        var right= parseFloat(payload.right);

        switch(payload.op){
          case "*":
          return left*right;
          break;
          case "+":
          return left+right;
          break;
          case "-":
          return left-right;
          break;
          case "/":
          return left/right
          break;
        }
      }
      console.log(processs);
      this.screen="loading";
      var home=this;
      setTimeout(()=>{
        if(resultcalc(processs)==undefined){
          home.screen="ERROR";
          home.isResult=true
        }else{
        home.screen=resultcalc(processs);
        home.isResult=true;
        }
       },1000)
    },
    inputDigi(i){
       //called whenever user enterters a digit
       if(this.isResult){
         this.isResult=false;
         this.screen="";
       }
      if(i=="="){
        //process result
        this.processResult();
        return;
      }
      this.screen=this.screen+i;
    }
  }
}

</script>

<style>

.digits{
  display: flex;
  flex-wrap: wrap;
   justify-content: center;
   align-items: center;
   
}
.digi{
  width: 100px;
  height:100px;
  align-self: center;
  background:black;
  margin: 10px;
  color:white;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 30px;
}
.digi:hover{
  background: lightgray;
  color:black;
  transform: scale(1.3);
  cursor:pointer;
}

.calc{
  border:1px solid grey;
  width: 400px;
  min-height:500px;
  margin-top:30px;
}
.calc_screen{
  border:1px solid black;
  height:150px;
  background:blue;
  display: flex;
  align-items: center;
  justify-content: center;
  color:white;
  font-size: 30px;
}
</style>
