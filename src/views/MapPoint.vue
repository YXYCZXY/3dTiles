<!-- Map3Dtiles -->
<template>
  <div id="mapbox">
 
  </div>
 </template>
 
 <script>
 import mapboxgl from "mapbox-gl";
 // import mapboxgl from '@cgcs2000/mapbox-gl';
 // import MapboxLanguage from '@mapbox/mapbox-gl-language';
 import FrameRateControl from '../assets/framerate';
  export default {
    name:'',
    mixins:[],
    components: {
 
    },
    props:{},
    data () {
      return {
 
      }
    },
    computed:{},
    mounted(){
     this.onload()
    },
    destroyed:{},
    methods:{
         onload() {
             let _this = this
             mapboxgl.accessToken = 'pk.eyJ1IjoiMjQ1MTc0ODYyIiwiYSI6ImNqbW82ZmE4dDBpMngza3Fvd3h2aXMxbXEifQ.JtY2C9MHBAkF6Wu2fdyTXQ';
             // mapboxgl.setRTLTextPlugin('https://api.mapbox.com/mapbox-gl-js/plugins/mapbox-gl-rtl-text/v0.2.3/mapbox-gl-rtl-text.js');
             let map = new mapboxgl.Map({
                 container: "mapbox",   //地图容器的id
                 //卫星
                 style: 'mapbox://styles/mapbox/dark-v10',    
                 center: [4.478322, 51.899662],
                  zoom: 18.5,
                  bearing: 27.8,
                  pitch: 58,
                  hash: true
             });
             // map.addControl(new MapboxLanguage({
             //     defaultLanguage: 'zh-Hans'
             // }))
             // 全屏
             map.addControl(new mapboxgl.FullscreenControl())
             //导航控件
             map.addControl(new mapboxgl.NavigationControl());
             // 比例尺控件
             map.addControl(new mapboxgl.ScaleControl())
             map.on('load',() =>{
               this.add3Dtiles(map)
             })
             // this.add3Dtiles(map)
         },
         // 创建 MapboxLayer 实例
         // const mapboxLayer = new THREE.MapboxLayer({
         //   map: map,
         //   opacity: 1,
         // });
 
         // 将 MapboxLayer 实例添加到场景中
         // scene.add(mapboxLayer);
 
         // 创建渲染器并开始渲染
         // const renderer = new THREE.WebGLRenderer();
         // renderer.setSize(window.innerWidth, window.innerHeight);
         // document.body.appendChild(renderer.domElement);
 
         // function animate() {
         //   requestAnimationFrame(animate);
         //   renderer.render(scene, camera);
         // }
         // animate();
         add3Dtiles(map){
           let tileslayer = new Mapbox3DTiles.Layer( { 
                     id: 'delaware', 
                     // url: 'http://127.0.0.1:5501/src/assets/dianyun2/tileset.json',
                     // url: 'http://127.0.0.1:5501/src/assets/hf/tileset.json',
                     // url:'http://127.0.0.1:5501/src/assets/map3dtiles/tileset.json'
                     url:'http://127.0.0.1:5501/src/assets/dianyun2/tileset.json',
                     lighting: 'ambient'
                 } );
                 console.log(tileslayer);
                 map.addLayer(tileslayer);
                 let tileslayer2 = new Mapbox3DTiles.Layer( { 
                     id: 'delaware2', 
                     // url: 'http://127.0.0.1:5501/src/assets/dianyun2/tileset.json',
                     // url: 'http://127.0.0.1:5501/src/assets/hf/tileset.json',
                     // url:'http://127.0.0.1:5501/src/assets/map3dtiles/tileset.json'
                     url:'http://127.0.0.1:5501/src/assets/dianyun2/tileset2.json',
                     lighting: 'ambient',
                     color: 0x0033aa, 
                      opacity: 1
                 } );
                 map.on('mousemove', (event)=>{
                  let features = map.queryRenderedFeatures(event.point, {outline: true, outlineColor: 0xff0000});
              })
                 map.addLayer(tileslayer2);
                 tileslayer.render()
                 const fps = new FrameRateControl({ /* optional options */ });
                 map.addControl(fps)
                
         }
    }
  }
 </script>
 
 <style scoped>
 #mapbox{
     width:100%;
     height:100%
 }
  
 </style>
 