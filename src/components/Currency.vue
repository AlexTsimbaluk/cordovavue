<template>
    <!-- <tr>
        <value
            :value="crypto.id"
        ></value>

        <value
            :value="crypto.name"
        ></value>

        <value
            :value="crypto.symbol"
        ></value>

        <value
            :value="crypto.rank"
        ></value>

        <value
            :value="crypto.circulating_supply"
        ></value>

        <value
            :value="crypto.total_supply"
        ></value>

        <value
            :value="crypto.max_supply"
        ></value>

        <value
            :value="crypto.last_updated"
        ></value>

        <value
            :value="crypto.price"
            :prev="prev.price"
        ></value>

        <value
            :value="crypto.volume_24h"
        ></value>

        <value
            :value="crypto.market_cap"
        ></value>

        <value
            :value="crypto.percent_change_1h"
        ></value>

        <value
            :value="crypto.percent_change_24h"
        ></value>

        <value
            :value="crypto.percent_change_7d"
        ></value>
    </tr> -->

    <tr>
        <value
            v-for="(val, key) in crypto"
            :value="crypto[key]"
            :prev="prev[key]"
            :key="key"
        ></value>
    </tr>
</template>

<script>
import Value from '@/components/Value';

export default {
    name: 'Currency',
    components: {
        Value
    },
    props: {
        crypto: Object,
        prev: Object
    }
}
</script>
