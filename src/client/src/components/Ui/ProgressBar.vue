<template>
    <div class="progress-bar">
        <span class="progress-bar__label">{{ label }}</span>
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
    margin-bottom: 24px;
    &__label {
        font-size: 1.5rem;
    }
    &__track {
        position: relative;
        display: flex;
        justify-content: flex-start;
        align-items: center;
        border-radius: 40px;
        padding: 10px;
        margin-top: 18px;
        height: 4rem;
        background: rgba(255,255,255,0.1);

        &-value {
            border-radius: 40px;
            background: #fff;
            height: 100%;
            width: 0;
            color: #333;

            span {
                position: absolute;
                top: 50%;
                left: 2rem;
                transform: translateY(-50%);
                font-size: 2rem;
                font-weight: 500;
            }
        }
    }
}
</style>