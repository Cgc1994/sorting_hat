<template>
    <div id="app">
        <div class="content">
            <div class="chat-container">
                <Chat v-if="visible"
                      :participants="participants"
                      :myself="myself"
                      :messages="messages"
                      :chat-title="chatTitle"
                      :placeholder="placeholder"
                      :colors="colors"
                      :border-style="borderStyle"
                      :hide-close-button="hideCloseButton"
                      :close-button-icon-size="closeButtonIconSize"
                      :submit-icon-size="submitIconSize"
                      :submit-image-icon-size="submitImageIconSize"
                      :load-more-messages="toLoad.length > 0 ? loadMoreMessages : null"
                      :async-mode="asyncMode"
                      :scroll-bottom="scrollBottom"
                      :display-header="true"
                      :send-images="true"
                      :profile-picture-config="profilePictureConfig"
                      :timestamp-config="timestampConfig"
                      :accept-image-types="'.png, .jpeg'"
                      @onMessageSubmit="onMessageSubmit"
                      @onType="onType"
                      @onClose="onClose('param value')"/>
            </div>
        </div>
    </div>
</template>

<script>
    import Chat from './components/Chat.vue'
    import Questions from './sorting_hat.json'

    export default {
        name: 'app',
        components: {
            Chat
        },
        data() {
            return {
                visible: true,
                firstTime: true,
                gryffindor: 0,
                slytherin: 0,
                ravenclaff: 0,
                hufflepuff: 0,
                questions: Questions,
                wizardName: '',
                participants: [
                    {
                        name: 'Sorcing hat',
                        id: 1
                    }
                ],
                myself: {
                    name: 'Myself',
                    id: 2 },
                messages: [
                    {
                        content: "Really?! Is it that hard?",
                        participantId: 2,
                        timestamp: {year: 2012, month: 3, day: 5, hour: 20, minute: 10, second: 3, millisecond: 123},
                        uploaded: true,
                        viewed: true,
                        type: 'text'
                    },
                    {
                        content: "We´ll see...What´s your name young wizard?",
                        participantId: 1,
                        timestamp: {year: 2012, month: 3, day: 5, hour: 20, minute: 10, second: 3, millisecond: 123},
                        uploaded: true,
                        viewed: true,
                        type: 'text'
                    }
                ],
                chatTitle: 'Sorting hat chat',
                placeholder: 'Send your message',
                colors: {
                    header: {
                        bg: '#d30303',
                        text: '#fff'
                    },
                    message: {
                        myself: {
                            bg: '#fff',
                            text: '#525252'
                        },
                        others: {
                            bg: '#fb4141',
                            text: '#fff'
                        },
                        messagesDisplay: {
                            //bg: 'rgb(247, 243, 243)',
                            //bg: '#f7f3f3'
                            bg: '#f7f3f3'
                        }
                    },
                    submitIcon: '#b91010',
                    submitImageIcon: '#b91010',
                },
                borderStyle: {
                    topLeft: "10px",
                    topRight: "10px",
                    bottomLeft: "10px",
                    bottomRight: "10px",
                },
                hideCloseButton: false,
                submitIconSize: 24,
                submitImageIconSize: 24,
                closeButtonIconSize: "20px",
                asyncMode: true,
                toLoad: [
                    {
                        content: "Hey, sorcing hat! Which house do I belong?",
                        participantId: 2,
                        timestamp: {year: 2012, month: 3, day: 5, hour: 20, minute: 10, second: 3, millisecond: 123},
                        uploaded: true,
                        viewed: true,
                        type: 'text'
                    },
                    {
                        content: "Your case is very difficult...I think you need a test to determine it",
                        participantId: 1,
                        timestamp: {year: 2012, month: 3, day: 5, hour: 20, minute: 10, second: 3, millisecond: 123},
                        uploaded: true,
                        viewed: true,
                        type: 'text'
                    },
                ],
                scrollBottom: {
                    messageSent: true,
                    messageReceived: false
                },
                profilePictureConfig: {
                    others: true,
                    myself: true,
                    styles: {
                        width: '30px',
                        height: '30px',
                        borderRadius: '50%'
                    }
                },
                timestampConfig: {
                    format: 'HH:mm',
                    relative: false
                }
            }
        },
        methods: {
            // eslint-disable-next-line
            onType: function (e) {
                // eslint-disable-next-line
                console.log('typing');
            },
            loadMoreMessages(resolve) {
                setTimeout(() => {
                    resolve(this.toLoad); this.messages.unshift(...this.toLoad);
                    this.toLoad = [];
                }, 1000);
            },
            onMessageSubmit(message) {
                /*
                * example simulating an upload callback.
                */ 
               this.messages.push(message);
                   
                if (isNaN(parseInt(message.content))) {
                    // input is string
                    // participant answers
                if (message.participantId === 2) {
                    setTimeout(() => {
                            if (this.wizardName === '') {
                                this.wizardName = message.content
                                this.firstTime = false
                                this.messages.push({
                                    content: "Ok, " + this.wizardName + "...get ready to take one of the most difficult things in your magic life.",
                                    participantId: 1,
                                    timestamp: {year: 2012, month: 3, day: 5, hour: 20, minute: 10, second: 3, millisecond: 123},
                                    uploaded: true,
                                    viewed: true,
                                    type: 'text'
                                });
                                //timeout
                                setTimeout(() => {
                                    var answer = 'With numbers select your prefered option. \n'
                                    for (let i = 0; i < this.questions[0].answers.length; i++) {
                                        answer = answer + (i+1) + ' ' + this.questions[0].answers[i].title + '\n'
                                    }
                                    this.messages.push({
                                            // content: this.questions[0].title,
                                            content: answer,
                                            participantId: 1,
                                            timestamp: {year: 2012, month: 3, day: 5, hour: 20, minute: 10, second: 3, millisecond: 123},
                                            uploaded: true,
                                            viewed: true,
                                            type: 'text'
                                        });
                                }, 2500);
                            } else {
                                this.messages.push({
                                    content: 'Answer not correct. Try again',
                                    participantId: 1,
                                    timestamp: {year: 2012, month: 3, day: 5, hour: 20, minute: 10, second: 3, millisecond: 123},
                                    uploaded: true,
                                    viewed: true,
                                    type: 'text'
                                });
                            }
                    }, 3000);}
                } else {
                    // input is number
                    // participant answers
                    if (message.participantId === 2) {
                        setTimeout(() => {
                            if (!this.firstTime) {
                                // other questions
                                if (Number(message.content) > this.questions[0].answers.length || Number(message.content) < 1) {
                                    // controlling accepted numbers
                                    this.messages.push({
                                            content: 'Answer not correct. Try again',
                                            participantId: 1,
                                            timestamp: {year: 2012, month: 3, day: 5, hour: 20, minute: 10, second: 3, millisecond: 123},
                                            uploaded: true,
                                            viewed: true,
                                            type: 'text'
                                        });
                                } else {
                                    for (let i = 0; i < this.questions[0].answers.length; i++) {
                                        if (Number(message.content)-1 === i) {
                                            this.gryffindor += this.questions[0].answers[i].scores.g
                                            this.slytherin += this.questions[0].answers[i].scores.s
                                            this.hufflepuff += this.questions[0].answers[i].scores.h
                                            this.ravenclaff += this.questions[0].answers[i].scores.r
                                        }
                                    }
                                    this.questions.splice(0, 1)
                                    if (this.questions.length > 0) {
                                        var answer = this.questions[0].title + '\n'
                                        for (let i = 0; i < this.questions[0].answers.length; i++) {
                                            answer = answer + (i+1) + ' ' + this.questions[0].answers[i].title + '\n'
                                        }
                                        setTimeout(() => {
                                            this.messages.push({
                                                content: answer,
                                                participantId: 1,
                                                timestamp: {year: 2012, month: 3, day: 5, hour: 20, minute: 10, second: 3, millisecond: 123},
                                                uploaded: true,
                                                viewed: true,
                                                type: 'text'
                                            });
                                        }, 1000)
                                    } else {
                                        var housesArray = [this.gryffindor, this.slytherin, this.hufflepuff, this.ravenclaff],
                                        maxHouse = Math.max.apply(Math.max, housesArray),
                                        houseNames = ["gryffindor", "slytherin", "hufflepuff", "ravenclaff"],
                                        maxHouseName = houseNames[housesArray.indexOf(maxHouse)];
                                        setTimeout(() => {
                                            this.messages.push({
                                                content: 'Congratulations ' + this.wizardName + '! You´ve finished the test. You belong to '+ maxHouseName+'!',
                                                participantId: 1,
                                                timestamp: {year: 2012, month: 3, day: 5, hour: 20, minute: 10, second: 3, millisecond: 123},
                                                uploaded: true,
                                                viewed: true,
                                                type: 'text'
                                            });
                                        }, 1000)
                                    }   
                                }                       
                            } else {
                                this.messages.push({
                                        content: 'Answer not correct. Try again',
                                        participantId: 1,
                                        timestamp: {year: 2012, month: 3, day: 5, hour: 20, minute: 10, second: 3, millisecond: 123},
                                        uploaded: true,
                                        viewed: true,
                                        type: 'text'
                                    });
                            }
                        }, 3000);
                    }
                }
                // timeout simulating the request
                setTimeout(() => {
                    message.uploaded = true
                    message.viewed = true
                }, 2000)
            },
            onClose(param) {
                console.log(param)
                this.visible = false;
            },
            addMessage() {
                /* this.messages.push(
                    {
                        content: "Update state",
                        // myself: false,
                        participantId: 1,
                        timestamp: {year: 2014, month: 3, day: 5, hour: 20, minute: 10, second: 3, millisecond: 123},
                        uploaded: true,
                        viewed: true
                    }
                ) */
                this.messages.push(
                    {
                        type: 'image',
                        preview: null,
                        content: 'image',
                        participantId: 1,
                        timestamp: {year: 2014, month: 3, day: 5, hour: 20, minute: 10, second: 3, millisecond: 123},
                        uploaded: true,
                        viewed: false
                    }
                )
            }
        }
    }
