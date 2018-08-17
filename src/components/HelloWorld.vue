<template>
    <div>
        <!-- <div v-if="dataRecieved">
            {{ cryptos }}
        </div> -->

        <table v-if="dataRecieved">
            <tr>
                <!-- <th v-for="(value, key) in cryptos['1']">{{key}}</th> -->
                <th rowspan="2">
                    ID
                </th>

                <th rowspan="2">
                    Name
                </th>

                <th rowspan="2">
                    Symbol
                </th>

                <th rowspan="2">
                    Site
                </th>

                <th rowspan="2">
                    Rank
                </th>

                <th colspan="3">
                    Supply
                </th>

                <th rowspan="2">
                    Updated
                </th>

                <th class="p_0">
                    <tr>
                        <th colspan="3">
                            Quotes USD
                        </th>

                        <th colspan="3">
                            Percent change for
                        </th>
                    </tr>
                </th>

            </tr>

            <tr>
                <th>
                    Circulating
                </th>

                <th>
                    Total
                </th>

                <th>
                    Max
                </th>

                <th class="p_0">
                    <tr>
                        <th>
                            Price
                        </th>

                        <th>
                            Volume 24h
                        </th>

                        <th>
                            Market cap
                        </th>

                        <th>
                            1h
                        </th>

                        <th>
                            24h
                        </th>

                        <th>
                            7d
                        </th>
                    </tr>
                </th>
            </tr>

            <tr
                v-for="(crypto, index) in cryptos"
            >
                <td v-for="(value, key) in crypto">{{value}}</td>

                <td>
                    <tr>
                        <td v-for="(value, key) in quotes[index]['USD']">
                            {{ value }}
                        </td>
                    </tr>
                </td>
            </tr>

        </table>
    </div>
</template>

<script>
import $ from 'jquery';
import axios from 'axios';

export default {
    name: 'HelloWorld',
    data () {
        return {
            dataRecieved: false,
            cryptos: {},
            quotes: {}
        }
    },
    methods: {
        dateFromTimestap (sec) {
            return new Date(sec * 1e3).toISOString().slice(-13, -5);
        },
        getData () {
            axios
                .get(' https://api.coinmarketcap.com/v2/ticker/?sort=id')
                .then((response) => {
                    try {
                        let data = response.data.data;

                        for (var obj in data) {
                            let crypto = data[obj];

                            for (var key in crypto) {
                                if (key == 'quotes') {
                                    let quotes = crypto[key];
                                    this.quotes[obj] = quotes;
                                    delete crypto[key];
                                }

                                if (key == 'last_updated') {
                                    crypto[key] = this.dateFromTimestap(crypto[key]);
                                }
                            }

                            this.cryptos[obj] = crypto;
                        }

                        this.dataRecieved = true;
                        console.log('Request::Success');

                        console.log(this.quotes[9]);
                        for (var key in this.quotes[9]) {
                            console.log(this.quotes[9][key]);
                        }
                    } catch(e) {
                        console.log('Error::No response');
                        throw new Error(e);
                    }
                })
                .catch((error) => {
                    console.log('Error::не удалось создать ajax-запрос');
                    console.log(error)
                });
        }
    },
    created () {
        this.getData();

        setInterval(() => {
            console.log('::Updated');
            this.getData();
        }, 10000);
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  margin: 0 10px;
}
a {
  color: #42b983;
}

table {
    border: 1px solid #333;
    border-collapse: collapse;
    border-spacing: 0;
}

tr {

}

th {
    border: 1px solid #333;
    padding: 5px;
    text-align: left;
}
th tr th {
    border-top: 0;
    border-bottom: 0;
}
td {
    padding: 5px;
}
.p_0 {
    padding: 0;
}
</style>
