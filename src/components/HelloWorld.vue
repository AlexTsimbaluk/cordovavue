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
                    Quotes
                </th>

                <th rowspan="2">
                    Updated
                </th>
            </tr>

            <tr>
                <!-- <th colspan="5"></th> -->

                <th>
                    Circulating
                </th>

                <th>
                    Total
                </th>

                <th>
                    Max
                </th>

                <!-- <th colspan="2"></th> -->
            </tr>

            <tr
                v-for="(crypto, index) in cryptos"
            >
                <td v-for="(value, key) in crypto">{{value}}</td>
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
            cryptos: null
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
                        let _cryptos = response.data.data;

                        for (var key in _cryptos) {
                            let crypto = _cryptos[key];
                            for (var key in crypto) {
                                if (key == 'last_updated') {
                                    crypto[key] = this.dateFromTimestap(crypto[key]);
                                }

                                if (key == 'quotes') {
                                    let quotes = crypto[key];
                                    for (var key in quotes) {
                                        console.log(quotes[key]);
                                    }
                                }
                            }
                        }

                        this.cryptos = _cryptos;
                        this.dataRecieved = true;
                    } catch(e) {
                        console.log('Error::No response');
                        throw new Error(e);
                        $('#app').addClass('js-error error-no-response-data');
                        $('body').append('<div>Data from Database failed');
                    }
                })
                .catch((error) => {
                    console.log('Error::не удалось создать ajax-запрос');
                    $('body').append('<div>Ajax failed');
                    $('#app').addClass('js-error error-ajax-query');
                    console.log(error)
                });
        }
    },
    created () {
        this.getData();

        setInterval(() => {
            console.log('::Updated');
            this.getData();
        }, 30000);
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
    border-collapse: collapse;
    border: 1px solid #333;
}

tr {

}

th {
    border: 1px solid #333;
    padding: 5px;
    text-align: left;
}
td {
    padding: 5px;
}
</style>
