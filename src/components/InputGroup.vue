<template>
    <div class="form-group">
        <label
            :for="id"
        >
            {{ label }}
        </label>
        <input
            :id="id"
            :name="id"
            :type="type"
            v-model="inputVal"
            @input="changeHandler"
            class="input_style"
        >
        <span
            v-show="isEvaluated && !isValid"
        >
            {{ errorMessage }}
        </span>
    </div>
</template>
<script>
export default {
    name: 'input-group',
    data() {
        return {
            errorMessage: "*This field is required",
            inputVal: '',
            isEvaluated: false,
            isValid: false
        }
    },
    props: {
        id: {
            type: String,
            required: true,
        },
        label: {
            type: String,
            required: true,
        },
        type: {
            type: String,
            required: true,
        },
    },
    methods: {
        changeHandler() {
            this.isEvaluated = true;
            this.$emit("inputEvent", this.inputVal)
            if(this.inputVal) {
                this.isValid = true;
            } else {
                this.isValid = false
            }
        },
        resetValue() {
            this.inputVal = '';
            this.isEvaluated = false;
            this.isValid = false
        }
    },
}
</script>

};