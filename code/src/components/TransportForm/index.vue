<template>
    <div class="container">
        <div class="title">
            <h3 class="form-title"> Como é o frete que você precisa? </h3>
            <hr class="hr1">
        </div>

        <form action="">
            <div class="destiny">
                <h2>Destino</h2>
                <select v-model="selected">
                    <option value="" disabled></option>
                </select>
            </div>

            <div> 
                <h2>Peso</h2>
                <input>
            </div>

            <div class="button">
                <button class="submit-button" type="submit"> 
                    Analisar
                </button>
            </div>
        </form>

        <div class="bestoptions">
            <h3>Estas são as melhores alternativas de frete que encontramos para você</h3>
            <hr class="hr1">
            <div class="cheaper-shipping">
                <img :src="require('../../assets/saving.png')" class="saving"/>
                <h5>    
                    Frete mais barato:
                </h5>
            </div>
            <div class="faster-shipping">
                <img :src="require('../../assets/speedometer.png')" class="speedometer"/>
                <h5>
                    Frete mais rápido:
                </h5>
            </div>
        </div>
    </div>
</template>

<script>
    // import api from '../../services/api.ts'

    
    export default {
        name: 'TesteComponent',
        // citys: "city",
        // data() {
        //     return {
        //         citys: {},
        //         time: {},
        //         name: {}
        //     }
        // },

        // created() {
        //     this.getCity();
        //     console.log(this.getCity);
        // },

        // methods: {
        //     getCity() {
        //         api
        //         .get("/citys")
        //         .then((res) => {
        //             this.citys = res.data;
        //         })
        //         .catch(function(error) {
        //             console.log(error)
        //         })
        //     }
        // }
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
    margin-left: 3rem;
    align-items: left;
    justify-content: center;
    flex-direction: column;
    gap: 1rem;
    width: 25rem;
}

select, input {
    border: 0;
    background: #D4DEDF;

    border-radius: 0.5rem;

    width: 25rem;
    padding: 0.5rem 1rem;

    transition: filter 0.3s;
}

select:hover, input:hover {
    filter: contrast(0.8)
}

div .button {
    /* padding-left: 1rem;
    margin: 2rem 6rem; */
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
    margin-left: 3rem;
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

.saving, .speedometer {
    width:2rem;
}

</style>