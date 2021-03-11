<template>
  <div id="app">
    <div class="app-stencil" ref="stencil" />
    <div class="app-content" ref="content" />
  </div>
</template>

<script>
import { Graph, Addon, Shape } from '@antv/x6'
const { Stencil } = Addon
const { Rect, Circle } = Shape
export default {
  name: 'App',
  mounted() {
    this.init()  
  },
  methods: {
    init() {
      const graph = new Graph({
        container: document.querySelector('.app-content'),
        grid: true,
        snapline: {
          enabled: true,
          sharp: true,
        }
        // scroller: {
        //   enabled: true,
        //   pageVisible: false,
        //   pageBreak: false,
        //   pannable: true,
        // },
      })

      graph.centerContent()

      const stencil = new Stencil({
        title: 'Components',
        target: graph,
        search(cell, keyword) {
          return cell.shape.indexOf(keyword) !== -1
        },
        placeholder: 'Search by shape name',
        notFoundText: 'Not Found',
        collapsable: true,
        stencilGraphWidth: 200,
        stencilGraphHeight: 180,
        groups: [
          {
            name: 'group1',
            title: 'Group(Collapsable)',
          }
        ],
      })
      document.querySelector('.app-stencil').appendChild(stencil.container)

      const r = new Rect({
        width: 70,
        height: 40,
        attrs: {
          rect: { fill: '#31D0C6', stroke: '#4B4A67', strokeWidth: 6 },
          text: { text: 'rect', fill: 'white' },
        },
      })

      const c = new Circle({
        width: 60,
        height: 60,
        attrs: {
          circle: { fill: '#FE854F', strokeWidth: 6, stroke: '#4B4A67' },
          text: { text: 'ellipse', fill: 'white' },
        },
      })

      const c2 = new Circle({
        width: 60,
        height: 60,
        attrs: {
          circle: { fill: '#4B4A67', 'stroke-width': 6, stroke: '#FE854F' },
          text: { text: 'ellipse', fill: 'white' },
        },
      })

      const r2 = new Rect({
        width: 70,
        height: 40,
        attrs: {
          rect: { fill: '#4B4A67', stroke: '#31D0C6', strokeWidth: 6 },
          text: { text: 'rect', fill: 'white' },
        },
      })

      stencil.load([r, c, c2, r2.clone()], 'group1') 
    }
  }
}
</script>

<style>
#app {
  font-family: sans-serif;
  padding: 0;
  display: flex;
  padding: 16px 8px;
}

.app-stencil {
  width: 200px;
  border: 1px solid #f0f0f0;
  position: relative;
}

.app-content {
  flex: 1;
  height: 520px;
  margin-left: 8px;
  margin-right: 8px;
  box-shadow: 0 0 10px 1px #e9e9e9;
}

.x6-graph-scroller {
  border: 1px solid #f0f0f0;
  margin-left: -1px;
}
</style>
