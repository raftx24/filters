<template>
    <core-date-filter v-bind="$attrs"
        v-on="$listeners">
        <template v-slot:default="{
                filters, filter, custom, minBindings, minEvents, maxBindings, maxEvents,
                direction, directionBindings, directionEvents, backEvents, filterEvents,
            }">
            <div class="date-filter is-paddingless">
                <div class="header has-text-centered has-background-light"
                    v-if="!compact">
                    <strong>
                        {{ displayLabel(custom) }}
                    </strong>
                </div>
                <div v-tooltip="compact ? displayLabel(custom) : null"
                    class="filter-wrapper"
                    :class="{ 'has-background-light': compact }">
                    <fade mode="out-in">
                        <div class="tags-wrapper has-text-centered"
                            key="tags"
                            v-if="!custom">
                            <div class="filter-tags">
                                <span v-if="direction">
                                    <vue-switch class="is-small direction"
                                        v-bind="directionBindings"
                                        v-on="directionEvents"/>
                                </span>
                                <span class="tag"
                                    :class="{ 'is-warning': filter === key }"
                                    v-for="(type, key) in filters"
                                    :key="key"
                                    v-on="filterEvents(key)">
                                    {{ i18n(type) }}
                                </span>
                            </div>
                        </div>
                        <div v-else
                            key="dates"
                            class="dates-wrapper animated">
                            <div class="columns is-mobile is-variable is-1 is-centered">
                                <div class="column is-narrow">
                                    <a class="button is-naked">
                                        <span class="icon is-small"
                                            v-on="backEvents">
                                            <fa icon="arrow-left"/>
                                        </span>
                                    </a>
                                </div>
                                <div class="column">
                                    <datepicker class="picker"
                                        :placeholder="i18n('From')"
                                        :alt-format="altFormat"
                                        :alt-input="altInput"
                                        v-bind="minBindings"
                                        v-on="minEvents"/>
                                </div>
                                <div class="column">
                                    <datepicker class="picker"
                                        :placeholder="i18n('To')"
                                        :alt-format="altFormat"
                                        :alt-input="altInput"
                                        v-bind="maxBindings"
                                        v-on="maxEvents"/>
                                </div>
                            </div>
                        </div>
                    </fade>
                </div>
            </div>
        </template>
    </core-date-filter>
</template>

<script>
import { library } from '@fortawesome/fontawesome-svg-core';
import { faArrowLeft } from '@fortawesome/free-solid-svg-icons';
import { VTooltip } from 'v-tooltip';
import { Datepicker } from '@enso-ui/datepicker/bulma';
import { Fade } from '@enso-ui/transitions';
import VueSwitch from '@enso-ui/switch/bulma';
import CoreDateFilter from '../renderless/CoreDateFilter.vue';

library.add(faArrowLeft);

export default {
    name: 'DateFilter',

    directives: { tooltip: VTooltip },

    components: {
        CoreDateFilter, Fade, Datepicker, VueSwitch,
    },

    props: {
        altFormat: {
            type: String,
            default: null,
        },
        altInput: {
            type: Boolean,
            defaut: false,
        },
        compact: {
            type: Boolean,
            default: false,
        },
        i18n: {
            type: Function,
            default: v => v,
        },
        name: {
            type: String,
            default: null,
        },
    },

    methods: {
        displayLabel(custom) {
            return `${custom ? this.i18n('Between') : this.i18n('When')}${this.name ? `: ${this.name}` : ''}`;
        },
    },
};
</script>

<style lang="scss">
    .date-filter {
        .header {
            border-top-left-radius: inherit;
            border-top-right-radius: inherit;
            padding-top: 0.5em;
        }

        .tag {
            cursor: pointer;
            margin: 2px;
        }

        .picker {
            width: 12.6em;
        }

        .filter-wrapper {
            border-radius: inherit;
            padding: 0.25em;
        }

        .tags-wrapper {
            .filter-tags {
                min-height: 2.25em;
                padding: 0.2em;

                .direction {
                    vertical-align: middle;
                }
            }
        }
    }
</style>
