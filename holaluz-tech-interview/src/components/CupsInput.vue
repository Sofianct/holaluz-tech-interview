<script>
import { ref } from 'vue'

const url = import.meta.env.VITE_BASE_URL
const clientUrl = import.meta.env.VITE_CLIENT_ENDPOINT
const supplyUrl = import.meta.env.VITE_SUPPLY_ENDPOINT

export default {
    name: "CupsInput",
    setup() {
        const clientData = ref('')
        const clientSupplyData = ref('')
        const input = ref('')
        const neighbor1 = ref('')
        const neighbor2 = ref('')
        const promo = ref('')
        const formError = ref('')

        const getClientData = async (cups) => {
            clientData.value = ''
            clientSupplyData.value = ''
            neighbor1.value = ''
            neighbor2.value = ''
            promo.value = ''
            if (cups.toString().length === 6) {
                formError.value = ''
                try {
                    const req = await fetch(url + clientUrl + cups)
                    const res = await req.json()
                    clientData.value = res[0]
                } catch (error) {
                    console.log(error)
                }
                try {
                    const req = await fetch(url + supplyUrl + cups)
                    const res = await req.json()
                    clientSupplyData.value = res[0]
                    if (res[0].neighbors.length > 0 && clientData.value['building_type'] === 'house') {
                        getPromo(clientSupplyData, res[0].neighbors)
                    } else {
                        promo.value = 'No access to promo'
                    }
                } catch (error) {
                    console.log(error)
                }

            }
            
        }

        const getNeighborPower = async (neighbors) => {
            try {
                const req = await fetch(url + supplyUrl + neighbors[0])
                const res = await req.json()
                neighbor1.value = res[0]
            } catch (error) {
                console.log(error)
            }
            if (neighbors.length === 2) {
                try {
                    const req = await fetch(url + supplyUrl + neighbors[1])
                    const res = await req.json()
                    neighbor2.value = res[0]
                } catch (error) {
                    console.log(error)
                }
            }
        }

        const sum = (obj) => {
            return Object.keys(obj).reduce((sum,key)=>sum+parseFloat(obj[key]||0),0);
        }

        const getPromo = async(clientSupply, neighbors) => {
            await getNeighborPower(neighbors)

            const clientNeighbors = clientSupply.value['neighbors']
            const clientPower = sum(clientSupply.value['power'])

            
            const n1Power = sum(neighbor1.value['power'])

            const n2Power = sum(neighbor2.value['power'])

            const n1Invoiced = parseInt(neighbor1.value['invoiced_amount'])
            const n2Invoiced = parseInt(neighbor2.value['invoiced_amount'])
            
            const neighborsInvoiced = (n1Invoiced + n2Invoiced)
            
            //--------------

            if (clientNeighbors.length === 1) {
                if (clientPower > n1Power) {
                    promo.value = "5%"
                }
                if (clientPower > n1Power && neighborsInvoiced > 100) {    
                    promo.value = "12%"
                } else {
                    promo.value = 'No discount'
                }
            }

            else {
                if (clientPower > n1Power || clientPower > n2Power) {
                    promo.value = "5%"
                    if (n1Invoiced > 100) {    
                        promo.value = "12%"
                    }
                }else {
                    promo.value = 'No discount'
                }
            }

        }

        const checkForm = (e) => {
            e.preventDefault();
            
            if (input.value === '') {
                formError.value = 'Este campo es requerido'
            } else if(input.toString().length > 6){
                formError.value = 'CUPS incorrecto'
            }
                       
            getClientData(input.value)
        }
        return {clientData, getClientData, checkForm, clientSupplyData, input, promo, neighbor1, neighbor2, formError}
    }
}

</script>

<template>
    <div class="main__container">
        <form @submit="checkForm" class="form__container">
            <article class="form__article">
                <label for="number">CUPS</label>
                <input placeholder="Example: 123456" type="number" for="number" v-model="input"/>
                <div class="errorMsg">
                    {{ formError }}
                </div>
            </article>
            <button type="submit">Descubre tu tarifa</button>
        </form>
      <div v-if="promo !== ''" class="client__container">
        <p>{{clientData.full_name}}</p>
        <p>Direccion: {{clientData.address}}</p>
        <p>CUPS: {{clientData.cups}}</p>
        <div class="promo__container">
            <p v-if="promo !== 'No access to promo' && promo !== 'No discount'">Descuento: {{promo}}</p>
            <p v-if="promo === 'No access to promo'">¡Vaya! No te hemos podido calcular una cuota</p>
            <p v-if="promo === 'No discount'"></p>
        </div>
        <button v-if="promo !== 'No access to promo'">Contratar</button>
        
      </div>
    </div>
    
</template>

<style scoped>
.main__container{ 
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    align-items: center;
    margin: 5rem 0 0;
    height: 40vh;
}
.form__article{
    display:flex;
    flex-direction: column;
    justify-content: flex-start;
    width: 20rem;
    margin: 0 0 0.2rem;
}
.form__container{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}
.form__container button{
    width: 13rem;
    height: 2rem;
    border-radius: 2rem;
    border: 0;
    background-color: #65717e;
    color: rgb(255, 235, 235);
    font-weight: 600;
    margin: 1.2rem 0 2rem
}

.form__container input{
    border-radius: 0.2rem;
    padding: 0 0.3rem;
    height: 2rem; 
}
.errorMsg{
    color: rgb(255, 60, 60);
    font-weight: 500;
    font-size: 0.85rem;
}
.client__container{ 
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 2rem;
    font-size: 1.1rem;
    box-shadow: -1px 3px 10px  rgba(61,61,61,0.2);
    background-color: white;
    border-radius: 0.6rem;
    line-height: 2rem;
    font-weight: 300;
}
button{ 
    margin: 0 auto;
    background-color:#e5007e;
    color: white;
    width: 8rem;
    height: 2rem;
    font-weight: 700;
    border-radius: 2rem;
    font-size: 1rem;
    border: #e5007e 1px solid;
    margin: 1rem 0 0 0;
}
.promo__container{
    font-weight: 600;
}

</style>