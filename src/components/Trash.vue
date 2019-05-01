<template>
    <div class="inbox-body">
        <app-messages :messages="trashedMessages"></app-messages>
    </div>
</template>

<script>
import Messages from './Messages.vue';

export default {
    name: 'app-trash',
    components: {
        'app-messages': Messages
    },
    props: {
        data: { type: Object, required: true }
    },
    computed: {
        trashedMessages() {
            return this.data.messages.filter(message => {
                return message.isDeleted;
            });
        }
    }
};
</script>
