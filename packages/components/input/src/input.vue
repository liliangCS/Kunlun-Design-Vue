<template>
    <div v-if="type !== 'textarea'" class="kl-input" :class="{ 'kl-input_suffix': showSuffix }">
        <input
            class="kl-input_inner"
            :class="[{ 'is-disabled': disabled }, size]"
            :placeholder="placeholder"
            :type="showPassword ? (passwordVisible ? 'text' : 'password') : type"
            :name="name"
            :value="modelValue"
            @input="handleInput"
            :disabled="disabled"
            :style="inputStyle"
            :readonly="readonly"
            :max="max"
            :min="min"
            :step="step"
            :autofocus="autofocus"
            :form="form"
            ref="input"
        />
        <span class="kl-input_suffix">
            <KlOtherError
                class="on-input_icon kl-icon-cancel"
                color="#000"
                :size="suffixIconSize - 8"
                v-if="clearable && modelValue"
                @click="clear"
            />
            <Component
                class="on-input_icon kl-icon-visible"
                color="#000"
                :size="suffixIconSize"
                v-if="showPassword && type === 'password'"
                @click="handlePassword"
                :is="passwordVisible ? 'KlSystemEyeClose' : 'KlOtherEyeOpenFill'"
            />
        </span>
    </div>
    <div v-else class="kl-input" :class="{ 'kl-input_suffix': showSuffix }">
        <textarea
            class="kl-textarea_inner"
            :class="{ 'is-disabled': disabled }"
            :placeholder="placeholder"
            :name="name"
            :value="modelValue"
            @input="handleInput"
            :disabled="disabled"
            :rows="rows"
            :style="inputStyle"
            :readonly="readonly"
            :autofocus="autofocus"
            :form="form"
        />
    </div>
</template>

<script setup lang="ts">
import { createNamespace } from '@kunlun-design/utils'
import { computed, ref } from 'vue'
import { KlOtherError } from '@kl-design/icons'
import './input.scss'

defineOptions({
    name: 'KlInput'
})

const emit = defineEmits(['update:modelValue', 'change', 'clear'])

const props = defineProps({
    placeholder: {
        type: String,
        default: ''
    },
    type: {
        type: String,
        default: 'text'
    },
    name: {
        type: String,
        default: ''
    },
    disabled: {
        type: Boolean,
        default: false
    },
    modelValue: {},
    clearable: {
        type: Boolean,
        default: false
    },
    readonly: {
        type: Boolean,
        default: false
    },
    showPassword: {
        type: Boolean,
        default: false
    },
    rows: {
        type: Number,
        default: 4
    },
    max: {},
    min: {},
    step: {},
    autofocus: {
        type: Boolean,
        default: false
    },
    form: {
        type: String
    },
    inputStyle: {
        type: Object
    },
    size: {
        type: String,
        default: 'default'
    }
})

const input = ref(null)

// icon后缀
const showSuffix = computed(() => props.clearable || props.showPassword)
const suffixIconSize = computed(() => {
    switch (props.size) {
        case 'default':
            return 24
        case 'small':
            return 20
        default:
            return 28
    }
})

const passwordVisible = ref(false)

// 数据修改回调
const handleInput = (e: { target: { value: any } }) => {
    emit('update:modelValue', e.target.value)
    emit('change', e.target.value)
}

// 清空input
const clear = (e: { target: { value: any } }) => {
    emit('update:modelValue', '')
    emit('clear')
}

// 是否显示密码
const handlePassword = () => {
    passwordVisible.value = !passwordVisible.value
}
const { n } = createNamespace('input')
</script>
