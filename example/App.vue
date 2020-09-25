<template>
    <section class="example">
        <div class="single">
            <min-example />
        </div>

        <div class="single">
            <vue-phenomenon :instances="[{ key: 'cube', settings: cube }]">
                <!-- vert -->
                <template v-slot:vertex>
                    <script type="x-shader/vertex">
                        attribute vec3 aPosition;
                        attribute vec3 aOffset;

                        uniform mat4 uProjectionMatrix;
                        uniform mat4 uModelMatrix;
                        uniform mat4 uViewMatrix;
                        uniform float uTime;
                        varying vec4 vColor;

                        void main(){
                            vec3 finalPos = aPosition + aOffset + vec3(0., 0., cos(aOffset.x + uTime) * 0.5);
                            gl_Position = uProjectionMatrix * uModelMatrix * uViewMatrix * vec4(finalPos, 1.0);
                            gl_PointSize = 5.;
                            vColor = mix(vec4(0., 0., 0., 1.), vec4(1.), 0.35 - finalPos.z);
                        }
                    </script>
                </template>

                <!-- frag -->
                <template v-slot:fragment>
                    <script type="x-shader/fragment">
                        precision highp float;
                        varying vec4 vColor;

                        void main(){
                            gl_FragColor = vColor;
                        }
                    </script>
                </template>
            </vue-phenomenon>
        </div>

        <div class="single">
            <uv-example />
        </div>
    </section>
</template>

<script>
/* eslint-disable */
import VuePhenomenon from '../src/VuePhenomenon'
import MinExample from './MinExample'
import UvExample from './UvExample'

let lastTime = Date.now()
export default {
    components: {
        'vue-phenomenon': VuePhenomenon,
        'min-example': MinExample,
        'uv-example': UvExample,
    },
    data() {
        return {
            radius: 3,
            cube: {
                multiplier: 5000,
                attributes: [
                    {
                        name: 'aOffset',
                        data: () => [this.rand(), this.rand(), 0],
                        size: 3,
                    },
                ],
                uniforms: {
                    uTime: {
                        value: 0.0,
                        type: 'float',
                    },
                },
                onRender: (instance) => {
                    instance.uniforms.uTime.value +=
                        (Date.now() - lastTime) / 500
                    lastTime = Date.now()
                },
            },
        }
    },
    methods: {
        rand() {
            return Math.random() * this.radius - this.radius / 2
        },
    },
}
</script>

<style lang="scss">
.example {
    display: grid;
    grid-template-columns: repeat(3, 250px);
    grid-gap: 20px;
    justify-content: center;
    grid-auto-rows: 250px;

    .single {
        position: relative;
        border: 1px solid black;
    }
}
</style>
