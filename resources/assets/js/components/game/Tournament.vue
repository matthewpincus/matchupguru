<template lang="html">
<div>
  <div class="form-group">
  <div class="col-sm-6">
  <label>Username * </label>
  <input type="text"  name="name" value="usera" class="form-control" disabled :value="user">
</div>
  </div>
  <div class="form-group" >
    <div class="col-sm-6">
  <label>Format *</label>
  <input type="text" class="form-control" :value="format_name" disabled>
    </div>
  </div>
<div class="form-group">
  <div class="col-sm-6">
<label>Your Deck *</label>
<input type="text" class="form-control" :value="current_deck_name" disabled>
</div>
</div>
<div class="form-group">
  <div class="col-sm-6">
<label>Opponent  username *</label>
<input type="text" class="form-control"  v-model="opponent">
</div>
</div>

 <div class="form-group">
    <div class="col-sm-12">
  <label>Top 8</label>
<input type="checkbox"  checked  v-model="top_8">
 </div>
</div>




<div class="form-group">
  <div class="col-sm-6">
<label style="width:100%;">
<div class="row"><div class="col-xs-4  col-md-4">Opponent Deck *</div> <div class="col-xs-offset-6 col-md-offset-6 text-primary ">
  <a href="#" data-toggle="modal" title="Add a new Deck" data-target="#deckAddModal">Add a new Deck?</a></div></div>
</label>
<select class="form-control" v-model="opponentdeck">
  <option v-for="deck in decks" :value="deck.id">{{deck.name}}</option>
</select>
</div>
</div>
<div class="form-group">
    <div class="col-sm-6">
  <label>Game 1 play/draw *</label>
<select class="form-control" v-model="g1" >
  <option value="p">Play</option>
  <option value="d">Draw</option>
</select>
</div>
</div>
<div class="form-group">
    <div class="col-sm-6">
  <label>Game 1 Starting hand size</label>
<input type="number" class="form-control" min="0"  name="game1" v-model="g1start">
  </div>
</div>
  <div class="form-group">
    <div class="col-sm-6">
  <label>Game 1 Starting hand size of opponent</label>
<input type="number" class="form-control"   min="0" name="game1size" v-model="g1value">
  </div>
</div>
<div class="form-group">
  <div class="col-sm-6">
  <label>Game 1 Result *</label>
  <select class="form-control" v-model="w1" @change="calculatewin()">
    <option value="w">Win</option>
    <option value="l">Loss</option>
  </select>
</div>
</div>
<div class="form-group">
<div class="col-sm-6">
  <label>Game 2 play/draw *</label>
<select class="form-control" v-model="g2" >
  <option value="p">Play</option>
  <option value="d">Draw</option>
</select>
</div>
</div>


<div class="form-group">
  <div class="col-sm-6">
  <label>Game 2 Result *</label>
  <select class="form-control" v-model="w2" @change="calculatewin()">
    <option value="w">Win</option>
    <option value="l">Loss</option>
  </select>
</div>
</div>

  <div class="form-group">
    <div class="col-sm-6">
  <label>Game 2 Starting hand size</label>
<input type="number" class="form-control" min="0"  name="game2" v-model="g2start">

  </div>
</div>

  <div class="form-group">
    <div class="col-sm-6">
  <label>Game 2 Starting hand size of opponent</label>
<input type="number"  class="form-control" min="0" name="game2size" v-model="g2value">

  </div>
</div>

<div class="form-group">
<div class="col-sm-6">

  <label>Game 3 play/draw</label>
<select class="form-control" v-model="g3" :disabled="notdis">
  <option value="p">Play</option>
  <option value="d">Draw</option>
</select>

</div>
</div>

<div class="form-group">
  <div class="col-sm-6">
  <label>Game 3 Result</label>
  <select class="form-control" v-model="w3" @change="calculatewin()" :disabled="notdis">
    <option value="w">Win</option>
    <option value="l">Loss</option>
  </select>

</div>
</div>

  <div class="form-group">
    <div class="col-sm-6">
  <label>Game 3 Starting hand size</label>
<input type="number" class="form-control" min="0" v-model="g3start" :disabled="notdis" >

  </div>
</div>

  <div class="form-group">
    <div class="col-sm-6">
  <label>Game 3 Starting hand size of opponent</label>
<input type="number" class="form-control" min="0" v-model="g3value" :disabled="notdis">
  </div>
</div>


  <div class="form-group">
    <div class="col-sm-6">
  <label>Match Result</label>
 <select class="form-control" v-model="result">
   <option value="w">Win</option>
   <option value="l">Loss</option>
 </select>
 </div>
</div>



  <div class="form-group">
    <div class="col-sm-6">
  <label>Key cards</label>
<input type="text" class="form-control" v-model="key">
 </div>
</div>

  <div class="form-group">
    <div class="col-sm-6">
  <label>Duds </label>
<input type="text" class="form-control"  v-model="duds">
 </div>
</div>


<div class="form-group">
    <div class="col-sm-12">
  <label>Side notes</label>
<input type="text" class="form-control"  v-model="note">
 </div>
</div>
<div class="col-sm-12" style="margin-top:2%;">
  <div class="text-center">
    <button  v-if="showFinishAndSubmit" class="btn btn-success" @click="savedata(1)" >Submit and Finish</button>
    <button :class="{'btn btn-primary':true ,'disabled':submitted} " @click="savedata(0)" > submit</button>
  </div>
</div>

<div class="col-sm-12">
 <div id="deckAddModal" class="modal fade" role="dialog">
  <div class="modal-dialog modal-lg">

    <div class="modal-content">
      <div class="modal-header">
        <button type="button" class="close" data-dismiss="modal">&times;</button>

      </div>
      <div class="modal-body">

        <create-deck  :format_id="currentformat"></create-deck>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
      </div>
    </div>

  </div>
