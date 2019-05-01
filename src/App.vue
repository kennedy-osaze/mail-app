<template>
    <div class="container">
        <div class="mail-box">
            <app-sidebar :messages="messages"></app-sidebar>
            <app-content :messages="messages"></app-content>
        </div>
    </div>
</template>

<script>
import Sidebar from './components/Sidebar.vue';
import Content from './components/Content.vue';
import messages from './data/messages';
import randomMessages from './data/random-messages';
import { EventBus } from './event-bus';

export default {
    name: 'app',
    components: {
        'app-sidebar': Sidebar,
        'app-content': Content, 
    },
    data() {
        return {
            messages
        };
    },
    created() {
        EventBus.$on('send-message', (data) => {
            const temp = [data.message];
            this.messages = temp.concat(this.messages.splice(0));
        });

        EventBus.$on('refresh-messages', () => {
            let randomIndex = Math.floor(Math.random() * randomMessages.length);
            const temp = [randomMessages[randomIndex]];
            this.messages = temp.concat(this.messages.splice(0));
        });
    }
}
</script>

<style>

</style>
