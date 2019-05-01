<template>
    <div class="inbox-body">
        <app-messages :messages="sentMessages"></app-messages>
    </div>
</template>

<script>
import Messages from './Messages.vue';

export default {
    name: 'app-sent',
    components: {
        'app-messages': Messages
    },
    props: {
        data: { type: Object, required: true }
    },
    computed: {
        sentMessages() {
            return this.data.messages.filter(message => {
                return (message.type === 'outgoing' && !message.isDeleted);
            });
        },
    }
};
</script>
