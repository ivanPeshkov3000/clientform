<template lang="pug">
    .input-validate
        label(:for="inputItem.name") {{inputItem.title}}
        input(
            :id="inputItem.name"
            v-model.trim="$v[inputItem.name].$model"
            :type="inputItem.type"
        )
        div.notice-valid
        //- span.notice-valid-ator(v-for="validator in validators" :class="{'invalid': validator.invalid}") {{validator.title}}
</template>

<script>
import * as vuelidate from 'vuelidate/lib/validators'
export default {
    props: {
        inputItem: {
            type: Object,
            required: true
        }
    },
    data() {
        return {
            noticeMessages: {
                type: "",
                message: ""
            }
        }
    },
    computed: {
        noticeInvalid: v => (
            v
        ),
        inputvalid() {
            return ({[this.inputItem.name]: function() {
                    const obj = {}
                    for(let value of this.inputItem.validators) {
                        if (typeof value == 'string') obj[value] = vuelidate[value]
                        if (Array.isArray(value)) obj[value[0]] = vuelidate[value[0]](value[1])
                    }
                    return obj
                }
            })
        }
    },
    validations: {
        this.inputvalid
    }
}
</script>