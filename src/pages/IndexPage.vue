<template>
    <q-page
        v-touch-pan.vertical.prevent.mouse="handlePan"
        class="flex flex-center text-white"
    >
        <div class="row">
            <q-input
                v-model="data.name"
                input-class="text-center text-h5 text-white"
                filled 
                color="teal"
                placeholder="Placeholder"
            />
        </div>

        <div class="row full-width item-center">
            <div class="col text-center">
                <q-btn
                    @click="decreaseCounter"
                    round 
                    icon="remove"
                    size="xl"
                />
            </div>

            <div class="col text-center text-h2">
                {{ data.counter }}
            </div>

            <div class="col text-center">
                <q-btn
                    @click="increaseCounter"
                    v-touch-repeat:300:300:300:300:50.mouse="increaseCounter"
                    round 
                    icon="add"
                    size="xl"
                />
            </div>
        </div>

        <div class="row">
            <q-btn
                @click="resetCounter"
                icon="restart_alt"
                round 
                size="xl"
            /> 
        </div>
    </q-page>
</template>


<style scoped>
    .q-page{
        max-width: 700px;
        margin: 0 auto;
    }
</style>
<script setup>
    // imports start
    import {reactive, watch} from 'vue'
    import { useQuasar } from 'quasar'

    const $q = useQuasar()

    // imports end

    // data start
    const data = reactive({
        counter: 0,
        name: ''
    })

    const savedData = $q.localStorage.getItem('data')
    if (savedData) Object.assign(data, savedData)

    watch(data, value => {
        $q.localStorage.set('data', value)
    })
    // data end

    // counter methods start
    const increaseCounter = () => {
        data.counter++
    }

    const decreaseCounter = () => {
        if(data.counter > 0) data.counter--
    }

    const resetCounter = () => {
        data.counter = 0
    }
    // counter methods end

    // Touch pan start
    const handlePan = e => {
        if(e.delta.y < 0)
            increaseCounter()
        else
            decreaseCounter()
    }
    // Touch pan end
</script>