</script>

<style>
    html { 
        background: url('./assets/background_image.jpg') no-repeat center center fixed; 
        -webkit-background-size: cover;
        -moz-background-size: cover;
        -o-background-size: cover;
        background-size: cover;
        }
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }

    .content {
        width: 100%;
        display: flex;
        align-items: center;
        justify-content: space-evenly;
        flex-wrap: wrap;
    }

    .chat-container {
        display: flex;
        align-items: center;
        justify-content: center;
        background: rgb(247, 243, 243);
        /* padding: 10px 0 10px 0; */
        height: 500px;
        width: 350px;
    }

    .external-controller {
        background: #2c3e50;
        height: 300px;
        width: 600px;
        display: flex;
        color: #eee;
    }

    .controller-btn-container {
        display: flex;
        justify-content: center;
        align-items: center;
        padding-left: 20px;
        padding-right: 20px;
        flex-wrap: wrap;
    }

    .btn-message {
        cursor: pointer;
        background: #eee;
        border: none;
        height: 40px;
        color: #2c3e50;
        border-radius: 5px;
        outline: none;
        transition: 0.3s;
    }

    .btn-participant {
        cursor: pointer;
        background: #eee;
        border: none;
        height: 40px;
        color: #2c3e50;
        border-radius: 5px;
        outline: none;
        transition: 0.3s;
    }

    .btn-message:hover {
        background: rgb(255, 255, 255);
    }

</style>
