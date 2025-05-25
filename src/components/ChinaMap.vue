<!-- filepath: g:\project\2025\map\src\components\ChinaMap.vue -->
<template>
  <div class="flex">
    <div id="mapdiv" style="width: 70vw; height: 50vw; background: #EEE;"></div>
    <div class="side-panel">
      <button class="map-btn" @click="randomHighlight">随机高亮省份</button><br/><br/>
      <button class="map-btn" @click="showInfo">显示省份信息</button>
      <div id="info" class="info-box">{{ infoText }}</div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ChinaMap",
  data() {
    return {
      map: null,
      selectedId: null,
      infoText: "",
      provinceInfo: {
        "CN-11": { name: "北京市", abbr: "京", capital: "北京" },
        "CN-12": { name: "天津市", abbr: "津", capital: "天津" },
        "CN-13": { name: "河北省", abbr: "冀", capital: "石家庄" },
        "CN-14": { name: "山西省", abbr: "晋", capital: "太原" },
        "CN-15": { name: "内蒙古自治区", abbr: "蒙", capital: "呼和浩特" },
        "CN-21": { name: "辽宁省", abbr: "辽", capital: "沈阳" },
        "CN-22": { name: "吉林省", abbr: "吉", capital: "长春" },
        "CN-23": { name: "黑龙江省", abbr: "黑", capital: "哈尔滨" },
        "CN-31": { name: "上海市", abbr: "沪", capital: "上海" },
        "CN-32": { name: "江苏省", abbr: "苏", capital: "南京" },
        "CN-33": { name: "浙江省", abbr: "浙", capital: "杭州" },
        "CN-34": { name: "安徽省", abbr: "皖", capital: "合肥" },
        "CN-35": { name: "福建省", abbr: "闽", capital: "福州" },
        "CN-36": { name: "江西省", abbr: "赣", capital: "南昌" },
        "CN-37": { name: "山东省", abbr: "鲁", capital: "济南" },
        "CN-41": { name: "河南省", abbr: "豫", capital: "郑州" },
        "CN-42": { name: "湖北省", abbr: "鄂", capital: "武汉" },
        "CN-43": { name: "湖南省", abbr: "湘", capital: "长沙" },
        "CN-44": { name: "广东省", abbr: "粤", capital: "广州" },
        "CN-45": { name: "广西壮族自治区", abbr: "桂", capital: "南宁" },
        "CN-46": { name: "海南省", abbr: "琼", capital: "海口" },
        "CN-50": { name: "重庆市", abbr: "渝", capital: "重庆" },
        "CN-51": { name: "四川省", abbr: "川", capital: "成都" },
        "CN-52": { name: "贵州省", abbr: "黔", capital: "贵阳" },
        "CN-53": { name: "云南省", abbr: "滇", capital: "昆明" },
        "CN-54": { name: "西藏自治区", abbr: "藏", capital: "拉萨" },
        "CN-61": { name: "陕西省", abbr: "陕", capital: "西安" },
        "CN-62": { name: "甘肃省", abbr: "甘", capital: "兰州" },
        "CN-63": { name: "青海省", abbr: "青", capital: "西宁" },
        "CN-64": { name: "宁夏回族自治区", abbr: "宁", capital: "银川" },
        "CN-65": { name: "新疆维吾尔自治区", abbr: "新", capital: "乌鲁木齐" },
        "CN-71": { name: "台湾省", abbr: "台", capital: "台北" },
        "CN-91": { name: "香港特别行政区", abbr: "港", capital: "香港" },
        "CN-92": { name: "澳门特别行政区", abbr: "澳", capital: "澳门" }
      }
    };
  },
  mounted() {
    // 动态加载ammap和chinaLow.js
    //原本用的是绝对路径/ammap/ammap.js，改为相对路径，因为打包后路径会变
    this.loadScript("ammap/ammap.js", () => {
      this.loadScript("ammap/maps/js/chinaLow.js", this.initMap);
      this.loadCss("ammap/ammap.css");
    });
  },
  methods: {
    loadScript(src, callback) {
      if (document.querySelector(`script[src="${src}"]`)) {
        callback && callback();
        return;
      }
      const script = document.createElement("script");
      script.src = src;
      script.onload = callback;
      document.body.appendChild(script);
    },
    loadCss(href) {
      if (document.querySelector(`link[href="${href}"]`)) return;
      const link = document.createElement("link");
      link.rel = "stylesheet";
      link.href = href;
      document.head.appendChild(link);
    },
    initMap() {
    this.map = new window.AmCharts.AmMap();
    this.map.dataProvider = {
        map: "chinaLow",
        getAreasFromMap: true
    };
    this.map.areasSettings = {
        autoZoom: false,
        selectedColor: "#FF0000",
        rollOverColor: "#E0E0E0",   // 鼠标悬停色
        outlineColor: "#000", // 新增：省份描边为黑色
        outlineThickness: 0.2   // 可选：描边宽度，默认1
    };
    // 监听地图初始化完成
    this.map.addListener("init", () => {
        // 地图和区域已准备好，可以安全访问 this.map.areas
        this.infoText = "地图已加载，可以随机高亮省份";
        // console.log('chinaLow:', window.AmCharts.maps.chinaLow);
    });
    this.map.write("mapdiv");
    },
randomHighlight() {
    // 直接从 window.AmCharts.maps.chinaLow 获取所有区域 id
    const mapData = window.AmCharts && window.AmCharts.maps && window.AmCharts.maps.chinaLow;
    if (!mapData || !mapData.svg || !mapData.svg.g || !Array.isArray(mapData.svg.g.path)) {
        this.infoText = "chinaLow 地图数据未加载或格式错误";
        return;
    }
    const areaIds = mapData.svg.g.path.map(a => a.id).filter(Boolean);
    if (areaIds.length === 0) {
        this.infoText = "未找到省份数据";
        return;
    }
    const idx = Math.floor(Math.random() * areaIds.length);
    const randomId = areaIds[idx];
    this.selectedId = randomId;
    console.log("随机选择省份：", randomId);

    const paths = window.AmCharts.maps.chinaLow.svg.g.path;
    const area = paths.find(item => item.id === randomId);
    const title = area ? area.title : null;
    console.log(title); // eg:输出 "Anhui"

  //     // 通过 SVG 直接操作高亮
  //   this.$nextTick(() => {
  //       // 先重置所有省份颜色
  //       const svg = document.querySelector("#mapdiv svg");
  //       if (svg) {
  //           // 只重置有 aria-label 的 path
  //           svg.querySelectorAll('path[aria-label]').forEach(path => {
  //               path.setAttribute("fill", "#FFD000"); // 默认色 #E0E0E0
  //           });
  //           // 高亮选中省份
  //           const selectedPath = Array.from(svg.querySelectorAll('path[aria-label]')).find(
  //               p => (p.getAttribute("id") || p.getAttribute("data-id") || p.getAttribute("aria-label")).includes(title)
  //           );
  //           if (selectedPath) {
  //               selectedPath.setAttribute("fill", "#E0E0E0"); // 高亮色 #FFD000
  //           }
  //       }
  //   }
  // );

    // 通过 amMap API 高亮
    if (this.map) {
        const areaObj = this.map.getObjectById(randomId);
        if (areaObj) {
            this.map.selectObject(areaObj);
        }
    }
    this.infoText = "";

},
    showInfo() {
      if (!this.selectedId) {
        this.infoText = "请先随机选择一个省份";
        return;
      }
      const info = this.provinceInfo[this.selectedId];
      if (info) {
        this.infoText = `省份：${info.name}\n简称：${info.abbr}\n省会：${info.capital}`;
      } else {
        this.infoText = "未找到该省份信息";
      }
    }
  }
};
</script>

<style scoped>
.flex {
  display: flex;
  align-items: center;
  height: 100vh;
}
.side-panel {
  margin-left: 20px;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}
.map-btn {
  display: inline-block;
  padding: 12px 32px;
  margin-bottom: 8px;
  font-size: 18px;
  font-weight: bold;
  color: #fff;
  background: linear-gradient(90deg, #ffb300 0%, #ff6f00 100%);
  border: none;
  border-radius: 24px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.08);
  cursor: pointer;
  transition: background 0.2s, transform 0.1s;
}
.map-btn:hover {
  background: linear-gradient(90deg, #ff9800 0%, #f44336 100%);
  transform: translateY(-2px) scale(1.04);
}
/* 新增：优化信息显示区域样式 */
.info-box {
  margin-top: 20px;
  padding: 20px 28px;
  min-width: 220px;
  min-height: 80px;
  background: #fffbe6;
  color: #333;
  border-radius: 16px;
  box-shadow: 0 2px 12px rgba(255, 179, 0, 0.12);
  font-size: 18px;
  line-height: 2;
  white-space: pre-line;
  border: 1px solid #ffe082;
  transition: box-shadow 0.2s;
}
.info-box:empty {
  display: none;
}
</style>