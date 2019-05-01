<template>
    <div class="inbox-body">
        <app-messages :messages="importantMessages"></app-messages>
    </div>
</template>

<script>
import Messages from './Messages.vue';

export default {
    name: 'app-important',
    components: {
        'app-messages': Messages
    },
    props: {
        data: { type: Object, required: true }
    },
    computed: {
        importantMessages() {
            return this.data.messages.filter(message => {
                return (message.type === 'incoming' && message.isImportant && !message.isDeleted);
            });
        },
    }
};
</script>
