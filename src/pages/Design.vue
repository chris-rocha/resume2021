<template>
<div class="inset">
<div v-if="loading" class="loading"><span class="material-icons">autorenew</span></div>
<transition-group tag="ul" class="two gd" name="bounce">
  <li v-for="(gd, index) in shuffled" :key="`loop-${index}`">
    <img loading="lazy" width="174" height="140" class="scale-with-grid" :src="gd.file" :alt="gd.type" />
    <div class="caption">
    <h4>{{ gd.type }}</h4>
    </div>
  </li>
</transition-group>
</div>
</template>

<script>
import axios from 'axios';

export default {
    name: "Design",
    data: function() {
        return {
            loading: false,
            design: []
        }
    },
    mounted() {
        this.fetchDesign();
    },
    computed: {
        shuffled: function (){
            let array = this.design;
            return array
                .map((value) => ({value, sort: Math.random()}))
                .sort((a,b) => a.sort - b.sort)
                .map(({value}) => value)
        }
    },
    methods: {
        async fetchDesign() {
            try {
                this.loading = true;
                const design = await axios.get('https://7plqc6kmkd.execute-api.us-east-2.amazonaws.com/prod/design-examples');
                this.design = design.data.body;
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
