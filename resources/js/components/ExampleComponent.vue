<template>
    <div class="container">
        <div class="row">
            <div class="col-md-8 col-md-offset-2">
                <div class="panel panel-default">
                    <div class="panel-heading">Example Chat</div>

                    <div class="panel-body">
                        <ul>
                            <li v-for="item in messages">{{ item.message }} - <i>{{ item.user }}</i></li>
                        </ul>

                        <input type="text" class="form-control" v-model="message"><br />
                        <div class="mb-1"></div>
                        <button class="btn btn-success" @click="send()">Send!</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>


<script>
    export default {
        data() {
            return {
                messages: [],
                message: '',
            }
        },
        methods: {
            send() {
                axios.post('/message', { message: this.message })
                    .then((response) => {
                        this.message = '';
                    });
            }
        },

        mounted() {
            Echo.channel('messages')
                .listen('.newMessage', (message) => {
                    this.messages.push(message);
                });
        }
    }
</script>
