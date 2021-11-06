<template>
<div class="inset">
<div v-if="loading" class="loading"><span class="material-icons">autorenew</span></div>
<transition-group tag="ul" class="two" name="bounce">
    <li v-for="(c, i) in sortCode" :key="`loop-${i}`">
        <a target="_blank" :href="c.link" :title="c.title">
            <img loading="lazy" width="300" height="240" :alt="c.title" class="scale-with-grid" :src="c.thumb" />
            <div class="caption">
            <h4>{{ c.title }}</h4>
            <div class="details">{{ c.details}}</div>
            </div>
        </a>
    </li>
</transition-group>
</div>
</template>

<script>
import axios from 'axios';

export default {
    name: "Code",
    data: function() {
        return {
            loading: false,
            code: [],
            order: ['explore-branson', 'discover-puerto-rico', 'visit-savannah', 'bermuda', 'ecotours']
        }
    },
    mounted() {
        this.fetchCode();
    },
    computed: {
        sortCode: function (){
            let array = this.code;
            let order =  this.order;
            return array.sort((a, b) => {
                if (order.indexOf(a.slug) > order.indexOf(b.slug)) {
                    return 1;
                } else {
                    return -1;
                }
            });
        }
    },
    methods: {
        async fetchCode() {
            try {
                this.loading = true;
                const code = await axios.get('https://z2t4tas4ok.execute-api.us-east-2.amazonaws.com/prod/code-examples');
                this.code = code.data.body;
            }
            catch (e) {
                console.log(e);
            }
            finally {
                this.loading = false;
            }
        },
    }
};
</script>
