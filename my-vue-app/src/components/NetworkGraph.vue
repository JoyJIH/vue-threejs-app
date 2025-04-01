<template>
  <div ref="container" class="network-graph"></div>
</template>

<script>
import * as THREE from 'three';

export default {
  name: 'NetworkGraph',
  mounted() {
    this.initGraph();
  },
  methods: {
    initGraph() {
      const scene = new THREE.Scene();
      const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
      const renderer = new THREE.WebGLRenderer({ antialias: true });
      renderer.setSize(window.innerWidth, window.innerHeight);
      this.$refs.container.appendChild(renderer.domElement);

      // Beispiel für Knoten und Kanten
      const nodes = [
        { position: new THREE.Vector3(1, 1, 1) },
        { position: new THREE.Vector3(-1, -1, 1) },
        { position: new THREE.Vector3(1, -1, -1) },
        { position: new THREE.Vector3(-1, 1, -1) },
      ];

      const edges = [
        { start: nodes[0].position, end: nodes[1].position },
        { start: nodes[1].position, end: nodes[2].position },
        { start: nodes[2].position, end: nodes[3].position },
        { start: nodes[3].position, end: nodes[0].position },
      ];

      // Knoten erstellen
      nodes.forEach(node => {
        const geometry = new THREE.SphereGeometry(0.1, 16, 16);
        const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
        const sphere = new THREE.Mesh(geometry, material);
        sphere.position.copy(node.position);
        scene.add(sphere);
      });

      // Kanten erstellen
      edges.forEach(edge => {
        const geometry = new THREE.BufferGeometry().setFromPoints([edge.start, edge.end]);
        const material = new THREE.LineBasicMaterial({ color: 0xffffff });
        const line = new THREE.Line(geometry, material);
        scene.add(line);
      });

      camera.position.z = 5;

      const animate = function () {
        requestAnimationFrame(animate);
        renderer.render(scene, camera);
      };

      animate();
    },
  },
};
</script>

<style scoped>
.network-graph {
  width: 100%;
  height: 100vh;
}
</style>