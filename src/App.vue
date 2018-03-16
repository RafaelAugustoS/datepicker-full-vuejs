<template>
    <div id="app">
        <div class="calendar">
            <div class="onDay">
                <h1>
                    {{diaSelected}}
                    <span>{{tratarDia(diaSelected)}}</span>
                </h1>

                <p v-if="DayNot(diaSelected)">Faltam <b>{{diasFalta[diasFalta.map(item => item.Dia).indexOf(diaSelected)].Hours}}</b> horas a serem cadastradas</p>
            </div>

            <div class="calendarItem">
                <header>
                    <div class="year">
                        <img src="./assets/play-arrow-left.png" @click="changeYear(anoSelected, 'previous')">
                        <span>{{anoSelected}}</span>
                        <img src="./assets/play-arrow.png" @click="changeYear(anoSelected, 'next')">
                    </div>
                    <ul>
                        <li v-for="(item, index) in 12" :class="{'selectedMonth': index == mesSelected}" @click="changeMonth(index)">{{tratarData(index)}}</li>
                    </ul>
                </header>

                <article>
                    <ul>
                        <li v-for="item in diasSemana">{{item}}</li>
                    </ul>    
                </article>

                <section>
                    <ul>
                        <li v-for="(item, index) in diasMes" :class="{'item': DayNot(item)}" >
                            <div :class="{'selected': item == diaSelected}" @click="changeDay(item)">
                                <span>{{item}}</span>
                                
                            </div>    
                        </li>
                    </ul>
                </section>
            </div>
        </div>  
    </div>
</template>

<script>

export default {
    name: 'App',
    data(){
        return{
            diasSemana: ['DOM', 'SEG', 'TER', 'QUA', 'QUI', 'SEX', 'SAB'],
            diasMes: [],
            diasFalta: [
                {Dia: 1, Hours: 8},
                {Dia: 2, Hours: 6.5},
                {Dia: 15, Hours: 7}
            ],
            mesSelected: new Date().getMonth(),
            anoSelected: new Date().getFullYear(),
            diaSelected: new Date().getDate(),
            diaAtual: new Date().toLocaleString()
        }
    },
    created(){
        this.getDaysInMonth(this.mesSelected, this.anoSelected)
        this.init()
    },
    computed: {
           
    },
    methods: {
        getDaysInMonth(month, year){
            let date = new Date(year, month, 1)
            this.diasMes = []

            while(date.getMonth() === month){
                this.diasMes.push(date.getDate())
                date.setDate(date.getDate() + 1)
            }
        },
        tratarData(mes){
            let month = []

            month[0] = 'Jan'
            month[1] = 'Fev'
            month[2] = 'Mar'
            month[3] = 'Abr'
            month[4] = 'Mai'
            month[5] = 'Jun'
            month[6] = 'Jul'
            month[7] = 'Ago'
            month[8] = 'Set'
            month[9] = 'Out'
            month[10] = 'Nov'
            month[11] = 'Dez'

            return month[mes]
        },
        tratarDia(day){
            let dia = []
            let data = new Date(this.anoSelected, this.mesSelected, day)

            dia[0] = 'Domingo'
            dia[1] = 'Segunda-Feira'
            dia[2] = 'Terça-Feira'
            dia[3] = 'Quarta-Feira'
            dia[4] = 'Quinta-Feira'
            dia[5] = 'Sexta-Feira'
            dia[6] = 'Sabado'

            return dia[data.getDay()]
        },
        changeDay(item){
            this.diaSelected = item
        },
        changeMonth(mes){
            this.mesSelected = mes
            this.getDaysInMonth(this.mesSelected, this.anoSelected)
            this.init(this.anoSelected, false)
        },
        changeYear(year, type){
            if(type == 'previous'){
                this.anoSelected = this.anoSelected - 1
            }else{
                this.anoSelected = this.anoSelected + 1
            }

            this.getDaysInMonth(this.mesSelected, this.anoSelected)
            this.init(this.anoSelected, false)
        },
        DayNot(dia){
            let retorno = this.diasFalta.map(item => {
                return item.Dia
            }).indexOf(dia)

            if(retorno != -1){
                return true
            }

            return false
        },
        init(ano = this.anoSelected, initTrue = true){
            let data = new Date(ano, this.mesSelected, 1)

            
            for(let i = 0; i < data.getDay(); i++){
                this.diasMes.unshift('')
            }

            this.diasMes.map(item => {
                return item
            })
        }
    }
}
</script>

