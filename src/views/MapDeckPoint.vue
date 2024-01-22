<!-- MapDeck -->
<template>
 <div id="mapbox">

 </div>
</template>

<script>
import mapboxgl from "mapbox-gl";
import MapboxLanguage from '@mapbox/mapbox-gl-language';
import { MapboxOverlay } from '@deck.gl/mapbox';
import { AmbientLight, DirectionalLight, LightingEffect} from '@deck.gl/core';
import { Tile3DLayer } from '@deck.gl/geo-layers';
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
                // center: [117.08,31.657], //中心点 火星
                // center: [117.287,31.748],//合肥
                center: [144.971,-37.806],
                //卫星
                style: 'mapbox://styles/mapbox/streets-v11', 
                //球面
                projection: 'globe', 
                //平面  
                // projection: 'equirectangular',      
                zoom:16,
                antialias: true,
                pitch: 45,
                attributionControl: false
            });
            map.addControl(new MapboxLanguage({
                defaultLanguage: 'zh-Hans'
            }))
            // 全屏
            map.addControl(new mapboxgl.FullscreenControl())
            //导航控件
            map.addControl(new mapboxgl.NavigationControl());
            // 比例尺控件
            map.addControl(new mapboxgl.ScaleControl())
            map.on('load',() =>{
              this.add3Dtiles(map)
            })
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
          // 创建环境光源
          const ambientLight = new AmbientLight({
            color: [255, 255, 255],
            intensity: 3.0
          });

          // 创建定向光源
          const directionalLight = new DirectionalLight({
            color: [255, 255, 255],
            intensity: 9.0,
            direction: [-3, -9, -1]
          });

          // 创建光照效果
          const lightingEffect = new LightingEffect({ ambientLight, directionalLight });
          const overlay = new MapboxOverlay({
            interleaved: true,
            layers: [
              new Tile3DLayer({
                id: 'plateau-3d-tiles',
                // 设置3D瓦片数据源
                // data: 'https://assets.cms.plateau.reearth.io/assets/4f/702958-5009-4d6b-a2e0-157c7e573eb2/13100_tokyo23-ku_2022_3dtiles%20_1_1_op_bldg_13101_chiyoda-ku_lod2_no_texture/tileset.json',
                // data: 'https://assets.cms.plateau.reearth.io/assets/14/b8f886-921d-46d3-9fd4-4f6e568b27d4/13100_tokyo23-ku_2022_3dtiles%20_1_1_op_bldg_13101_chiyoda-ku_lod2/tileset.json',
                // data:'http://127.0.0.1:5500/mapboxdemo/src/assets/blue/tileset.json',
                // data:'http://127.0.0.1:5501/src/assets/mars3d/tileset.json',
                // data:'http://127.0.0.1:5501/src/assets/hf/tileset.json',//合肥
                data:'http://127.0.0.1:5501/src/assets/3Dtiles/RoyalExhibitionBuilding/tileset.json',
                opacity: 1,
                pickable: true,//开启交互
                // getColor: [128, 0, 128],//定义层中对象的颜色
                // 【参考】https://github.com/nagix/mt3d-plugin-plateau
                // 瓦片加载完成时的回调函数
                onTilesetLoad: (tileset) => {
                  const { cartographicCenter } = tileset;
                  const [longitude, latitude] = cartographicCenter;
                  console.log(longitude, latitude);
                },
                // 单个瓦片加载完成时的回调函数
                onTileLoad: d => {
                  const { content } = d;
                  if (content && content.gltf && content.gltf.materials) {
                    content.gltf.materials.forEach(material => {
                      
                      if (material.pbrMetallicRoughness) {
                        // 设置材质的颜色
                        // material.pbrMetallicRoughness.baseColorFactor = [128 / 255, 0 / 255, 128 / 255, 1];

                      }
                    });
                  }
                  // 调整瓦片的高度
                  // content.cartographicOrigin.z -= 36.6741 + 3.1;
                }
              })
            ],
            effects: [
              lightingEffect
            ]
          });
          map.addControl(overlay);
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
