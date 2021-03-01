<template>
    <svg ref="svg" id="svg" xmlns="http://www.w3.org/2000/svg" :width="width + (margin * 2)" :height="height + (margin * 2)" :viewBox="`0 0 ${width + (margin * 2)} ${height + (margin * 2)}`" fill="grey">
        <filter id="shadow" x="0" y="0" width="200%" height="200%">
            <feDropShadow dx="0" dy="20" stdDeviation="10" flood-color="#000000" flood-opacity="0.4" />
        </filter>
        <rect :width="width + (margin * 2)" :height="height + (margin * 2)" fill="#fafafa"/>
        <g :transform="`translate(${margin}, ${margin})`">
            <rect :width="width" :height="height" fill="white"/>
            <rect :width="width" :height="height" rx="5" fill="#353B46" filter="url(#shadow)"/>
            <rect :width="width" :height="28.7" rx="5" fill="#EEF1F5"/>
            <circle cx="14.5" cy="14.5" r="5.5" fill="#EF534A"/>
            <circle cx="33.5" cy="14.5" r="5.5" fill="#EDC04A"/>
            <circle cx="52.5" cy="14.5" r="5.5" fill="#97D272"/>
            <text :x="width / 2" y="20" text-anchor="middle" fill="black">{{ filename }}</text>
            <foreignObject x="10" y="40" :width="width - (padding * 2)" :height="height - padding - 40">
                <div ref="code" xmlns="http://www.w3.org/1999/xhtml"></div>
            </foreignObject>
        </g>
    </svg>
</template>

<script lang="ts">
import { defineComponent, ref, onMounted } from "vue";
import { saveSvg } from '../utils/svg'

export default defineComponent({
    props: {
        code: {
            type: String,
            required: true
        },
        filename: {
            type: String,
            required: true
        }
    },
    setup(props) {
        const svg = ref<HTMLElement>()
        const code = ref<HTMLElement>()
        const width = ref(416)
        const height = ref(224)
        const margin = 64
        const padding = 10
        
        onMounted(() => {
            if (code.value) {
                code.value.innerHTML = props.code.replace('&nbsp;', '')
                width.value = Math.max(code.value.scrollWidth, 416) + (padding * 2)
                height.value = Math.max(code.value.scrollHeight, 224) + (padding + 40) 
            }
            
            const firstChild = code.value?.children[0] as HTMLElement
            if (firstChild) {
                firstChild.style.backgroundColor = ""
            }

            download()
        })

        function download() {
            if (svg.value) {
                saveSvg(svg.value, props.filename)
            }
        }

        return {
            svg,
            code,
            width,
            height,
            margin,
            padding
        }
    },
})
</script>