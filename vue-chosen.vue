<template>
    <select :data-placeholder="placeholder" :multiple="multiple" :disabled="disabled">
        <option v-for="option in customOptions" v-bind:value="option.id">
            {{ option.label }}
        </option>
    </select>
</template>

<script>
    export default {
        props: {
            value: {
                type: [String, Number, Array, Object],
                default: null
            },
            options: {
                type: [Array, Object],
                default: Object
            },
            multiple: {
                type: Boolean,
                default: false
            },
            placeholder: {
                type: String,
                default: 'Select'
            },
            searchable: {
                type: Boolean,
                default: true
            },
            searchableMin: {
                type: Number,
                default: 1
            },
            allowEmpty: {
                type: Boolean,
                default: true
            },
            allowAll: {
                type: Boolean,
                default: false
            },
            disabled: {
                type: Boolean,
                default: false
            },
            onValueReturn: {
                type: Object,
                default: () => {
                    return {}
                }
            }
        },

        computed: {
            customOptions() {
                let vm = this,
                    options = [];

                if (this.allowAll) {
                    options.push({
                        'id': '-1',
                        'label': 'All'
                    });
                }

                Object.keys(this.options).forEach(function (key) {
                    options.push({
                        'id': key,
                        'label': vm.options[key]
                    });
                });

                return this.allowEmpty ? [{id: null, label: ''}].concat(options) : options;
            },

            localValue() {
                let value = this.allowAll && this.value === null ? '-1' : this.value

                this.$nextTick(function () {
                    $(this.$el).val(value).trigger("chosen:updated");
                });

                return value;
            }
        },

        watch: {
            localValue() {
            },

            customOptions() {
                this.$nextTick(function () {
                    let value = this.allowAll && this.value === null ? '-1' : this.value
                    $(this.$el).val(value).trigger("chosen:updated");
                });
            }
        },

        mounted() {
            let component = this;

            $(this.$el).chosen({
                width: "100%",
                disable_search_threshold: this.searchable ? this.searchableMin : 100000
            }).change(function ($event) {
                const value = $($event.target).val()
                if (typeof component.onValueReturn[value] !== 'undefined') {
                    return component.$emit('input', component.onValueReturn[value]);
                }
                if (component.allowAll && $($event.target).val() === '-1') {
                    return component.$emit('input', null);
                }
                component.$emit('input', $($event.target).val());
            });
        }
    }
</script>
