<template>
    <div class="inbox-body">
        <div class="mail-option">
            <div class="btn-group mb-5">
                <a href="#" class="btn btn-primary" @click="refresh">
                    <i class="fa fa-refresh"></i>&nbsp; Refresh
                </a>
            </div>
        </div>

        <app-messages :messages="incomingMessages"></app-messages>
    </div>
</template>

<script>
import Messages from './Messages.vue';
import { EventBus } from '../event-bus';

export default {
    name: 'app-inbox',
    components: {
        'app-messages': Messages
    },
    props: {
        data: { type: Object, required: true }
    },
    computed: {
        incomingMessages() {
            return this.data.messages.filter(message => {
                return (message.type === 'incoming' && !message.isDeleted);
            });
        },
    },
    methods: {
        refresh() {
            EventBus.$emit('refresh-messages');
        }
    }
};
</script>
