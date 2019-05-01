<template>
    <aside class="sm-side">
        <div class="user-head">
            <a class="inbox-avatar" href="javascript:;">
                <img width="64" height="60" src="../assets/images/avatar.jpg">
            </a>
            <div class="user-name">
                <h5>Kennedy Osaze</h5>
                <span class="email-address">info@example.com</span>
            </div>
            <a class="mail-dropdown pull-right" href="javascript:;">
                <i class="fa fa-chevron-down"></i>
            </a>
        </div>

        <div class="inbox-body">
            <app-compose></app-compose>
        </div>

        <ul class="inbox-nav inbox-divider">
            <li :class="{ active: activeView === 'app-inbox' }">
                <a href="#" @click.prevent="render('app-inbox', { title: 'Inbox' })">
                    <i class="fa fa-inbox"></i> Inbox
                    <span class="label label-danger pull-right">{{ unreadMessages.length }}</span>
                </a>
            </li>
            <li :class="{ active: activeView === 'app-sent' }">
                <a href="#" @click.prevent="render('app-sent', { title: 'Sent' })">
                    <i class="fa fa-envelope-o"></i> Sent Mail
                    <span class="label label-default pull-right">{{ sentMessages.length }}</span>
                </a>
            </li>
            <li :class="{ active: activeView === 'app-important' }">
                <a href="#" @click.prevent="render('app-important', { title: 'Important' })">
                    <i class="fa fa-bookmark-o"></i> Important
                    <span class="label label-warning pull-right">{{importantMessages.length }}</span>
                </a>
            </li>
            <li :class="{ active: activeView === 'app-trash' }">
                <a href="#" @click.prevent="render('app-trash', { title: 'Trash' })">
                    <i class=" fa fa-trash-o"></i> Trash
                    <span class="label label-default pull-right">{{ trashedMessages.length }}</span>
                </a> 
            </li>
        </ul>
    </aside>
</template>

<script>
import { EventBus } from '../event-bus';
import Compose from './Compose.vue';

export default {
    name: 'app-sidebar',
    components: {
        'app-compose': Compose,
    },
    props: {
        messages: {
            type: Array,
            required: true
        },
    },
    data() {
        return {
            activeView: 'app-inbox',
        };
    },
    computed: {
        unreadMessages() {
            return this.messages.filter(message => {
                return (message.type === 'incoming' && !message.isRead && !message.isDeleted);
            });
        },
        sentMessages() {
            return this.messages.filter(message => {
                return (message.type === 'outgoing' && !message.isDeleted);
            });
        },
        importantMessages() {
            return this.messages.filter(message => {
                return (message.type === 'incoming' && message.isImportant && !message.isDeleted);
            });
        },
        trashedMessages() {
            return this.messages.filter(message => {
                return message.isDeleted;
            });
        }
    },
    methods: {
        render(view, data = {}) {
            EventBus.$emit('change-view', { tag: view, ...data });
        }
    },
    created() {
        EventBus.$on('change-view', (data) => {
            this.activeView = data.tag;
        });
    }
};
</script>
