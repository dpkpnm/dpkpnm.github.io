# dpkpnm.github.io<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<div id="app">
  <svg ref="svg" width="100vw" height="100vh">
    <path v-for="stroke in strokePaths" :d="stroke"></path>
  </svg>
  <pre class="console">
    Current stroke
    <div v-for="stroke in newStroke">{{stroke.x}},{{stroke.y}} @{{stroke.time}}</div>
  </pre>
</div>
<script>
var app = new Vue({
  el: '#app',
  data: {
    strokeData: [], // stores tme data and positions
    strokePaths: [], // stores svg d elements
    newStroke: [],
    isDrawing: false
  },
  methods: {
    mouseUp: function() {
      this.isDrawing = false;
      this.strokeData.push(this.newStroke);
    },
    mouseDown: function() {
      this.isDrawing = true;
      this.strokePaths.push("");
      this.newStroke = [];
    },
    mouseMove: function(event) {
      if (this.isDrawing) {
        this.strokePaths[this.strokePaths.length - 1] += (this.newStroke.length == 0 ? " M" : " L") + event.offsetX + " " + event.offsetY;
        this.newStroke.push({ x: event.offsetX, y: event.offsetY, time: Math.floor(new Date()) });
        this.$forceUpdate();
      }
    }
  },
  mounted() {
    window.addEventListener('mousedown', this.mouseDown, false);
    window.addEventListener('mousemove', this.mouseMove, false);
    window.addEventListener('mouseup', this.mouseUp, false);
  }
})
</script>
<style>
* {
  margin: 0;
  padding: 0;
}
.console {
  position: absolute;
  right: 0;
  top: 0;
  width: 300px;
  height: 100vh;
  overflow-y: auto;
  background-color: #eee;
  font-size: 12px;
  padding: 8px;
}
path {
  stroke-width: 2px;
  stroke-linecap: round;
  fill: none;
  stroke: red;
}
</style>
