<template>
    <Lunchbox
        background="black"
        :cameraPosition="[5, 2, 5]"
        :cameraLook="[0, 0.5, 0]"
    >
        <group :rotation-y="now * -0.00001">
            <pointLight
                v-for="i in 10"
                :key="i"
                :color="`hsl(${360 * (i / 10)}, 100%, 90%)`"
                :position="[
                    Math.sin(now * (0.001 + i * 0.00015) + i) *
                        (i * 0.5 * Math.sin(now * 0.001) + 0.5),
                    1,
                    Math.cos(now * (0.001 + i * 0.00025) + i) *
                        (i * 0.5 * Math.sin(now * 0.001) + 1.5),
                ]"
            />

            <mesh :rotation-x="Math.PI * -0.5" :scale="10">
                <planeGeometry />
                <meshPhysicalMaterial>
                    <textureLoader src="/tableDiff.jpg" attach="map" />
                    <textureLoader src="/tableNormal.jpg" attach="normalMap" />
                    <textureLoader
                        src="/tableRoughness.jpg"
                        attach="roughnessMap"
                    />
                </meshPhysicalMaterial>
            </mesh>
        </group>
    </Lunchbox>
</template>

<script setup lang="ts">
import { onBeforeRender, renderer } from 'lunchboxjs'
import { ref, watch } from 'vue'

watch(
    renderer,
    (r) => {
        if (!r) return
        r.physicallyCorrectLights = true
    },
    { immediate: true }
)

const now = ref(Date.now())

onBeforeRender(() => {
    now.value = Date.now()
})
</script>