<template>
  <div class="caret-wrapper">
    <button @click="Comp = 'Map'">3Dtiles-白膜</button>
    <button @click="Comp = 'MapBuld'">3Dtiles-批量模型</button>
    <button @click="Comp = 'MapPoint'">3Dtiles-点云</button>
    <button @click="Comp = 'MapDeckPoint'">Deck-点云</button>
    <button @click="Comp = 'MapDeck'">Deck-白膜</button>
    <button @click="Comp = 'MapDeckMd'">Deck-模型</button>
    <button @click="Comp = 'MapDeckSY'">Deck-倾斜摄影</button>
    <component :is="Comp">这是div</component>
  </div>
  
</template>

<script>
import Map from './Map';
import MapDeckPoint from './MapDeckPoint.vue';
import MapDeck from './MapDeck.vue';
import MapBuld from './MapBuld.vue';
import MapPoint from './MapPoint.vue';
import MapDeckMd from './MapDeckMd.vue';
import MapDeckSY from './MapDeckSY.vue';
 export default {
   name:'',
   mixins:[],
   components: {
    Map,
    MapDeckPoint,
    MapBuld,
    MapPoint,
    MapDeck,
    MapDeckMd,
    MapDeckSY
   },
   props:{},
   data () {
     return {
      Comp:'Map'
     }
   },
   computed:{},
   mounted(){},
   destroyed:{},
   methods:{}
 }
</script>

<style scoped>
.caret-wrapper{
  width:100%;
  height:100%
}

::v-deep .el-main{
  padding:0
}
 
</style>
