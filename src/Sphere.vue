<template>
    <group>
        <mesh
            @pointerOver="
                () => {
                    targetIntensity = 1
                    intensity = 1
                }
            "
            @pointerOut="targetIntensity = 0"
        >
            <sphereGeometry :args="[0.45]" />
            <meshLambertMaterial
                :emissiveIntensity="intensity"
                :emissive="color"
            />
        </mesh>

        <pointLight
            :color="color"
            :intensity="intensity"
            :distance="2"
            :visible="intensity > epsilon"
        />
    </group>
</template>

<script lang="ts" setup>
import { onBeforeRender } from 'lunchboxjs'
import { ref } from 'vue'

const lerp = (v0: number, v1: number, t: number) => {
    return v0 * (1 - t) + v1 * t
}

const targetIntensity = ref(0)
const intensity = ref(0)
const epsilon = 0.001
const color = `rgb(${Math.floor(Math.random() * 255)}, ${Math.floor(
    Math.random() * 255
)}, ${Math.floor(Math.random() * 255)})`

onBeforeRender(() => {
    intensity.value = lerp(intensity.value, targetIntensity.value, 0.08)
})
</script>