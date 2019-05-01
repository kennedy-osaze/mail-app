<template>
    <div>
        <a href="#compose-modal" data-toggle="modal" class="btn btn-compose">Compose</a>

        <div aria-hidden="true" role="dialog" tab-index="-1" id="compose-modal" class="modal fade" style="display: none;">
            <div class="modal-dialog">
                <div class="modal-content">
                    <div class="modal-header">
                        <button type="button" aria-hidden="true" data-dismiss="modal" class="close">&times;</button>
                        <h4 class="modal-title">New Message</h4>
                    </div>

                    <div class="modal-body">
                        <form role="form" class="form-horizontal" @submit.prevent="sendMessage">
                            <div class="form-group">
                                <label for="subject" class="col-md-2 control-label">Subject</label>
                                <div class="col-md-10">
                                    <input type="text" id="subject" class="form-control" v-model="message.subject">
                                </div>
                            </div>

                            <div class="form-group">
                                <label for="message" class="col-md-2 control-label">Message</label>
                                <div class="col-md-10">
                                    <textarea class="form-control" id="message" rows="10" v-model="message.content"></textarea>
                                </div>
                            </div>

                            <div class="form-group">
                                <div class="col-md-offset-2 col-md-10">
                                    <button type="submit" class="btn btn-send">Send</button>
                                </div>
                            </div>
                        </form>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { EventBus } from '../event-bus';
import moment from 'moment';

export default {
    name: 'app-compose',
    data() {
        return {
            message: {
                subject: null,
                content: null
            }
        };
    },
    methods: {
        sendMessage() {
            console.log(this.message);
            EventBus.$emit('send-message', {
                message: {
                    ...this.message,
                    isDeleted: false,
                    type: 'outgoing',
                    date: moment(),
                    from: {
                        name: 'Kennedy Osaze',
                        email: 'me.osaze@gmail.com'
                    },
                    attachments: []
                }
            });
        }
    }
}
</script>
