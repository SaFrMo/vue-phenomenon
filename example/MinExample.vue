<template>
    <vue-phenomenon :instances="[cube]">
        <!-- vertex -->
        <template v-slot:vertex>
            <script type="x-shader/vertex">
                attribute vec3 aLocation;

                uniform mat4 uProjectionMatrix;
                uniform mat4 uModelMatrix;
                uniform mat4 uViewMatrix;

                void main(){
                    gl_Position = uProjectionMatrix * uModelMatrix * uViewMatrix * vec4(aLocation, 1.0);
                    gl_PointSize = 2.;
                }
            </script>
        </template>

        <!-- fragment -->
        <template v-slot:fragment>
            <script type="x-shader/fragment">
                precision highp float;

                void main(){
                    gl_FragColor = vec4(1., 0., 0., 1.);
                }
            </script>
        </template>
    </vue-phenomenon>
</template>

<script>
import VuePhenomenon from '../src/VuePhenomenon'

export default {
    components: {
        'vue-phenomenon': VuePhenomenon,
    },
    data() {
        return {
            cube: {
                key: 'cube',
                settings: {
                    multiplier: 5000,
                    attributes: [
                        {
                            name: 'aLocation',
                            data: () => [
                                Math.random() - 0.5,
                                Math.random() - 0.5,
                                Math.random() - 0.5,
                            ],
                            size: 3,
                        },
                    ],
                },
            },
        }
    },
}
</script>
