<template>
  <div>
    <!-- 海洋 -->
    <mesh name="Ocean" @update:obj="handleMesh">
      <geometry type="Plane" :args="[10000, 10000, 40, 40]"></geometry>
      <material type="MeshBasic" :color="0x0044ff">
        <texture :options="txtOpts"></texture>
      </material>
    </mesh>
    <animation v-if="geom" :fn="animate" :speed="speed"></animation>
  </div>
</template>

<script>
import { RepeatWrapping } from 'three'
import { Object3D } from 'vue-threejs'

export default {
  name: 'Ocean',
  mixins: [Object3D],

  data () {
    return {
      txtOpts: {/* 海洋材质 */
        url: require('../../static/water.jpg'),
        wrapS: RepeatWrapping,
        wrapT: RepeatWrapping,
        repeat: [20, 20]
      },
      speed:10,
      geom: null
    }
  },

  methods: {
    handleMesh (mesh) {
      if (!mesh) return
      let g = mesh.geometry
      g.rotateX(-Math.PI / 2)
      for (let i = 0, l = g.vertices.length; i < l; i++) {
        g.vertices[ i ].y = 10 * Math.sin(i / 2)
      }
      this.geom = g
    },

    animate (tt) {
      let g = this.geom
      for (let i = 0, l = g.vertices.length; i < l; i++) {
        g.vertices[i].y = 10 * Math.sin(i / 5 + (tt + i) / 7)
      }
      g.verticesNeedUpdate = true
    }
  }
}
</script>
