<template>
    <section class="vue-phenomenon">
        <canvas ref="canvas" />
        <slot name="vertex" />
        <slot name="fragment" />
    </section>
</template>

<script>
import get from 'lodash/get'

export default {
    props: {
        options: { type: Object, default: () => {} },
        instances: { type: Array, default: () => [] },
        allowAlpha: { type: Boolean, default: true },
    },
    data() {
        return {
            phenom: null,
            addedKeys: [],
        }
    },
    async mounted() {
        if (!window) return

        if (!window.Phenomenon) {
            window.Phenomenon = (await import('phenomenon')).default
        }

        const opts = this.options || {}

        this.phenom = new window.Phenomenon({
            canvas: this.$refs.canvas,
            ...opts,

            context: {
                ...(opts.context || {}),
            },

            settings: {
                devicePixelRatio: window.devicePixelRatio,
                ...(opts.settings || {}),
            },
        })

        // add transparency
        if (this.allowAlpha) {
            this.phenom.gl.enable(this.phenom.gl.BLEND)
            this.phenom.gl.blendFunc(
                this.phenom.gl.SRC_ALPHA,
                this.phenom.gl.ONE_MINUS_SRC_ALPHA
            )
        }

        // done mounting phenomenon
        this.$emit('initialized', this.phenom)

        this.refreshInstances()
    },
    methods: {
        refreshInstances() {
            if (!this.phenom) {
                // eslint-disable-next-line
                console.warn('Phenomenon not initialized.')
                return
            }

            // TODO: hard refresh
            // if (hardRefresh) {
            // }

            for (let instance of this.instances) {
                // already added, ignore
                if (this.addedKeys.includes(instance.key)) {
                    continue
                }

                const vertex =
                    get(instance, 'settings.vertex', null) ||
                    get(this.$slots, 'vertex[0].children[0].text', '')
                const fragment =
                    get(instance, 'settings.fragment', null) ||
                    get(this.$slots, 'fragment[0].children[0].text', '')

                const args = [
                    instance.key,
                    {
                        vertex,
                        fragment,
                        ...instance.settings,
                    },
                ]
                this.phenom.add(...args)

                this.$emit('added', args)
            }
        },
    },
}
</script>

<style lang="scss">
.vue-phenomenon {
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    width: 100%;
    height: 100%;

    canvas {
        position: absolute;
        top: 0;
        right: 0;
        bottom: 0;
        left: 0;
        width: 100%;
        height: 100%;
    }
}
</style>
