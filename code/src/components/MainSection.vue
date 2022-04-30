<template>
    <div class="container">
        <div class="title">
            <h3 class="form-title"> Como é o frete que você precisa? </h3>
            <hr class="hr1">
        </div>

        <form action="">
            <div class="destiny">
                <h2>Destino</h2>
                <select v-model="citySelected">
                    <option v-for="(city, index) in cities" :key="index">{{city}}</option>
                </select>
            </div>

            <div> 
                <h2>Peso</h2>
                <input v-model="weight" type="number"/>
            </div>

            <div class="button">
                <button class="submit-button" type="button" @click="calculateBestOptions"> 
                    Analisar
                </button>
            </div>
        </form>

        <div class="bestoptions">
            <h3>Estas são as melhores alternativas de frete que encontramos para você</h3>
            <hr class="hr1">
            <div class="cheaper-shipping">
                <img :src="require('../assets/saving.png')" class="saving"/>
                <h5>    
                    Frete mais barato: 
                    <strong v-if="initialized"> 
                        {{cheaperCarrier.name}} - {{cheaperCarrier.shipping_price}} - {{cheaperCarrier.lead_time+"h"}}
                    </strong>
                </h5>
            </div>
            <div class="faster-shipping">
                <img :src="require('../assets/speedometer.png')" class="speedometer"/>
                <h5>
                    Frete mais rápido:
                    <strong v-if="initialized"> 
                        {{fasterCarrier.name}} - {{fasterCarrier.shipping_price}} - {{fasterCarrier.lead_time+"h"}}
                    </strong>
                </h5>
            </div>
        </div>
        <inputForm/>
    </div>
</template>

<script>
    import api from '../services/api'
    
    export default {
        name: 'MainSection',
        components: {
        },
        data() {
            return {
                apiData: [],
                cities: [],
                citySelected: "",
                weight: "",
                cheaperCarrier: {},
                fasterCarrier: {},
                initialized: false,
            }
        },
        created() {
            this.getData();
        },
        methods: {
            getData() {
                api
                .get()
                .then((res) => {
                    this.apiData = res.data
                    this.cities = this.apiData.map(
                        item => item.city
                    )
                    // retira duplicação de cidades
                    this.cities = this.cities.filter((element, index, arr) => arr.indexOf(element)==index)
                    this.formateValues()
                })
                .finally(
                    // utilizei isso aq para verificar se os hooks estavam corretos
                    () => {
                        console.log("api data", this.apiData)
                        console.log("cities", this.cities)
                    }
                )
                .catch((error) => {
                    console.log(error);
                });
            },
            formateValues() {
                // esse função formata todos os valores para Number
                this.apiData.map(
                    element => {
                        element['cost_transport_light'] = parseFloat(element['cost_transport_light'].replace('R$', ''))
                        element['cost_transport_heavy'] = parseFloat(element['cost_transport_heavy'].replace('R$', ''))
                        element['lead_time'] = parseFloat(element['lead_time'].replace('h', ''))
                        return element
                    }
                )
            },
            calculateBestOptions() {
                this.initialized = true;
                //criei essa variável para que os valores das minhas variáveis declaradas(algumas como undefined) no data() não aparecessem até eu clicar a primeira vez no botão

                const carriersFiltered = this.apiData.filter(
                    element => element.city === this.citySelected
                )

                this.cheaperCarrier = carriersFiltered[0];
                this.fasterCarrier = carriersFiltered[0];
                // eu faço tanto cheaperCarrier como fasterCarrier iniciar com o primeiro objeto do objeto filtrado

                const weightShip = this.weight< 100 ? 'cost_transport_light' : 'cost_transport_heavy'; 
                //seleciono com ternário qual preço vou utilizar
                carriersFiltered.forEach(
                    element => {
                        if(element[weightShip] < this.cheaperCarrier[weightShip]) {
                            //se o valor do meu element for menor do que o valor salvo no cheaperCarrier, eu atualizo cheaperCarrier
                            this.cheaperCarrier = {...element};
                        }
                        if(element.lead_time < this.fasterCarrier.lead_time) {
                            //se o valor do meu element for menor do que o valor salvo no fasterCarrier, eu atualizo fasterCarrier
                            this.fasterCarrier = {...element};
                        }
                    }
                )
                // console.log("Transportadoras filtradas: ",carriersFiltered)
                // console.log("Transportadora mais barata: ",this.cheaperCarrier)
                // console.log("Transportadora mais rápida: ",this.fasterCarrier)

                let newValueCheaper = this.weight * this.cheaperCarrier[weightShip];
                let formattedValueCheaper = newValueCheaper.toLocaleString('pt-BR', { style: 'currency', currency: 'BRL' });
                this.cheaperCarrier = {...this.cheaperCarrier, shipping_price: formattedValueCheaper};

                let newValueFaster = this.weight * this.fasterCarrier[weightShip];
                let formattedValueFaster = newValueFaster.toLocaleString('pt-BR', { style: 'currency', currency: 'BRL' });
                this.fasterCarrier = {...this.fasterCarrier, shipping_price: formattedValueFaster};
                // aqui eu adiciono os valores formatados pro real ao meu objeto com uma propriedade nova denominada 'shipping_price'
            }
        }
    };
</script>

<style>
.container {
    display: flex;
    flex-direction: column;
    margin: 2rem 0;
}

h3 {
    color:#114844;
}

.hr1 {
    border: 0;
    height: 2px;
    width: 35rem;
    background-image: linear-gradient(to right, transparent, #CCC, transparent);  
}

form {
    display: flex;
    margin-left: 2rem;
    align-items: left;
    justify-content: center;
    flex-direction: column;
    gap: 1rem;
    width: 25rem;
}

select, input {
    border: 0;
    border-radius: 0.5rem;
    background: #D4DEDF;
    width: 25rem;
    padding: 0.5rem 1rem;

    transition: filter 0.3s;
}

select:hover, input:hover {
    filter: contrast(0.8)
}

div .button {
    display: flex;
    justify-content: center;
    align-items: center;
}

div .submit-button {
    border: 0;
    background: #86d1ac;
    width: 10rem;
    padding: 1rem;
    margin: 2rem 0;
   
    font-weight: 700;

    transition: filter 0.3s, width 0.3s;
}

.submit-button:hover {
    filter: contrast(0.85);
    width: 10.5rem;
}

.faster-shipping, .cheaper-shipping {
    width: 87.25%;
    border-radius: 0.25rem;
    margin-top: 1rem;
    margin-left: 2rem;
    padding: 1rem;
}       

.faster-shipping{
    background: #bae1ff;
}

.cheaper-shipping{
    background: #ffffba;
}

h5 {
    display: inline;
    margin: 1rem;
}

h5 strong {
    margin-left: 1rem;
}

.saving, .speedometer {
    width:2rem;
}

</style>