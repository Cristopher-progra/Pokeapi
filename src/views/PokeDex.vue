<template>
    <ion-page>
        <ion-header>
            <ion-toolbar>
                <ion-title>
                    Pokemon
                </ion-title>
            </ion-toolbar>
        </ion-header>
        <ion-content>
            <ion-grid>
                <ion-row>
                    <ion-searchbar class="ion-no-padding ion-padding-top " color="ligth" 
                    placehaolder="Escriba una cantidad"
                    fill="solid"
                    type="number"
                    v-model.number="cantidad">
                    </ion-searchbar>
                    <ion-button color="dark" size="small" expand="full" shape="round"
                    @click="getData">
                        Mostrar Pokemon
                    </ion-button>    
                </ion-row>

                <div class="ion-padding" v-for="(pokemon, index) in pokemonArray" :key="index">             
                    <ion-card :style="{backgroundColor: pokemon.colorCard}">
                        <ion-card-content>
                            <ion-row>
                                <ion-col size="5" class="contPokemon">
                                    <ion-img :src="pokemon.sprites.other.dream_world.front_default" class="pokemon">

                                    </ion-img>

                                </ion-col>
                                <ion-col size="7" class="contData">
                                    <ion-card-subtitle class="numero">
                                        ID: #{{ pokemon.id }}
                                    </ion-card-subtitle>
                                    <ion-card-title>
                                        {{ pokemon.name }}
                                    </ion-card-title>
                                    <ion-card-subtitle>
                                        <ion-badge>
                                            <ion-icon :icon="pokemon.icono">

                                            </ion-icon> {{ pokemon.types[0].type.name }}
                                        </ion-badge>
                                    </ion-card-subtitle>
                                </ion-col>
                            </ion-row>

                        </ion-card-content>
                    </ion-card>
                </div>
            </ion-grid>
        </ion-content>
    </ion-page>
   
</template>

<script>
import{IonPage,IonHeader,IonToolbar,IonTitle,IonContent,IonGrid,IonRow,IonCol,
IonSearchbar,IonButton,IonCard,IonCardHeader,IonCardContent,IonImg,IonCardSubtitle,IonCardTitle,IonBadge} from '@ionic/vue'
import{flash,water,leaf,flame,warning,medal,paperPlane} from 'ionicons/icons'

export default{
    name:'PokeDex',
    component:{
        IonPage,IonHeader,IonToolbar,
        IonTitle,IonContent,IonGrid,
        IonRow,IonCol,
        IonSearchbar,IonButton,
        IonCard,IonCardHeader,IonCardContent,IonImg,
        IonCardSubtitle,IonCardTitle,IonBadge
    },
    data(){
        return{
            flash,water,leaf,flame,warning,medal,paperPlane,
            //ARREGLO pARA ALMACENAR
            pokemonArray:[],
            //Para almacenar el id de cada pokemon
            pokemonId: 1,
            //cantidad de pokemon a buscar
            cantidad : 10,
            //maximo de pokemon
            maxId: 807,

        }
    },
    methods:{
        async getData(){
            try{
                //Reiniciar mi arreglo de poquemon
                this.pokemonArray = [];

                for ( let i = 0; i < this.cantidad; i++){
                    this.pokemonId = Math.floor(Math.random() * this.maxId) + 1;

                    try{
                         //Realizar solicitud info
                        const response = await fetch(`https://pokeapi.co/api/v2/pokemon/${this.pokemonId}`);

                        //verificar respiuesta exitosa
                        if(!response.ok){
                            throw new Error ('No se puede obtener la data');
                        }
                        //convertir la respuesta a jason
                        const data = await response.json();

                        if(!data.sprites.other.dream_world.front_default){
                            i--;

                        }else{
                            switch(data.types[0].type.name){
                                case 'dragon':
                                    data.colorCard = '#7038F8';
                                    data.icono = this.paperPlane;
                                //asignar el pokemon al array
                                    this.pokemonArray.push(data);
                                    break;
                                case 'fighting':
                                    data.colorCard = '#F08030';
                                    data.icono = this.medal;
                                //asignar el pokemon al array
                                    this.pokemonArray.push(data);
                                    break;
                                case 'fire':
                                    data.colorCard = '#C03028';
                                    data.icono = this.flame;
                                //asignar el pokemon al array
                                    this.pokemonArray.push(data);
                                    break;
                                case 'water':
                                    data.colorCard = '#6890F0';
                                    data.icono = this.water;
                                //asignar el pokemon al array
                                    this.pokemonArray.push(data);
                                    break;
                                case 'grass':
                                    data.colorCard = '#78C850';
                                    data.icono = this.leaf;
                                //asignar el pokemon al array
                                    this.pokemonArray.push(data);
                                    break;
                                case 'electric':
                                    data.colorCard = '#F8D030';
                                //asignar el pokemon al array
                                    this.pokemonArray.push(data);
                                    data.icono = this.flash;
                                    break;
                                case 'psychic':
                                    data.colorCard = '#F55099';
                                    data.icono = this.warning;
                                    //asignar el pokemon al array
                                    this.pokemonArray.push(data);
                                    break;
                                default:
                                    i--;
                                    break;
                                                        
                            }
                        
                        }

                    }catch(error){
                        console.log("Ha ocurrido un error",error);
                    }

                }
                
                 //log de la data
                console.log(this.pokemonArray);
               
            }catch(error){
                console.log("Ha ocurrido un error: ",error);
            } 
        }
    },
    mounted() {
        this.getData();
    },
}

</script>

<style>
ion-button{
    padding-top: 10px;
    margin: auto;  
}
ion-card{
    border-radius: 8px;
    margin:3% 0%;

}
ion-card-content{
    display: contents;
}
.contPokemon{
    background: rgba(0,0,0,0.3);
    border-radius: 50% 50% 50% 0;
}
.contData{
    justify-content: center;
    margin: auto;
    padding-left: 10%;

}
.pokemon{
    position: relative;
    height: 200px;
    margin: auto;
    max-width: 200px;

}
ion-badge{
    margin-top: 10px;
    background: rgba(0,0,0,0.3);

}
ion-card-title{
    font-weight: 900;
}
.numero{
    color: rgba(0,0,0,0.6);
    font-weight: bold
}
</style>

