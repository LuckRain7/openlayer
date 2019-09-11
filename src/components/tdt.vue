<template>
    <div id="map" >
        <div  id="info"></div>
    </div>
</template>

<script>
    import 'ol/ol.css';
    import { Map, View } from 'ol';
    import TileLayer from 'ol/layer/Tile';
    import TileWMS from 'ol/source/TileWMS';
    import WMTS from 'ol/source/WMTS';
    import WMTSTileGrid from 'ol/tilegrid/WMTS';
    import {getWidth,getTopLeft} from 'ol/extent';
    import {get} from 'ol/proj';
    import ZoomSlider from 'ol/control/ZoomSlider';
    import ZoomToExtent from 'ol/control/ZoomToExtent';
    import ScaleLine from 'ol/control/ScaleLine';
    import Zoom from 'ol/control/Zoom';




    export default {
        data() {
            return {
                map: null
            };
        },

        mounted() {


            var projection = get('EPSG:4326');
            // 设置地图缩放级别分辨率
            var projectionExtent = projection.getExtent();
            var size = getWidth(projectionExtent) / 256;
            var resolutions = [];
            var matrixIds = [];
            for (var z = 0; z < 19; ++z) {
                // generate resolutions and matrixIds arrays for this WMTS
                resolutions[z] = size / Math.pow(2, z);
                matrixIds[z] = z;
            }


            const url = 'http://www.sdmap.gov.cn/tileservice/SDPubMap'
            const shiliang = new TileLayer({
                opacity: 0.7,
                source: new WMTS({
                    url: url,
                    projection: projection,
                    tileGrid: new WMTSTileGrid({
                        origin: getTopLeft(projectionExtent),
                        resolutions: resolutions,
                        matrixIds: matrixIds
                    }),
                    format: 'image/png',
                    style: 'default',
                    matrixSet: "EPSG:4326",
                    layer: 'sdvec',
                    LAYER: '0',
                    tileMatrixSet: 'taishannew',
                    wrapX: true
                })
            });
            const pt_source =new TileWMS({
                url: 'http://192.168.1.165:8080/geoserver/test/wms',
                params: {
                    // 参数
                    'layers': 'test:pt',
                    'bbox': '117.355354309082%2C35.892276763916%2C117.36400604248%2C35.8998184204102',
                    'width': '768',
                    'height': '669',

                },
                serverType: 'geoserver',
                transition: 0
            });
            const pt = new TileLayer({
                source: pt_source
            });



            var map = new Map({
                layers: [
                    shiliang, pt//, fw, xxyl, zzq, zs, znws, cmzzdp, sccj, ct, jg, dsjzx, gjzd, bjx, yqrk, cxlc
                ],
                target: 'map',
                view: new View({
                    // 调试很长时间 就是这个参数的问题
                    // 坐标系如果不设置，默认为墨卡托
                    projection: projection,
                    // 中心点位置
                    center: [117.3588, 35.898],
                    // 放大级别
                    zoom: 16
                })
            });
            //实例化ZoomSlider控件并加载到地图容器中
            var zoomslider = new ZoomSlider();
            map.addControl(zoomslider);

            //实例化zoomToExent控件并加载到地图容器中
            var zoomToExent = new ZoomToExtent({
                extend: [13100000, 4290000,
                    13200000, 5210000
                ]
            });
            map.addControl(zoomToExent);
            map.addControl(new ScaleLine());
            var zoom = new Zoom();
            map.addControl(zoom);

            //模拟查询的wms图层名称比如是wmsLayer
//该wmsLayer的数据源是墨卡托的3857举例

            // http://192.168.1.165:8080/geoserver/test/wms
            // ?SERVICE=WMS
            // &VERSION=1.1.1
            // &REQUEST=GetFeatureInfo
            // &FORMAT=image%2Fpng
            // &TRANSPARENT=true
            // &QUERY_LAYERS=test%3Apt
            // &LAYERS=test%3Apt
            // &exceptions=application%2Fvnd.ogc.se_inimage
            // &INFO_FORMAT=text%2Fhtml
            // &FEATURE_COUNT=50
            // &X=50
            // &Y=50
            // &SRS=EPSG%3A4326
            // &STYLES=
            // &WIDTH=101
            // &HEIGHT=101
            // &BBOX=117.35912203788759
            // %2C35.8928918838501
            // %2C117.36128926277162
            // %2C35.89505910873413



        }
    };
</script>

<style>
    #map{
        height: 850px;
        width: 900px;
        position: relative;
    }
    /*隐藏ol的一些自带元素*/
    .ol-attribution,.ol-zoom { display: none;}

</style>










