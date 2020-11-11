<template>
  <main>
    <div class="container">
      <div class="d-flex mt-4">
        <div class="filter">
          <div class="options">
            <div class="title">Опции тарифа</div>
            <div class="option-list">
              <div class="d-flex list-item">
                <div class="checkbox">
                  <input type="checkbox" id="checkbox1"/>
                  <label for="checkbox1"></label>
                </div>
                <div>Только прямые</div>
              </div>
              <div class="d-flex list-item">
                <div class="checkbox">
                  <input type="checkbox" id="checkbox2"/>
                  <label for="checkbox2"></label>
                </div>
                <div>Только с багажом</div>
              </div>
              <div class="d-flex list-item">
                <div class="checkbox">
                  <input type="checkbox" id="checkbox3"/>
                  <label for="checkbox3"></label>
                </div>
                <div>Только возвратные</div>
              </div>
            </div>
          </div>
          
          <div class="aircompanies">
            <div class="title">Авиакомпании</div>
            <div class="aircompany-list">
              <div class="d-flex list-item">
                <div class="checkbox">
                  <input type="checkbox" id="checkbox"/>
                  <label for="checkbox"></label>
                </div>
                <div>Все</div>
              </div>
              <div class="d-flex list-item" v-for="airline of airlines" :key="airline.key">
                <div class="checkbox">
                  <input type="checkbox" :id="airline.id"/>
                  <label :for="airline.id"></label>
                </div>
                <div>{{ airline.name }}</div>
              </div>
            </div>
          </div>
          <div class="drop-filter">
            <a @click.prevent="click">Сбросить все фильтры</a>
          </div>
        </div>
        <div class="tickets">
          <div class="ticket flex-1" v-for="flight in flights" :key="flight.id">
            <div class="details-block">
              <div class="flex-center">
                <div class="logo flex-center-2">
                  <img :src="'https://aviata.kz/static/airline-logos/80x80/'+ flight.itineraries[0][0].carrier + '.png'" alt="aircompany">
                  <div class="logo-text">
                    {{ flight.itineraries[0][0].carrier_name}}
                  </div>
                </div>
                <div class="flex-center-2">
                  <div class="start-date">
                    <div class="date">25 ноя, вс</div>
                    <div class="time">23:25</div>
                  </div>
                  <div class="distance-detail">
                    <div class="detail-text">через Шымкент, 1ч 50м</div>
                  </div>
                  <div class="end-date">
                    <div class="date">26 ноя, вс</div>
                    <div class="time">23:25</div>
                  </div>
                </div>
              </div>
              <div class="actions d-flex align-items-center">
                <div class="more-info">
                  <a href="#">Детали перелета</a>
                  <a href="#">Условия тарифа</a>
                </div>
                <div class="no-return">
                  <a href="#">невозвратный</a>
                </div>
              </div>
            </div>
            <div class="price-block text-center">
              <div class="ticket-price">{{ flight.price }} т</div>
              <div class="select-ticket text-center">
                <button>Выбрать</button>
              </div>
              <div class="price-text">Цена за всех пассажиров</div>
              <div class="flex-center-2">
                <div class="no-baggage">Нет багажа</div>
                <div class="add-baggage">
                  <button>+ Добавить багаж</button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import {AirlineData} from '@/components/data/airlineData';

@Component
export default class AirTickets extends Vue {
  public data = require('../results.json');
  private flights = this.data.flights;
  
  get airlines() {
    let airlines = this.data.airlines;
    let modifiedAirlines: AirlineData[] = [];
    for (let key in airlines) {
      if(airlines.hasOwnProperty(key)){
        const airline: AirlineData = {
          key: key,
          name: airlines[key],
        }
        modifiedAirlines.push(airline);
      }
    }
    return modifiedAirlines;
  }
  
  public click() {
    console.log(this.data);
  }
  
}
</script>

<style lang="scss">
</style>
