<template>
    <v-container>
        <v-row justify="start" align="center">
            <v-col class="col-md-8 offset-md-2 col-sm-10 offset-sm-1" >
                <v-breadcrumbs :items="items" class="bread"></v-breadcrumbs>
                <v-btn color="primary" class="white--text mb-8" to="/bilar"><v-icon dark left>{{ mdiArrowLeft }}</v-icon>Tillbaka</v-btn>
                <v-img :src="'/img/larger/' + model + '.webp'" contain></v-img>
                <div class="my-4 d-flex flex-row align-center">
                    <h1 >{{brand}} {{ model }}</h1>
                    <v-btn color="primary" class="ml-auto white--text" :href="specs.link" target="_blank">Till {{brand}}.se<v-icon dark right>{{ mdiLaunch }}</v-icon></v-btn>
                </div>
                <v-divider></v-divider>
                <h2 class="mt-4">Specifikationer</h2>
                <v-simple-table>
                    <template v-slot:default>
                    <thead>
                        <tr>
                        <th class="text-left">Namn</th>
                        <th class="text-left">Värde</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="item in specsItems" :key="item.name">
                        <td>{{ item.name }}</td>
                        <td>{{ item.value }}</td>
                        </tr>
                    </tbody>
                    </template>
                </v-simple-table>
            </v-col>

        </v-row>
    </v-container>
</template>

<script>
import { api } from '@/Api.js'
import { mdiArrowLeft } from '@mdi/js'
import { mdiLaunch } from '@mdi/js'

export default {
    data: () => ({
        mdiArrowLeft,
        mdiLaunch,
        model: '',
        brand: '',
        specs: {},
        items: [
            {
                text: 'Hem',
                disabled: false,
                to: '/'
            },
            {
                text: 'Alla bilar',
                disabled: false,
                to: '/bilar'
            },
            {
                text: 'Link 1',
                disabled: true
            },
        ],
        specsItems: [
            {
                name: 'Märke',
                value: ''
            },
            {
                name: 'Modell',
                value: ''
            },
            {
                name: 'Variant',
                value: ''
            },
            {
                name: 'Kaross',
                value: ''
            },
            {
                name: 'Max Effekt (kW)',
                value: ''
            },
            {
                name: 'Topphastighet (km/h)',
                value: ''
            },
            {
                name: 'Acceleration (0-100 km/h)',
                value: ''
            },
            {
                name: 'Drivmedel',
                value: ''
            },
            {
                name: 'Bränsleförbrukning, blandad körning (l/100km)',
                value: ''
            },
            {
                name: 'Utsläpp CO2 (g/km)',
                value: ''
            },
            {
                name: 'Antal säten',
                value: ''
            },
            {
                name: 'Lastutrymme (l)',
                value: ''
            },

        ]
    }),
    metaInfo () {
      return {
        title: this.brand + ' ' + this.model,
        meta: [
          {vmid: 'description', name: 'description', content: 'Hitta allt du behöver veta om ' + this.model + '. Specifikation, omdömen och guider. Vi samlar allt på samma ställe' }
        ],
        link: [
            { rel: 'canonical', href: 'bil/' + this.model}
        ]
      }
    },
    created() {
        this.model = this.$route.params.model
        this.getModelData()
    },
    methods: {
        getModelData() {
            api.get(`/models/${this.model}`)
                .then(response => {
                    this.brand = response.data.brand
                    this.setBreadCrumbs()
                    this.getSpecs()
                })
                .catch(error => {
                    console.log(error)
                })
        },
        getSpecs(){
            api.get(`/specs/${this.model}`)
                .then(response => {
                    this.specs = response.data
                    this.setSpecItems()
                }).catch(error => {
                    console.log(error)
                })
        },
        setBreadCrumbs() {
            this.items[2].text = this.brand + ' ' + this.model
        },
        setSpecItems() {
            this.specsItems[0].value = this.specs.brand
            this.specsItems[1].value = this.specs.model
            this.specsItems[2].value = this.specs.variant
            this.specsItems[3].value = this.specs.body_type
            this.specsItems[4].value = this.specs.max_power_kW
            this.specsItems[5].value = this.specs.top_speed_kmh
            this.specsItems[6].value = this.specs.acceleration_0_100 + ' s'
            this.specsItems[7].value = this.specs.fuel_type
            this.specsItems[8].value = this.specs.fuel_consumption_combined
            this.specsItems[9].value = this.specs.co2_emissions_combined
            this.specsItems[10].value = this.specs.seats
            this.specsItems[11].value = this.specs.luggage_capacity
        }
    }

}
</script>
<style scoped>
.bread {
    padding-left: 4px;
    padding-top: 0;
}
</style>