</div>
</div>




</div>


</template>

<script>
    import createDeck from "../user/suggest-deck.vue";
export default {
  props:{
    gameid:null,
      tournament_name:null,
      tournament_size:null,
    event_type:null,
      deck_id:null,
      slug:null,
   },
  data(){
    return{
    user:null,
    user_id:null,
    opp_id:null,
    opponent:null,
    currentformat:null,
    currentdeck:null,
    opponentdeck:null,
    format_name:null,
    format_id:null,
    decks:null,
    root:null,
    g1:'p',
    g2:'d',
    g3:'p',
    w1:'w',
    w2:'w',
    w3:'w',
    result:'w',
    g1start:null,
    g1value:null,
    g2start:null,
    g2value:null,
    g3start:null,
    g3value:null,
    g3error:false,
    g3sizeerror:false,
    key:null,
    duds:null,
    note:null,
    notdis:true,
    current_deck_name:null,
    league_id:null,
    submitted:false,
    shownewform:false,
    currentadddeck:null,
   currentaddstyle:null,
   currentadddesc:null,
        top_8:true,
        first:false,
        showFinishAndSubmit:false

    }
  },
  methods:{
    savedata:function(finish){


      this.g3error=false;
      this.g3sizeerror=false;

      if(this.event_type!=7){
                if(this.opponent==null){
      this.root.showsuccess("Insert the opponent username");
      return;
    }

      }


      this.$validator.validateAll().then(result=>{
        if(result){
            if(this.first==true)
                return;

            this.first=true;

      let senddata={
        opponent_name:this.opponent,
        format_id:this.currentformat,
        deck_id:this.currentdeck,
        opp_deck_id:this.opponentdeck,
        g1start:this.g1start,
        g1value:this.g1value,
        g2start:this.g2start,
        g2value:this.g2value,
        g3start:this.g3start,
        g3value:this.g3value,
        g1:this.g1,
        g2:this.g2,
        g3:this.g3,
        w1:this.w1,
        w2:this.w2,
        w3:this.w3,
        result:this.result,
        key:this.key,
        duds:this.duds,
        note:this.note,
        game_id:this.gameid,
          top_8:this.top_8,
          tournament_name:this.tournament_name,
          tournament_size:this.tournament_size,
          type:this.event_type,
          tournament_id:this.slug,
          finish:finish
      };

 this.submitted=true;



      axios.post('/tournament',senddata)
  .then( (data)=> {

    if(data.data==1){
      window.location.href="/game/magic/match";
    }
  })
  .catch((error)=> {
    this.submitted=false;
    this.first=false;
  });
    }else{
            this.root.showsuccess("Please fix the errors!!!");
          }
      })
    },
    calculatewin:function(){
    if(this.w1=='w'){
     this.g2='d';
     if(this.w2=='w'){
       this.result='w';
       this.w3='w';
      this.notdis=true;
      return;
     }
    if(this.w2=='l'){
      this.notdis=false;
       this.g3='p';
      if(this.w3=='w'){
        this.result='w'
      }else{
        this.result='l'
      }
      return;
  }
  }
    if(this.w1=='l'){
     this.g2='p';
     if(this.w2=='l'){
       this.result='l';
       this.w3='l';
      this.notdis=true;

     }
    if(this.w2=='w'){
      this.notdis=false;
       this.g3='d';
       if(this.w3=='w'){
         this.result='w'
       }else{
         this.result='l'
       }
       return;

  }
  }
  }
    ,
  startgame:function(){

           let slug;

        if(this.slug==null||this.slug==undefined)
           slug=0;

    if(slug==0){
        axios.get("/load/"+this.deck_id).then((data)=>{
            this.user=data.data.user.magic_name;
            this.decks=data.data.decks;
            this.user_id=data.data.user.id;
            this.currentformat=data.data.format.id;
            this.format_name=data.data.format.name;
            this.current_deck_name=data.data.deck.name;
            this.currentdeck=data.data.deck.id;
            this.opponentdeck=this.decks[0].id;
        }).catch((error)=>{

        });


    }else{
        axios.get("/tournament/load-data/"+this.slug).then((response)=>{
          let  return_data=response.data;
           this.user=return_data.magic_name;
           this.decks=return_data.opponent_decks;
           this.currentformat=return_data.format_id;
           this.format_name=return_data.format_name;
           this.opponentdeck=this.decks[0].id;
           this.currentdeck=this.deck_id;
            this.current_deck_name=return_data.deck_name;
        })
    }



  },
  changedeck:function(){
axios.get("/format/"+this.currentformat).then((data)=>{
  this.decks=data.data.decks;
  this.currentdeck=this.decks[0].id;
  this.opponentdeck=this.decks[0].id;
  this.root.showsuccess("Format Changed Successfully");
}).catch((error)=>{
});
  },
  adddeck:function(){
    axios.post('/user/deck/add', {
        format:this.currentformat,
        deck:this.currentadddeck,
        style:this.currentaddstyle,
        description:this.currentadddesc
      })
      .then( (response) =>{

    if(response.data==0){
      this.root.showsuccess("Deck already exist")
      return;
    }
    location.reload();

      })
      .catch((error)=> {
      console.log(error);
      });

  }
},
  mounted(){
  this.startgame();
  if(this.leagueid!=null){
    this.league_id=this.leagueid;
  }
  this.root=this.$root.$root;

  if(this.slug!=null)
      this.showFinishAndSubmit=true;

},
    components:{
        "create-deck":createDeck
    }


}
</script>

<style lang="css">
</style>
