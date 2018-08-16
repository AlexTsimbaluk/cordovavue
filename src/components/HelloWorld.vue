<template>
    <div>
        <table v-if="dataRecieved">
            <tr>
                <th v-for="(value, key) in cryptos['1']">{{key}}</th>
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
    created () {
        axios
            // .get(' https://api.coinmarketcap.com/v2/listings/')
            .get(' https://api.coinmarketcap.com/v2/ticker/')
            .then((response) => {
                try {
                    this.cryptos = (response.data.data);
                    this.dataRecieved = true;
                    console.log('Request success');
                    console.log(this.cryptos);
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

th {
    padding: 4px 2px;
    text-align: left;
}
td {
    padding: 2px;
}
</style>
