<template lang="pug">
    .input-validate
        label(:for="item.name") {{item.title}}
        input(
            :id="item.name"
            v-if="item.type=='text'||'checkbox'||'radio'"
            v-model.trim="$v[item.name].$model"
            :type="item.type"
            @input="sendData"
        )
        select(
            :id="item.name"
            v-else-if="item.type=='select'||'multiselect'"
            v-model.trim="$v[item.name].$model"
            :type="item.type"
            :multiple="item.type=='multiselect'"
            @input="sendData"
        )
            option(
                v-for="opt of item.options"
            ) {{opt}}
        div.notice-valid(
            v-show="$v.$error"
            tabindex="0"
        )
            span.notice-validator(
                v-for="notice in noticeValidator" :class="{'invalid': notice.invalid}"
            ) {{notice.message}}
</template>

<script>
import * as vuelidators from 'vuelidate/lib/validators'

export default {
    props: {
        item: {
            type: Object,
            required: true
        }
    },
    data() {
        return {
            [this.item.name]: ""
        }
    },
    computed: {
        noticeValidator: function() {
            const notices = {
                required: "Обязательное поле",
                alpha: "Только буквы!",
                numeric: "Только цифры!"
            }
            const valid = this.item.validators
            const obj = {}
            for (let validator in valid) {
                obj[validator] = {}
                switch (validator) {
                    case 'required':
                    case 'numeric':
                    case 'alpha':
                        obj[validator].message = notices[validator]
                        break;
                    case 'minLength':
                        obj[validator].message = `Минимум ${this.$v[this.item.name].$params.minLength.min} символов`
                        break;
                }
                obj[validator].invalid = !this.$v[this.item.name][validator]
            }
            return obj
        }
    },
    methods: {
        sendData: function() {
            let value = [[this.item.name], this.$v[this.item.name]]
            this.item.listener(value)
        }
    },
    validations() {
        const valid = this.item.validators
        const obj = {}
        for (let val in valid) {
            if (typeof valid[val] == "boolean") obj[val] = vuelidators[val]
            if (typeof valid[val] == "number") obj[val] = vuelidators[val](valid[val])
        }
        return { [this.item.name]: obj }
    }
}
</script>

<style lang="scss" scoped>
    .input-validate {
        display: inline-grid;
        grid-template-columns: 2fr 3fr 1fr;
        align-items: center;
        grid-gap: .5em;
        position: relative;
        input[type="text"] {
            height: 30px;
            border: 0;
            background: none;
            font-size: 14px;
            border-bottom: 2px solid rgba($color: #000000, $alpha: .3);
            outline: none;
            &:focus {
            border-bottom-color: rgba($color: #000000, $alpha: .6);
            }
        }
        .notice-valid {
            &::before {
                content: "\26A0";
                font-size: 40px;
                color: red;
            }
            * {
                visibility: hidden;
            }
            position: absolute;
            right: -120px;
            text-align: left;
            z-index: -1;
            
            .notice-validator {
                display: list-item;
                margin-left: 15px;
                margin-right: 5px;
                font-size: 12px;
                list-style-type: "‎\2714  ";
                &.invalid {
                    list-style-type: "\2718  ";
                    color: red;
                }
            }
        }
        input:focus~.notice-valid {
            min-width: max-content;
            background-color: #fff;
            border: 1px solid grey;
            border-radius: 3px;
            padding: 2px;
            padding-left: 10px;
            z-index: 5;
            * {
                visibility: visible;
            }
        }
    }
</style>