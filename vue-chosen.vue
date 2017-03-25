<template>
    <select :data-placeholder="placeholder" :multiple="multiple" ref="select">
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
                default: []
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
            }
        },

        computed: {
            customOptions() {
                let vm = this,
                    options = [];

                Object.keys(this.options).forEach(function(key) {
                    options.push({
                        'id': key,
                        'label': vm.options[key]
                    });
                });

                return this.allowEmpty ? [{id: null, label: ''}].concat(options) : options;
            },

            localValue() {
                this.$nextTick(function() {
                    $(this.$el).val(this.value).trigger("chosen:updated");
                });

                return this.value;
            }
        },

        watch: {
            localValue() {
            },

            customOptions() {
                this.$nextTick(function() {
                    $(this.$el).val(this.value).trigger("chosen:updated");
                });
            }
        },

        mounted() {
            let component = this;

            $(this.$el).chosen({
                width: "100%",
                disable_search_threshold: this.searchable ? this.searchableMin : 100000
            }).change(function($event) {
                component.$emit('input', $($event.target).val());
            });
        }
    }
</script>
