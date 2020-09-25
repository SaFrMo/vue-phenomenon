Bare-bones Vue component wrapping a [Phenomenon](https://github.com/vaneenige/phenomenon) renderer. Example:

```html
<template>
    <vue-phenomenon :instances="[cube]">
        <!-- vertex shader -->
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

        <!-- fragment shader -->
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
    // assuming you've already registered `vue-phenomenon` component
    export default {
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
```

## Props

| Name       | Type    | Default | Notes                                                                                                                                                                                                                    |
| ---------- | ------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| options    | Object  | `{}`    | Options to pass to Phenomenon constructor (see [docs](https://github.com/vaneenige/phenomenon#phenomenonoptions)).                                                                                                       |
| instances  | Array   | `[]`    | Instances for Phenomenon to create. Each instance should be in the form `{ key: 'unique-key', settings: {} }`. Each instanc will be passed to [Phenomenon.add](https://github.com/vaneenige/phenomenon#addkey-settings). |
| allowAlpha | Boolean | `true`  | Whether or not alpha should be respected in the created scene. If `false`, alpha values in the fragment shader other than 0 or 1 will be ignored.                                                                        |

## Events

| Name          | Params         | Notes                                                                                                                                              |
| ------------- | -------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| `initialized` | `(phenomenon)` | Fired when Phenomenon initialized. Passes the created Phenomenon object. Useful for accessing the webgl context of the created scene, for example. |
