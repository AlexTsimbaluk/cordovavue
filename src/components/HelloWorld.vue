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

                <!-- <th rowspan="2">
                    Site
                </th> -->

                <th rowspan="2">
                    Rank
                </th>

                <th colspan="3">
                    Supply
                </th>

                <th rowspan="2">
                    Updated
                </th>

                <th colspan="3">
                    Quotes USD
                </th>

                <th colspan="3">
                    Percent change for
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

            <tr
                v-for="(crypto, key) in cryptos"
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
            cryptos: {},
            __cryptos: {},
            start: 0
        }
    },
    methods: {
        dateFromTimestap (sec) {
            return new Date(sec * 1e3).toISOString().slice(-13, -5);
        },
        getInterval (sec) {
            return Math.round(sec / 1000);
        },
        createTemp () {
            this.__cryptos = this.cryptos;
        },
        getData () {
            axios
                .get('https://api.coinmarketcap.com/v2/ticker/?sort=id&limit=1')
                .then((response) => {
                    try {
                        let data = response.data.data;

                        for (var obj in data) {
                            let crypto = data[obj];

                            for (var key in crypto) {
                                if (key == 'website_slug') {
                                    delete crypto[key];
                                }

                                if (key == 'quotes') {
                                    let quote = crypto[key]['USD'];
                                    delete crypto[key];

                                    for (var name in quote) {
                                        if (name == 'price') {
                                            crypto[name] = quote[name];
                                        }

                                        if (name == 'volume_24h') {
                                            crypto[name] = quote[name];
                                        }

                                        if (name == 'market_cap') {
                                            crypto[name] = quote[name];
                                        }

                                        if (name == 'percent_change_1h') {
                                            crypto[name] = quote[name];
                                        }

                                        if (name == 'percent_change_24h') {
                                            crypto[name] = quote[name];
                                        }

                                        if (name == 'percent_change_7d') {
                                            crypto[name] = quote[name];
                                        }
                                    }
                                }

                                if (key == 'last_updated') {
                                    crypto[key] = this.dateFromTimestap(crypto[key]);
                                }
                            }

                            if (this.cryptos[obj] == undefined) {
                                console.log('First request');
                            } else {
                                if (crypto['price'] != this.cryptos[obj]['price']) {
                                    let end = Date.now()
                                    console.log('Data updated');
                                    console.log(this.getInterval(end - this.start) + 'sec from previous update');
                                    this.start = end;

                                    this.createTemp();

                                    for (var key in crypto) {
                                        let val = crypto[key];
                                        let __val = this.__cryptos[obj][key];
                                        let diff = val - __val;
                                        if (key == 'price') {
                                            if (diff > 0) {
                                                console.log('up');
                                            } else if (diff < 0) {
                                                console.log('down');
                                            } else {
                                                console.log('not changed');
                                            }
                                        }
                                    }
                                }
                            }
                        }

                        this.cryptos = data;
                        this.dataRecieved = true;

                        for (var obj in this.cryptos) {
                            let crypto = this.cryptos[obj];

                            for (var key in crypto) {
                                /*if (key == 'price') {
                                    if (crypto[key] > this.__cryptos[obj][key]) {
                                        console.log('price grow');
                                    }
                                }*/
                            }
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
        this.start = Date.now();
        this.getData();

        setInterval(() => {
            this.getData();
        }, 1000);
    },
    beforeUpdate () {
        // console.log('::beforeUpdate');
    },
    updated () {
        // console.log('::updated');
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
