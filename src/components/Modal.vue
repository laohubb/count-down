<template>
    <transition name="modal">
        <div  v-if="visible" class="modal-mask" @click="cancel">
            <div class="modal-wrapper" >
                <div class="modal-container" @click.stop>
                    <div class="modal-header">
                        <slot name="header">default header</slot>
                    </div>
                    <div class="modal-body">
                        <slot name="body">default body</slot>
                    </div>
                    <div class="modal-footer">
                        <slot name="footer">
                            <button class="modal-default-button" @click="$emit('close')">OK</button>
                        </slot>
                    </div>
                </div>
            </div>
        </div>
    </transition>
</template>

<script setup>
import { defineProps } from 'vue';
import { defineEmits } from 'vue'

const props = defineProps({
    visible: {
        type: Boolean
    }
})
const emits = defineEmits(['update:visible','close'])

function cancel() {
    emits('update:visible', false)
}

</script>


<style scoped>


.modal-mask {
    position: fixed;
    z-index: 9998;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, .5);
    display: table;
    transition: opacity .3s ease;
}

.modal-wrapper {
    display: table-cell;
    vertical-align: middle;
}

.modal-container {
    width: 500px;
    margin: 0px auto;
    padding: 20px 30px;
    background-color: #fff;
    border-radius: 2px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, .33);
    transition: all .3s ease;
    font-family: Helvetica, Arial, sans-serif;
}

.modal-header {
    padding: 10px 0;
}

.modal-body {
    padding: 10px 0;
}

.modal-footer {
    padding: 10px;
    text-align: right;
}

.modal-default-button {
    border-radius: 2px;
    padding: 8px 14px;
    background-color: #42b983;
    border: 1px solid #42b983;
    color: #fff;
    font-size: 15px;
    cursor: pointer;
}
</style>