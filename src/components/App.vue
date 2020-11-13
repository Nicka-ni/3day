<template>
    <div ><center>
        <h3>Населенний пункт</h3>
        <select  v-model = "comboBox" v-on:click = "Sort()" @change = "Filter()">
            <option v-for="item in ChMail"  v-bind:key="item.Ref">{{ item.SettlementAreaDescription }}</option>
        </select>
    <div>
        <br>
        <h3>Поштове відділення </h3>
        <select v-if = "comboBox != false">
            <option v-for="item in FilterMail" v-bind:key="item.Ref">{{ item.Description }} -- {{ item.CityDescription }}</option>
        </select>
    </div>
</center>
    </div>

    
</template>

<script>
    import Vue from 'vue'
    import axios from 'axios'
    import VueAxios from 'vue-axios'
 
    export default {
       data: function() {
           return {
                AllMail:[],
                ChMail:[],
                comboBox:false,
                FilterMail:[]
           };
        },
        mounted: function(){
             axios.post("https://api.novaposhta.ua/v2.0/json/",{
                    "modelName": "AddressGeneral",
                    "calledMethod": "getWarehouses",
                    "methodProperties": {
                        "CityName": ""
                        },
                    "apiKey": "9a557481f95094531372a9d1b55222c8"
                }).then((response) =>{
                    console.log(response.data);
                    this.AllMail = response.data.data;
                })
        },
        methods: {
             Sort: function(){
            var _ = require('lodash');
            this.ChMail = _.uniqBy(this.AllMail, 'SettlementAreaDescription'); 
            },
            Filter: function(){
                let search = this.comboBox
                this.FilterMail = _.filter(this.AllMail, function(id) { 
                if(id.SettlementAreaDescription == search){
                        if(id.SettlementTypeDescription == "місто"){
                            return id
                        }
                    } 
                });
                console.log(this.FilterMail);
            },
        }
    }
</script>

