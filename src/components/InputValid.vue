/* eslint-disable vue/no-side-effects-in-computed-properties */
<template lang="pug">
    .input-validate
        label(:for="item.name") {{item.title}}
        input(
            :id="item.name"
            v-model.trim="$v[item.name].$model"
            :type="item.type"
        )
        div.notice-valid(
            v-show="$v.$error"
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
        .notice-valid {
            position: absolute;
            min-width: max-content;
            text-align: left;
            .notice-validator {
                display: list-item;
                font-size: 12px;
                list-style-type: "‎\2714  ";
                &.invalid {
                    list-style-type: "\2718  ";
                    color: red;
                }
            }
        }
        &:focus .notice-valid {
            opacity: 1;
        }
    }
</style>