<template>
    <div class="p-grid-layout" :style="containerStyle">
        <div v-for="(item, index) in items"
             :key="index"
             :class="cardClass(item, index)"
             :style="cardStyle(item, index)"
             v-on="$listeners"
        >
            <slot name="card" :item="item" :index="index">
                {{ item }}
            </slot>
        </div>
    </div>
</template>

<script lang="ts">
import { reactive, computed, toRefs } from '@vue/composition-api';

import type { GridLayoutProps } from '@/others/deprecated/grid-layout/type';

export default {
    name: 'PGridLayout',
    components: {
    },
    props: {
        cardMinWidth: {
            type: String,
            default: '12rem',
        },
        cardMaxWidth: {
            type: String,
            default: '1fr',
        },
        cardHeight: {
            type: String,
            default: '20rem',
        },
        rowGap: {
            type: String,
            default: '1rem',
        },
        fixColumn: {
            type: Number,
            default: null,
        },
        columnGap: {
            type: String,
            default: '1rem',
        },
        cardClass: {
            type: Function,
            default: () => ['card-item'],
        },
        cardStyle: {
            type: Function,
            default: () => ({}),
        },
        items: {
            type: Array,
            default: () => [],
        },
        loading: {
            type: Boolean,
            default: false,
        },
        selectItem: {
            type: String,
            default: '',
        },
    },
    setup(props: GridLayoutProps) {
        const state = reactive({
            containerStyle: computed(() => ({
                display: 'grid',
                'grid-template-columns': `repeat(${props.fixColumn || 'auto-fill'}, minmax(${props.cardMinWidth}, ${props.cardMaxWidth}))`,
                'grid-auto-rows': props.cardHeight,
                'row-gap': props.rowGap,
                'column-gap': props.columnGap,
            })),
        });
        return {
            ...toRefs(state),
        };
    },
};
</script>

<style lang="postcss">
.p-grid-layout {
    .icon {
        @apply inline-block;
    }

    .card-item {
        @apply bg-white border border-gray-200 rounded-sm;
        cursor: pointer;
        &:hover {
            @apply border-l border-secondary;
            cursor: pointer;
        }
    }
}
</style>
