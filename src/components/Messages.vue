<template>
    <table v-if="messages.length > 0" class="table table-inbox table-hover">
        <tbody>
            <tr v-for="(message, index) in messages"
                :key="index"
                :class="{ unread: typeof message.isRead !== 'undefined' && !message.isRead }"
                @click="openMessage(message)"
                >
                <td class="inbox-small-cells">
                    <input type="checkbox" class="mail-checkbox">
                </td>
                <td class="inbox-small-cells">
                    <a href="#" v-if="typeof message.isImportant !== 'undefined'" @click.prevent.stop="message.isImportant = !message.isImportant">
                    <i :class="['fa', 'fa-star', { 'inbox-stared': message.isImportant }]"></i>
                    </a>
                </td>
                <td class="view-message">{{ message.from.name }}</td>
                <td class="view-message">{{ message.subject }}</td>
                <td class="view-message inbox-small-cells">
                    <i v-if="message.attachments.length" class="fa fa-paperclip"></i>
                </td>
                <td class="view-message text-right">{{ message.date.fromNow() }}</td>
            </tr>
        </tbody>
    </table>

    <p v-else>No messages here yet.</p>
</template>

<script>
import { EventBus } from '../event-bus';

export default {
    name: 'app-messages',
    props: {
        messages: { type: Array, required: true }
    },
    methods: {
        openMessage(message) {
            EventBus.$emit('change-view', {
                tag: 'app-view-message',
                title: message.subject,
                data: { message }
            });
        },
        say() { console.log('yes'); }
    }
};
</script>