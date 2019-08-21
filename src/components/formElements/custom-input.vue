<template>
    <div class="field-wrap">
        <div class="input-wrap">
            <label :for="name">{{ label }}</label>
            <input v-validate="validate" :type="type" :name="name">
        </div>
        <div class="error">
            {{ errors.first(name) }}
        </div>
    </div>
</template>

<script>
    export default {
        props: ['validate', 'type', 'name', 'label'],
    }
</script>

<style scoped lang="scss">
    .field-wrap {
        display: flex;
        justify-content: flex-start;
        flex-direction: column;

        .input-wrap {
            display: flex;
            align-items: center;
            flex-direction: row;
        }
    }


    label {
        margin-right: 5px;
    }

    .error {
        margin-top: 5px;
        color: darkred;
        text-align: left;
        font-size: 12px;
    }
</style>
