<template>
    <div class="container">
        <div class="calendar" :class="isCalendarTheme ? 'dark' : 'light'">
            <div class="calendar__header grid">
                <div class="calendar__title">
                    <p class="title">
                        <span class="d-block">{{currentDate.year}}</span>
                        <span class="d-block">{{currentDate.monthName}}</span>
                    </p>
                    <p>{{currentDate.date}}</p>
                </div>
                <div class="calendar__buttons">
                    <div @click.prevent="changeTheme()" class="change-theme">
                        <span class="ico fas fa-moon"
                        :class="isThemeBtn ? 'ico-light' : 'ico-dark'"
                        ></span>
                    </div>
                    <button @click="previous()" class="btn btn--previous"><span class="fas fa-chevron-left"></span></button>
                    <button @click="next()" class="btn btn--next"><span class="fas fa-chevron-right"></span></button>
                </div>
            </div>
            <div class="calendar__body">
                <div class="calendar__week grid">
                    <div class="week" v-for="(weekDay, index) in weekDays" :key="index">
                        {{weekDay}}
                    </div>
                </div>
                <div class="calendar__days grid">
                    <div class="days" 
                        v-for="(day, index) in currentDate.daysInMonth" :key="index" @click="eventDay(day)">
                        {{day}}
                    </div>
                </div>
                <div class="create-event">
                    <button @click="newEvent()" class="btn btn--plus"><span class="fas fa-plus"></span></button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Calendar',
    data(){
        return{
            counter: null,
            currentDate: {
                dateObj: new Date(),
                date: 0,
                days: 31,
                daysInMonth: [],
                startingDay: 0,
                lastDay: 0,
                monthName: '',
                currentMonth: 0,
                year: 0
            },
            prevDate: {
                lastDay: 0,
            },
            dateSelected: {},
            monthArr: [
                'Enero',
                'Febrero',
                'Marzo',
                'Abril',
                'Mayo',
                'Junio',
                'Julio',
                'Agosto',
                'Septiembre',
                'Octubre',
                'Noviembre',
                'Diciembre'
            ],
            weekDays: [
                'Lun',
                'Mar',
                'Mié',
                'Jue',
                'Vie',
                'Sa',
                'Do'
            ],
            isEvent: false,
            isCalendarTheme: false,
            isThemeBtn: false
            
        }
    },
    methods:{
        setCalendar(){
            this.currentDate.date = this.currentDate.dateObj.toDateString()
            this.currentDate.monthName = this.monthArr[this.currentDate.dateObj.getMonth()]
            this.currentDate.year = this.currentDate.dateObj.getFullYear()
            this.currentDate.lastDay = new Date(this.currentDate.dateObj.getFullYear(), 
            this.currentDate.dateObj.getMonth() + 1, 0).getDate()
            this.currentDate.dateObj.setDate(1)
            this.currentDate.startingDay = this.currentDate.dateObj.getDay()

            this.prevDate.lastDay = new Date(this.currentDate.dateObj.getFullYear(), 
            this.currentDate.dateObj.getMonth(), 0).getDate()


            const range = (start, stop, step) => Array.from({ length: (stop - start) / step + 1}, (_, i) => start + (i * step));
            this.currentDate.daysInMonth = range(1, this.currentDate.lastDay, 1);

           this.currentDate.daysInMonth.map((item, index) => {
               if(index < this.currentDate.startingDay - 1){
                   this.currentDate.daysInMonth.unshift(this.prevDate.lastDay--)
               }
           })
        },
        
        previous(){
            this.currentDate.dateObj.setMonth(this.currentDate.dateObj.getMonth() - 1)
            return this.setCalendar()
        },
        next(){
            this.currentDate.dateObj.setMonth(this.currentDate.dateObj.getMonth() + 1)
            return this.setCalendar()
        },
        newEvent(){
            return this.$router.push({path: '/'})
        },
        eventDay(day){
            this.dateSelected = new Date(this.currentDate.year, this.currentDate.dateObj.getMonth(), day)
            return this.$router.push({name: '/EventCalendar', params: {date: this.dateSelected}})
        },
        changeTheme(){
            return (this.isCalendarTheme = !this.isCalendarTheme, this.isThemeBtn = !this.isThemeBtn)
        }
    },
    created(){
       this.setCalendar()
    }
}
</script>

<style lang="scss" scoped>
.grid{
    display: grid;
    grid-template-columns: repeat(7, 1fr);
    gap: 5px;
    align-items: center;
}
.calendar{
    margin-top: 50px;
    padding: 15px;
    border-radius: 3px;
    &.light{
        background-color: #ffffff;
        box-shadow: 2px 2px 4px #b8b7b7;
        border: 1px solid #e2e2e2;
        color: #192533;
        transition: .2s ease-in-out all;
        .calendar__week{
            border-bottom: .5px solid #d3d3d3;
            background-color: #f5f5f5;
            margin-bottom: 5px;
        }
        .days:hover{
            background-color: #f7f7f7;
        }
        .btn--previous, .btn--next{
            background-color: #f5f5f5;
            border: 1px solid #d3d3d3;
            color: #242f3c;
        }
        .btn--plus{
            box-shadow: 2px 2px 4px #ababab;
            color: #242f3c;
        }
    }
    &.dark{
        background-color: #192533;
        box-shadow: 2px 2px 4px #b8b7b7;
        border: 1px solid #e2e2e2;
        color: #e2e2e2;
        transition: .2s ease-in-out all;
        .calendar__week{
            border-bottom: .5px solid #6d6d6d;
            background-color: #0e161f;
        }
        .days:hover{
            background-color: #121e2b;
        }
        .btn--previous, .btn--next{
            background-color: #0e161f;
            border: 1px solid #242f3c;
            color: white;
        }
        .btn--plus{
            box-shadow: 2px 2px 4px #030d17;
            color: #e2e2e2;
        }
    }
}
.calendar__header{
    padding: 0 10px;
    .calendar__title{
        grid-column: span 5;
        .title{
            font-size: 24px;
            font-weight: bold;
        }
        .d-block{
            display: block;
        }
    }
    .calendar__buttons{
        grid-column: span 2;
        justify-self: flex-end;
    }
}
.btn{
    margin-left: 15px;
    cursor: pointer;
    padding: 8px;
    border-radius: 3px;
    font-size: 16px;
    min-width: 35px;
    min-height: 35px;
    text-align: center;
}
.calendar__body{
    .week, .days{
        padding: 20px 10px;
    }
    .days{
        cursor: pointer;
    }
}
.create-event{
    text-align: right;
    .btn--plus{
        background-color: #1497F6;
        min-height: 45px;
        min-width: 45px;
        border: none;
        border-radius: 100%;
    }
}
.change-theme{
    width: 55px;
    background-color: #ffffff;
    border: 1px solid #d3d3d3;
    border-radius: 15px;
    overflow: hidden;
    margin-left: auto;
    margin-bottom: 25px;
    cursor: pointer;
    .ico {
        font-size: 16px;
        width: 25px;
        height: 25px;
        display: flex;
        align-items: center;
        justify-content: center;
        padding: 6px;
        border-radius: 50%;
        transition: all .2s ease-in-out;
        &.ico-dark{
            color: #e2e2e2;
            background-color: #242f3c;
            border: 1px solid #d3d3d3;
            transform: translateX(0);
        }
        &.ico-light{
            color: #242f3c;
            background-color: #f5f5f5;
            border: 1px solid #d3d3d3;
            transform: translateX(30px);
        }
    }
}
    
</style>