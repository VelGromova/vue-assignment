<template>
    <div class="progress-bar">
        <h3 class="progress-bar__label">{{ label }}</h3>
        <div class="progress-bar__track">
            <div class="progress-bar__track-value" :style="{'width' : value + '%'}">
                <span>{{ value }} {{ unit }}</span>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        label: {
            type: String,
            default: '',
        },
        value: {
            type: Number,
            required: true,
        },
        unit: {
            type: String,
            required: true,
        }
    }
}
</script>

<style lang="scss">
.progress-bar {
    margin-bottom: var(--space-lg);
    &__label {
        font-size: var(--text-lg);
    }
    &__track {
        position: relative;
        display: flex;
        justify-content: flex-start;
        align-items: center;
        border-radius: rem(40);
        padding: rem(10);
        margin-top: rem(18);
        height: rem(80);
        background: rgba(255,255,255,0.1);

        &-value {
            border-radius: rem(40);
            background: var(--color-white);
            height: 100%;
            width: 0;
            color: var(--color-grey);

            span {
                position: absolute;
                top: 50%;
                left: rem(32);
                transform: translateY(-50%);
                font-size: var(--text-lg);
                font-weight: var(--bold-font-weight);
            }
        }
    }
}
</style>