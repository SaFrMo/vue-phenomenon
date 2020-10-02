<template>
    <vue-phenomenon :instances="[plane]">
        <!-- vertex -->
        <template v-slot:vertex>
            <script type="x-shader/vertex">
                attribute vec3 aPosition;
                uniform mat4 uProjectionMatrix;
                uniform mat4 uModelMatrix;
                uniform mat4 uViewMatrix;

                varying vec2 vUv;

                void main(){
                    vec3 offset = vec3(0., 0., 1.5);
                    vec4 pos = vec4(aPosition.xy - vec2(0.5), aPosition.z, 1.0) + vec4(offset, 0.);
                    gl_Position = uProjectionMatrix * uModelMatrix * uViewMatrix * pos;
                    vUv = aPosition.xy;
                }
            </script>
        </template>

        <!-- fragment -->
        <template v-slot:fragment>
            <script type="x-shader/fragment">
                precision highp float;
                varying vec2 vUv;

                void main(){
                    float c = pow(1. - distance(vUv, vec2(0.5)), 5.);
                    gl_FragColor = vec4(c, 1., c, c);
                }
            </script>
        </template>
    </vue-phenomenon>
</template>

<script>
import VuePhenomenon from '../src/VuePhenomenon'

const size = 1

export default {
    components: {
        'vue-phenomenon': VuePhenomenon,
    },
    data() {
        return {
            plane: {
                key: 'plane',
                settings: {
                    multiplier: 1,
                    geometry: {
                        vertices: [
                            { x: size, y: size, z: 0 },
                            { x: size, y: 0, z: 0 },
                            { x: 0, y: size, z: 0 },
                            { x: size, y: 0, z: 0 },
                            { x: 0, y: 0, z: 0 },
                            { x: 0, y: size, z: 0 },
                        ],
                    },
                    mode: 4,
                },
            },
        }
    },
}
</script>