<style>
    *, *:before, *:after{
        margin: 0;
        padding: 0;
        box-sizing: border-box;
        font-family: 'Roboto', sans-serif;
    }

    html, body, #app{
        width: 100%;
        height: 100%;
    }
    
    body{
        background: #F5F5F5;
    }

    .calendar{
        width: 600px;
        height: 350px;
        border-radius: 20px;
        float: left;
        box-shadow: 2px 2px 2px #E1E1E1;
        margin-top: 50px;
        margin-left: 50px;
    }

    .onDay{
        float: left;
        width: 35%;
        height: 100%;
        background: #EC364F;
        border-top-left-radius: 20px;
        border-bottom-left-radius: 20px;
    }

    .onDay h1{
        color: #FFF;
        text-align: center;
        font-size: 7em;
        margin-bottom: 0;
        padding-bottom: 0;
        margin-top: 50px;
    }

    .onDay h1 span{
        display: block;
        font-size: 22px;
        font-weight: 400;
        margin-top: 0;
    }

    .onDay p{
        color: #FFF;
        font-weight: 300;
        text-align: center;
        font-size: 15px;
    }

    .calendarItem{
        float: left;
        width: 65%;
        padding: 20px;
        background: white;
        height: 100%;
        border-top-right-radius: 20px;
        border-bottom-right-radius: 20px;
    }

    .calendarItem header{
        float: left;
        width: 100%;
        border-bottom: 1px solid #E1E1E1;
    }

    .calendarItem header .year{
        color: #CCC;
        font-weight: 500;
        font-size: 20px;
        width: 100%;
        float: left;
        text-align: right;
        margin-bottom: 15px;
        transition: 0.5s;
    }


    .calendarItem header .year img{
        width: 13px;
        cursor: pointer;
    }

    .calendarItem header ul{
        margin: 0;
        padding: 0;
        padding-bottom: 7px;
        float: left;
        width: 100%;
    }

    .calendarItem header ul li{
        list-style: none;
        float: left;
        font-size: 11px;
        color: #888;
        width: calc(100% / 12);
        text-align: center;
        font-weight: 400;
        cursor: pointer;
        transition: 0.5s;
    }

    .calendarItem header ul li:hover{
        color: black;
    }

    .calendarItem article{
        width: 100%;
        float: left;
    }

    .calendarItem article ul{
        list-style: none;
        width: 100%;
        float: left;
        margin-top: 5px;
    }

    .calendarItem article ul li{
        float: left;
        width: calc(100% / 7);
        text-align: center;
        color: #555;
        font-size: 15px;
        font-weight: 500;
    }

    .calendarItem section ul{
        list-style: none;
        margin-top: 30px;
        float: left;
        width: 100%;
    }

    .calendarItem section ul li{
        float: left;
        width: calc(100% / 7);
        text-align: center;
        font-size: 14px;
        height: 35px;
        color: #666;
        position: relative;
        line-height: 1.8;
        cursor: pointer;
        transition: 0.5s;
    }

    .calendarItem section ul li:hover{
        color: red;
        font-weight: 500;
    }

    .item:after{
        content: '';
        width: 4px;
        height: 4px;
        background: red;
        display: block;
        border-radius: 50%;
        position: absolute;
        top: 0;
        right: 10px;
    }

    .selected{
        margin: 0 auto;
        width: 25px;
        height: 25px;
        border-radius: 50%;
        background: #EC364F;
        color: #FFF;
        font-weight: 500;
    }

    .selectedMonth{
        font-weight: 600 !important;
        color: black !important;
    }
</style>