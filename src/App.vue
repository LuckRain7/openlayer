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
    import axios from 'axios';


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
            //http://192.168.1.165:8080/geoserver/test/wms?service=WMS&version=1.1.0&request=GetMap&layers=test%3Afw&bbox=117.354629516602%2C35.895435333252%2C117.36287689209%2C35.9005088806152&width=768&height=472&srs=EPSG%3A4326&format=application/openlayers
            const fw = new TileLayer({
                source: new TileWMS({
                    url: 'http://192.168.1.165:8080/geoserver/test/wms',
                    params: {
                        // 参数
                        'layers': 'test:fw',
                        'bbox': '117.354629516602%2C35.895435333252%2C117.36287689209%2C35.9005088806152',
                        'width': '768',
                        'height': '472',

                    },
                    serverType: 'geoserver',
                    transition: 0
                })
            });
            //http://192.168.1.165:8080/geoserver/test/wms?service=WMS&version=1.1.0&request=GetMap&layers=test%3Axxyl&bbox=117.357955932617%2C35.8974571228027%2C117.364402770996%2C35.9024391174316&width=768&height=593&srs=EPSG%3A4326&format=application/openlayers
            const xxyl = new TileLayer({
                source: new TileWMS({
                    url: 'http://192.168.1.165:8080/geoserver/test/wms',
                    params: {
                        // 参数
                        'layers': 'test:xxyl',
                        'bbox': '117.357955932617%2C35.8974571228027%2C117.364402770996%2C35.9024391174316',
                        'width': '768',
                        'height': '593',

                    },
                    serverType: 'geoserver',
                    transition: 0
                })
            });
            //http://192.168.1.165:8080/geoserver/test/wms?service=WMS&version=1.1.0&request=GetMap&layers=test%3Azzq&bbox=117.357437133789%2C35.8959045410156%2C117.363960266113%2C35.9027061462402&width=736&height=768&srs=EPSG%3A4326&format=application/openlayers
            const zzq = new TileLayer({
                source: new TileWMS({
                    url: 'http://192.168.1.165:8080/geoserver/test/wms',
                    params: {
                        // 参数
                        'layers': 'test:zzq',
                        'bbox': '117.357437133789%2C35.8959045410156%2C117.363960266113%2C35.9027061462402',
                        'width': '736',
                        'height': '768',

                    },
                    serverType: 'geoserver',
                    transition: 0
                })
            });

            //http://192.168.1.165:8080/geoserver/test/wms?service=WMS&version=1.1.0&request=GetMap&layers=test%3Azs&bbox=117.363265991211%2C35.8999519348145%2C117.364349365234%2C35.9024353027344&width=335&height=768&srs=EPSG%3A4326&format=application/openlayers
            const zs = new TileLayer({
                source: new TileWMS({
                    url: 'http://192.168.1.165:8080/geoserver/test/wms',
                    params: {
                        // 参数
                        'layers': 'test:zs',
                        'bbox': '117.363265991211%2C35.8999519348145%2C117.364349365234%2C35.9024353027344',
                        'width': '335',
                        'height': '768',

                    },
                    serverType: 'geoserver',
                    transition: 0
                })
            });

            //http://192.168.1.165:8080/geoserver/test/wms?service=WMS&version=1.1.0&request=GetMap&layers=test%3Aznws&bbox=117.356369018555%2C35.8988342285156%2C117.357604980469%2C35.8995552062988&width=768&height=448&srs=EPSG%3A4326&format=application/openlayers
            const znws = new TileLayer({
                source: new TileWMS({
                    url: 'http://192.168.1.165:8080/geoserver/test/wms',
                    params: {
                        // 参数
                        'layers': 'test:znws',
                        'bbox': '117.356369018555%2C35.8988342285156%2C117.357604980469%2C35.8995552062988',
                        'width': '768',
                        'height': '448',

                    },
                    serverType: 'geoserver',
                    transition: 0
                })
            });

            //http://192.168.1.165:8080/geoserver/test/wms?service=WMS&version=1.1.0&request=GetMap&layers=test%3Acmzzdp&bbox=117.35569805517446%2C35.89549381273969%2C117.35712573177784%2C35.896318692554985&width=768&height=443&srs=EPSG%3A4326&format=application/openlayers
            const cmzzdp = new TileLayer({
                source: new TileWMS({
                    url: 'http://192.168.1.165:8080/geoserver/test/wms',
                    params: {
                        // 参数
                        'layers': 'test:cmzzdp',
                        'bbox': '117.35569805517446%2C35.89549381273969%2C117.35712573177784%2C35.896318692554985',
                        'width': '768',
                        'height': '443',

                    },
                    serverType: 'geoserver',
                    transition: 0
                })
            });

            //http://192.168.1.165:8080/geoserver/test/wms?service=WMS&version=1.1.0&request=GetMap&layers=test%3Asccj&bbox=117.357650756836%2C35.8932037353516%2C117.36336517334%2C35.9009666442871&width=565&height=768&srs=EPSG%3A4326&format=application/openlayers
            const sccj = new TileLayer({
                source: new TileWMS({
                    url: 'http://192.168.1.165:8080/geoserver/test/wms',
                    params: {
                        // 参数
                        'layers': 'test:sccj',
                        'bbox': '117.35569805517446%2C35.89549381273969%2C117.35712573177784%2C35.896318692554985',
                        'width': '565',
                        'height': '768',

                    },
                    serverType: 'geoserver',
                    transition: 0
                })
            });

            //http://192.168.1.165:8080/geoserver/test/wms?service=WMS&version=1.1.0&request=GetMap&layers=test%3Act&bbox=117.356719970703%2C35.8999977111816%2C117.363220214844%2C35.9007186889648&width=768&height=330&srs=EPSG%3A4326&format=application/openlayers
            const ct = new TileLayer({
                source: new TileWMS({
                    url: 'http://192.168.1.165:8080/geoserver/test/wms',
                    params: {
                        // 参数
                        'layers': 'test:ct',
                        'bbox': '117.356719970703%2C35.8999977111816%2C117.363220214844%2C35.9007186889648',
                        'width': '768',
                        'height': '330',

                    },
                    serverType: 'geoserver',
                    transition: 0
                })
            });

            //http://192.168.1.165:8080/geoserver/test/wms?service=WMS&version=1.1.0&request=GetMap&layers=test%3Ajg&bbox=117.357437133789%2C35.8977699279785%2C117.363227844238%2C35.9003028869629&width=768&height=335&srs=EPSG%3A4326&format=application/openlayers
            const jg = new TileLayer({
                source: new TileWMS({
                    url: 'http://192.168.1.165:8080/geoserver/test/wms',
                    params: {
                        // 参数
                        'layers': 'test:jg',
                        'bbox': '117.357437133789%2C35.8977699279785%2C117.363227844238%2C35.9003028869629',
                        'width': '768',
                        'height': '335',

                    },
                    serverType: 'geoserver',
                    transition: 0
                })
            });

            //http://192.168.1.165:8080/geoserver/test/wms?service=WMS&version=1.1.0&request=GetMap&layers=test%3Adsjzx&bbox=117.356536865234%2C35.8998107910156%2C117.357841491699%2C35.8999786376953&width=768&height=330&srs=EPSG%3A4326&format=application/openlayers
            const dsjzx = new TileLayer({
                source: new TileWMS({
                    url: 'http://192.168.1.165:8080/geoserver/test/wms',
                    params: {
                        // 参数
                        'layers': 'test:dsjzx',
                        'bbox': '117.356536865234%2C35.8998107910156%2C117.357841491699%2C35.8999786376953',
                        'width': '768',
                        'height': '330',

                    },
                    serverType: 'geoserver',
                    transition: 0
                })
            });

            //http://192.168.1.165:8080/geoserver/test/wms?service=WMS&version=1.1.0&request=GetMap&layers=test%3Agjzd&bbox=117.356109619141%2C35.9000968933105%2C117.356117248535%2C35.9001007080078&width=768&height=384&srs=EPSG%3A4326&format=application/openlayers
            const gjzd = new TileLayer({
                source: new TileWMS({
                    url: 'http://192.168.1.165:8080/geoserver/test/wms',
                    params: {
                        // 参数
                        'layers': 'test:gjzd',
                        'bbox': '117.356109619141%2C35.9000968933105%2C117.356117248535%2C35.9001007080078',
                        'width': '768',
                        'height': '384',

                    },
                    serverType: 'geoserver',
                    transition: 0
                })
            });

            //http://192.168.1.165:8080/geoserver/test/wms?service=WMS&version=1.1.0&request=GetMap&layers=test%3Ayqrk&bbox=117.356422424316%2C35.899787902832%2C117.356483459473%2C35.8999519348145&width=330&height=768&srs=EPSG%3A4326&format=application/openlayers
            const bjx = new TileLayer({
                source: new TileWMS({
                    url: 'http://192.168.1.165:8080/geoserver/test/wms',
                    params: {
                        // 参数
                        'layers': 'test:bjx',
                        'bbox': '117.354316711426%2C35.8919165676758%2C117.364501953125%2C35.9028739929199',
                        'width': '713',
                        'height': '768',

                    },
                    serverType: 'geoserver',
                    transition: 0
                })
            })

            //http://192.168.1.165:8080/geoserver/test/wms?service=WMS&version=1.1.0&request=GetMap&layers=test%3Abjx&bbox=117.354316711426%2C35.8919165676758%2C117.364501953125%2C35.9028739929199&width=713&height=768&srs=EPSG%3A4326&format=application/openlayers
            const yqrk = new TileLayer({
                source: new TileWMS({
                    url: 'http://192.168.1.165:8080/geoserver/test/wms',
                    params: {
                        // 参数
                        'layers': 'test:yqrk',
                        'bbox': '117.356422424316%2C35.899787902832%2C117.356483459473%2C35.8999519348145',
                        'width': '330',
                        'height': '768',

                    },
                    serverType: 'geoserver',
                    transition: 0
                })
            })

            //http://192.168.1.165:8080/geoserver/test/wms?service=WMS&version=1.1.0&request=GetMap&layers=test%3Acxlc&bbox=117.362739562988%2C35.9008140563965%2C117.363128662109%2C35.9009399414063&width=768&height=330&srs=EPSG%3A4326&format=application/openlayers
            const cxlc = new TileLayer({
                source: new TileWMS({
                    url: 'http://192.168.1.165:8080/geoserver/test/wms',
                    params: {
                        // 参数
                        'layers': 'test:cxlc',
                        'bbox': '117.362739562988%2C35.9008140563965%2C117.363128662109%2C35.900939941406',
                        'width': '768',
                        'height': '330',

                    },
                    serverType: 'geoserver',
                    transition: 0
                })
            })


            var map = new Map({
                layers: [
                    shiliang, pt, fw, xxyl, zzq, zs, znws, cmzzdp, sccj, ct, jg, dsjzx, gjzd, bjx, yqrk, cxlc
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


            map.on('click',mapClick);
            function mapClick(evt){
                console.log(evt);
                var bbox =  evt.map.previousExtent_;
                const url =`http://192.168.1.165:8080/geoserver/test/wms?SERVICE=WMS&VERSION=1.1.1&REQUEST=GetFeatureInfo&FORMAT=image%2Fpng&TRANSPARENT=true&QUERY_LAYERS=test%3Apt&LAYERS=test%3Apt&exceptions=application%2Fvnd.ogc.se_inimage&INFO_FORMAT=text%2Fhtml&FEATURE_COUNT=50&X=50&Y=50&SRS=EPSG%3A4326&STYLES=&WIDTH=101&HEIGHT=101&BBOX=${bbox[0]},${bbox[1]},${bbox[2]},${bbox[3]}`

                axios.get(url).then((data)=>{
                      console.log(data);
                })
                // var viewResolution = map.getView().getResolution();
                //
                // var url = pt.getSource().getGetFeatureInfoUrl(
                //     evt.coordinate, viewResolution, 'EPSG:3857',
                //     {
                //         'INFO_FORMAT': 'text/javascript',//geoserver支持jsonp才能输出为jsonp的格式
                //         'FEATURE_COUNT': 50     //点击查询能返回的数量上限
                //     });
                // $.ajax({
                //     type: 'GET',
                //     url:url,
                //     dataType: 'jsonp',
                //     jsonp:'format_options',
                //     jsonpCallback:"callback:success_jsonpCallback"
                // });
            }
//回调函数接收查询结果

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










