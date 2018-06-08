<template>

    <!--<v-layout justify-center align-center>-->
    <v-layout>
        <v-flex xs12>

            <v-layout>
                <v-flex xs6 class="pa-2">
                    <webview v-for="(i, index) in maps" v-show="index === currentMap" ref="ya" :src="i" class="webwiew" :preload="preya"></webview>
                </v-flex>
                <v-flex xs6 class="pa-2">
                    <v-layout class="mb-2">
                        <v-flex>
                            <v-card :color="balanceJelastick < 1000 ? 'red darken-4' : 'blue-grey darken-2'" class="white--text">
                                <div class="pa-2">
                                    <h1>Баланc Jelastic: {{ formatMoney(balanceJelastick) }} руб</h1>
                                </div>
                            </v-card>
                        </v-flex>
                    </v-layout>

                    <v-layout class="mb-2">
                        <v-flex>
                            <v-data-table
                                    :items="users"
                                    hide-actions
                                    hide-headers
                                    class="elevation-1"
                            >
                                <template slot="items" slot-scope="props">
                                    <td>{{ props.item.name }}</td>
                                    <td class="text-xs-right">{{ props.item.count }}</td>
                                    <td class="text-xs-right">{{ formatDate(props.item.createdAt) }}</td>
                                    <td class="text-xs-right">{{ props.item.email }}</td>
                                </template>
                            </v-data-table>
                        </v-flex>
                    </v-layout>

                    <v-layout class="mb-2">
                        <v-flex>
                            <v-data-table
                                    :items="modules"
                                    hide-actions
                                    hide-headers
                                    class="elevation-1"
                            >
                                <template slot="items" slot-scope="props">
                                    <td>{{ props.item.name }}</td>
                                    <td class="text-xs-right">{{ props.item.release.version }}</td>
                                    <td class="text-xs-right">{{ props.item.installCount }}</td>
                                </template>
                            </v-data-table>
                        </v-flex>
                    </v-layout>

                </v-flex>
            </v-layout>











            <!--<v-layout>-->
                <!--<v-flex xs6>-->
                    <!--<v-card color="blue-grey darken-2" class="white&#45;&#45;text">-->
                        <!--<v-card-title primary-title>-->
                            <!--<div class="headline">Unlimited music now</div>-->
                            <!--<div>Listen to your favorite artists and albums whenever and wherever, online and offline.</div>-->
                        <!--</v-card-title>-->
                        <!--<v-card-actions>-->
                            <!--<v-btn flat dark>Listen now</v-btn>-->
                        <!--</v-card-actions>-->
                    <!--</v-card>-->
                <!--</v-flex>-->
            <!--</v-layout>-->

            <!--<v-layout>-->
                <!--<v-flex xs6>-->
                    <!--<v-card color="blue-grey darken-2" class="white&#45;&#45;text">-->
                        <!--<v-card-title primary-title>-->
                            <!--<div class="headline">Unlimited music now</div>-->
                            <!--<div>Listen to your favorite artists and albums whenever and wherever, online and offline.</div>-->
                        <!--</v-card-title>-->
                        <!--<v-card-actions>-->
                            <!--<v-btn flat dark>Listen now</v-btn>-->
                        <!--</v-card-actions>-->
                    <!--</v-card>-->
                <!--</v-flex>-->
            <!--</v-layout>-->




        </v-flex>

    </v-layout>



</template>

<script>
    import axios from 'axios'

    export default {
        data(){
            return {
                preya: "file:///Users/evgenijspilka/Documents/dev/desctop/dashboard/static/ya-preloder.js",
                currentMap: 0,
                maps: [
                    "https://yandex.ru/pogoda/moscow/maps/nowcast?lat=55.572654&lon=37.563481&ll=37.609870_55.633148&z=10&le_Lightning=1",
                    "https://yandex.ru/pogoda/moscow/maps/temperature?lat=55.572654&lon=37.563481&ll=37.609870_55.633148&z=10&le_TemperatureBalloons=1&le_WindParticles=1",
                    "https://yandex.ru/pogoda/moscow/maps/wind?lat=55.572654&lon=37.563481&ll=37.609870_55.633148&z=10&le_WindParticles=1",
                    "https://yandex.ru/pogoda/moscow/maps/pressure?lat=55.572654&lon=37.563481&ll=37.609870_55.633148&z=10&le_WindParticles=1"
                ],
                balanceJelastick: 0,
                users: [],
                modules: []
            }
        },
        created(){
            this.getBalanceJelastic();
            this.getUsers();
            this.getModules();
        },
        mounted(){

            setInterval(() => {
                if(this.currentMap === 3){
                    this.currentMap = 0
                }
                else{
                    this.currentMap++
                }
            }, 60000)

            setInterval(() => {
                this.getBalanceJelastic();
                this.getUsers();
                this.getModules();
            }, 60 * 60 * 1000)
        },
        methods: {
            formatMoney(num){
                var p = num.toFixed(2).split(".");
                return p[0].split("").reverse().reduce(function(acc, num, i, orig) {
                    return  num=="-" ? acc : num + (i && !(i % 3) ? " " : "") + acc;
                }, "") + "." + p[1];
            },
            formatDate(date){
                let time = new Date(date);
                return time.toLocaleString();

            },
            getBalanceJelastic(){
                axios.get("https://api.upoint.online/public/monitor/balance").then((res) => {
                    this.balanceJelastick = res.data.balance;
                })
            },
            getUsers(){
                axios.get("https://api.upoint.online/public/monitor/users").then((res) => {
                    console.log(res.data);
                    this.users = res.data;
                })
            },
            getModules(){
                axios.get("https://api.upoint.online/public/monitor/modules").then((res) => {
                    console.log(res.data);
                    this.modules = res.data;
                })
            }
        }
    }
</script>

<style>
    @keyframes hidden {
        from {opacity: 0}
        50% {opacity: 0.5}
        to {opacity: 1}
    }
.webwiew{
    height: 650px;
    animation: hidden linear 1.5s ;
    animation-fill-mode: forwards;

}
</style>