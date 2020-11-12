<template>
  <main>
    <div class="container">
      <div class="d-flex mt-4">
        <div class="filter">
          <div class="options">
            <div class="title">Опции тарифа</div>
            <div class="option-list">
              <div class="list-item">
                <label class="d-flex checkbox-block">
                  <input type="checkbox" id="checkbox-1" value="11" v-model="types"/>
                  <span></span>
                  Только прямые
                </label>
              </div>
              <div class="list-item">
                <label class="d-flex checkbox-block">
                  <input type="checkbox" id="checkbox-2" value="12"  v-model="types"/>
                  <span></span>
                  Только с багажом
                </label>
              </div>
              <div class="list-item">
                <label class="d-flex checkbox-block">
                  <input type="checkbox" id="checkbox-3" value="13"  v-model="types"/>
                  <span></span>
                  Только возвратные
                </label>
              </div>
            </div>
          </div>
          
          <div class="aircompanies">
            <div class="title">Авиакомпании</div>
            <div class="aircompany-list">
              <div class="list-item">
                <label class="d-flex checkbox-block">
                  <input type="checkbox" id="checkbox" v-model="showAll" @change="selectAll"/>
                  <span></span>
                  Все
                </label>
              </div>
              <div class="list-item" v-for="(airline, index) of airlines" :key="airline.key">
                <label class="d-flex checkbox-block">
                  <input type="checkbox" :id="'checkbox' + index" :value="airline.key" v-model="filterAirlines"/>
                  <span></span>
                  {{ airline.name }}
                </label>
              </div>
            </div>
          </div>
          <div class="drop-filter">
            <a @click.prevent="clearAllFilters">Сбросить все фильтры</a>
          </div>
        </div>
        <div class="tickets">
          <div class="ticket flex-1" v-for="flight in filteredFlights" :key="flight.id">
            <div class="details-block">
              <div class="flex-center">
                <div class="logo flex-center-2">
                  <img :src="'https://aviata.kz/static/airline-logos/80x80/'+ flight.carrier + '.png'" alt="aircompany">
                  <div class="logo-text">
                    {{ flight.carrierName}}
                  </div>
                </div>
                <div class="flex-center-2">
                  <div class="start-date">
                    <div class="date">{{ flight.depDate }} ноя, вс</div>
                    <div class="time">{{ flight.depTime }}</div>
                  </div>
                  <div class="distance-detail">
                    <div class="detail-text">через Шымкент, 1ч 50м</div>
                  </div>
                  <div class="end-date">
                    <div class="date">{{ flight.arrDate }} ноя, вс</div>
                    <div class="time">{{ flight.arrTime }}</div>
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
    <!-- not correctly work  -->
    <div class="overlay">
      <div v-show="loader" class="loader"></div>
    </div>
  </main>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import {AirlineData} from '@/components/data/airlineData';

@Component
export default class AirTickets extends Vue {
  public data = require('../results.json');
  public types: any[] = [];
  public filterAirlines: any = [];
  private flights = this.data.flights;
  private showAll: boolean = true;
  private loader: boolean = false;

  public clearAllFilters() {
    this.showAll = false;
    this.types = [];
    this.filterAirlines = [];
  }

  public selectAll() {
    this.loader = true;
    this.filterAirlines = [];
    if (this.showAll) {
      this.airlines.forEach((air: any) => {
        this.filterAirlines.push(air.key);
        this.loader = false;
      });
    } else {
      this.showAll = !this.showAll;
      this.loader = false;
    }
  }

  get airlines() {
    const airlines = this.data.airlines;
    const modifiedAirlines: AirlineData[] = [];
    for (const key in airlines) {
      if (airlines.hasOwnProperty(key)) {
        const airline: AirlineData = {
          key,
          name: airlines[key],
        };
        modifiedAirlines.push(airline);
      }
    }
    return modifiedAirlines;
  }

  get filteredFlights() {
    this.loader = true;
    const filteredFlights: any = [];
    this.modifiedFlights.forEach((item: any) => {
      this.modifiedFlights.forEach((filter: any, index: number) => {
        if (this.modifiedFlights.hasOwnProperty(index)) {
          if (item.carrier === this.filterAirlines[index]) {
            filteredFlights.push(item);
          }
        }
      });
    });
    this.loader = false;
    return filteredFlights;
  }

  get modifiedFlights() {
    const modifiedFlights: any = [];
    this.flights.forEach((flight: any) => {
      const flightData = {
        id: flight.id,
        price: flight.price,
        carrierName: flight.itineraries[0][0].carrier_name,
        carrier: flight.itineraries[0][0].carrier,
        depDate: flight.itineraries[0][0].dep_date.substr(7, 2),
        depTime: flight.itineraries[0][0].dep_date.substr(10, 5),
        arrDate: flight.itineraries[0][0].arr_date.substr(7, 2),
        arrTime: flight.itineraries[0][0].arr_date.substr(10, 5),
      };
      modifiedFlights.push(flightData);
    });
    return modifiedFlights;
  }

  public mounted() {
    this.airlines.forEach((air) => {
      this.filterAirlines.push(air.key);
    });
  }

}
</script>

<style lang="scss">
</style>
