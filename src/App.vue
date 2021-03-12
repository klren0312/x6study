<template>
  <div id="app">
    <div class="app-stencil" ref="stencil" />
    <div class="app-content" ref="content" />
  </div>
</template>

<script>
import { Graph, Addon, Shape } from '@antv/x6'
const { Stencil } = Addon
const { Rect } = Shape
export default {
  name: 'App',
  mounted() {
    this.init()  
  },
  methods: {
    init() {
      const graph = new Graph({
        selecting: {
          enabled: true,
          rubberband: true,
          movable: true
        },
        container: document.querySelector('.app-content'),
        grid: {
          size: 10,
          visible: true,
          type: 'mesh'
        },
        snapline: {
          enabled: true,
          sharp: true,
        },
        translating: {
          restrict: true, // 将移动范围限制在画布距离画布边缘 20px 处
        }
      })

      // graph.centerContent()

      const stencil = new Stencil({
        title: '座位',
        target: graph,
        search(cell, keyword) {
          return cell.shape.indexOf(keyword) !== -1
        },
        placeholder: '搜索',
        notFoundText: '空',
        collapsable: true,
        stencilGraphWidth: 200,
        stencilGraphHeight: 180,
        groups: [
          {
            name: 'group1',
            title: '名称',
          }
        ],
      })
      document.querySelector('.app-stencil').appendChild(stencil.container)

      const seat1 = new Rect({
        width: 70,
        height: 40,
        attrs: {
          rect: { fill: '#31D0C6', strokeWidth: 0 },
          text: { text: '2', fill: 'white' },
        }
      })

      const seat2 = new Rect({
        width: 70,
        height: 70,
        attrs: {
          rect: { fill: '#31D0C6', strokeWidth: 0 },
          text: { text: '4', fill: 'white' },
        }
      })

      stencil.load([seat1, seat2], 'group1')

      graph.on('cell:change:*', () => {
        console.log(graph.toJSON())
      })

      graph.on('node:mouseenter', ({ node }) => {
        node.addTools({
          name: 'button-remove',
          args: {
            x: 0,
            y: 0,
            offset: { x: 10, y: 10 },
          },
        })
      })

      graph.on('node:mouseleave', ({ node }) => {
        node.removeTools()
      })
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
