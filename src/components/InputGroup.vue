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
            :class="{
                'is-invalid': !isValid,
                'form-control': true,
            }"
            v-model="inputVal"
            @input='$emit("update:inputVal",inputVal)'
        >
        <span
            v-show="!isdValid"
            class="invalid-feedback"
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
         inputVal: {
            type: String,
            default: '',
        },
    },
    computed: {
        isValid() {
            return !this.errorMessage;
        },
        
    },
}
</script>

};