<template>
    <td
		:class="getClassName"
    >
        {{ value }}
    </td>
</template>

<script>
export default {
    name: 'Value',
    data () {
    	return {

    	}
    },
    props: ['value', 'prev'],
    computed: {
    	getClassName () {
    		let diff = this.value - this.prev;

    		if (diff > 0) {
    		    return 'higher';
    		} else if (diff < 0) {
    		    return 'lower';
    		} else {
    		}
    	}
    }
}
</script>

<style>
	.higher {
		color: #0f0;
	}
	.lower {
		color: #f00;
	}
</style>