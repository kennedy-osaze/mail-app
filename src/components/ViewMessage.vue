<template>
    <div class="inbox-body">
        <div class="mail-option">
            <button class="btn btn-primary mr-5 mb-5" @click="goBack">
                <i class="fa fa-arrow-left" aria-hidden="true"></i>&nbsp; Back
            </button>

            <button class="btn btn-danger mr-5 mb-5" @click="data.message.isDeleted = true" :disabled="data.message.isDeleted">
                <i class="fa fa-trash-o"></i>&nbsp; {{ data.message.isDeleted ? 'Deleted' : 'Delete' }}
            </button>

            <template v-if="typeof data.message.isRead !== 'undefined'">
                <button class="btn btn-primary mr-5 mb-5" @click="data.message.isRead = false" :disabled="!data.message.isRead">
                    <i class="fa fa-envelope-open" aria-hidden="true"></i>&nbsp; Mark as unread
                </button>

                <button class="btn btn-primary mb-5" @click="data.message.isRead = true" :disabled="data.message.isRead">
                    <i class="fa fa-envelope" aria-hidden="true"></i>&nbsp; Mark as read
                </button>
            </template>
        </div>

        <p><strong>Date:</strong> {{ data.message.date.fromNow() }}</p>
        <p><strong>From:</strong> {{ data.message.from.name }} &lt;{{ data.message.from.email }}&gt;</p>
        <hr>
        <div v-html="data.message.content" class="message"></div>

        <div v-if="data.message.attachments.length" class="attachment-mail">
            <h4>Attachments</h4>

            <ul>
                <li v-for="(attachment, index) in data.message.attachments" :key="index">
                    <i class="fa fa-paperclip"></i> {{ attachment.fileName }} ({{ attachment.size | formatBytes }})
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
import { EventBus } from '../event-bus';

export default {
    name: 'app-view-message',
    props: {
        data: { type: Object, required: true }
    },
    methods: {
        goBack() {
            const previousView = this.$parent.previousView;

            EventBus.$emit('change-view', {
                tag: previousView.tag,
                title: previousView.title,
                data: previousView.data
            });
        }
    },
    filters: {
        formatBytes(bytes) {
            if (bytes === 0) return '0 Bytes';

            let decimals = 2;
            let k = 1000;
            let dm = decimals + 1 || 3;
            let sizes = ['Bytes', 'KB', 'MB', 'GB', 'TB', 'PB', 'EB', 'ZB', 'YB'];
            let i = Math.floor(Math.log(bytes) / Math.log(k));

            return parseFloat((bytes / (k ** i)).toFixed(dm)) + ' ' + sizes[i];
        }
    },
    activated() {
        if (typeof this.data.message.isRead !== 'undefined') {
            this.data.message.isRead = true;
        }
    }
};
</script>
