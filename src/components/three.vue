<template>
  <div id="scene" ref="sc">

  </div>
</template>

<script>
/* eslint-disable max-len */
/* eslint-disable no-unused-vars */
/* eslint-disable no-undef */
// eslint-disable-next-line no-unused-vars

// import * as THREE from 'three';
import Globe from 'globe.gl';
/*
- https://discoverthreejs.com/book/first-steps/app-structure/
- https://threejs.org/docs/index.html#manual/en/introduction/Creating-a-scene
- https://github.com/mrdoob/three.js
*/
// Get a reference to the container element that will hold our scene

export default {
  props: [
    'lat', 'lon',
  ],
  mounted() {
    const container = this.$refs.sc;
    /*
    console.log('width: ', container.offsetWidth);

    // create a Scene
    const scene = new THREE.Scene();

    // Set the background color
    scene.background = new THREE.Color(1, 1, 1);

    // Create a camera
    const fov = 35; // AKA Field of View
    const aspect = container.offsetWidth / container.offsetHeight;
    const near = 0.1; // the near clipping plane
    const far = 100; // the far clipping plane

    const camera = new THREE.PerspectiveCamera(fov, aspect, near, far);

    // every object is initially created at ( 0, 0, 0 )
    // move the camera back so we can view the scene
    camera.position.set(0, 0, 10);

    // create a geometry
    const geometry = new THREE.BoxBufferGeometry(2, 2, 2);
    const geo = new THREE.SphereGeometry(2, 40, 40);
    // create a default (white) Basic material
    const material = new THREE.MeshBasicMaterial();
    material.wireframe = true;
    material.color = new THREE.Color(1, 0, 0);

    // create a Mesh containing the geometry and material
    const cube = new THREE.Mesh(geo, material);

    // add the mesh to the scene
    scene.add(cube);

    // create the renderer
    const renderer = new THREE.WebGLRenderer();

    // next, set the renderer to the same size as our container element
    renderer.setSize(container.offsetWidth, container.offsetHeight); // container.clientWidth, container.clientHeight);

    // finally, set the pixel ratio so that our scene will look good on HiDPI displays
    renderer.setPixelRatio(window.devicePixelRatio);

    // add the automatically created <canvas> element to the page
    container.append(renderer.domElement);

    // render, or 'create a still image', of the scene
    renderer.render(scene, camera);
  */
    const data = [{
      lat: '',
      lng: '',
      size: 4,
      // pointLabel: 'location',
    }];

    data[0].lat = this.lat;
    data[0].lng = this.lon;

    const myglobe = Globe();
    myglobe.width(container.offsetWidth);
    myglobe.height(container.offsetHeight);
    myglobe.globeImageUrl('//unpkg.com/three-globe/example/img/earth-night.jpg');
    myglobe(container).pointsData(data);
    myglobe.pointsData(data);
  },
  methods: {
    three() {

    },
  },
};
</script>
