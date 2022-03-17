<template>
    <Lunchbox background="white">
        <!-- spheres -->
        <Sphere
            v-for="(item, i) in width * height"
            :key="i"
            :position="[
                (i % width) - width * 0.5,
                Math.floor(i / width) - height * 0.5,
                -10,
            ]"
        />

        <!-- background -->
        <mesh :position-z="-15" :scale="50">
            <planeGeometry />
            <meshLambertMaterial color="white" />
        </mesh>

        <!-- light -->
        <pointLight :intensity="0.05" />
    </Lunchbox>
</template>

<script lang="ts" setup>
import Sphere from './Sphere.vue'
import { setCustomRender } from 'lunchboxjs'
import * as THREE from 'three'

const width = 20
const height = 10

// postprocessing
import { EffectComposer } from 'three/examples/jsm/postprocessing/EffectComposer'
import { RenderPass } from 'three/examples/jsm/postprocessing/RenderPass'
import { UnrealBloomPass } from 'three/examples/jsm/postprocessing/UnrealBloomPass'

let composer: EffectComposer | null = null

setCustomRender((opts) => {
    const canvas = opts.renderer?.domElement

    // ignore if no canvas
    if (!canvas || !opts.scene || !opts.camera) return

    // setup effect composer if needed
    if (!composer) {
        composer = new EffectComposer(opts.renderer as THREE.WebGLRenderer)

        const renderPass = new RenderPass(opts.scene, opts.camera)
        composer.addPass(renderPass)

        const bloomPass = new UnrealBloomPass(
            new THREE.Vector2(canvas.width, canvas.height),
            0.55,
            0.6,
            0.1
        )
        composer.addPass(bloomPass)
    }

    composer.render()
})
</script>