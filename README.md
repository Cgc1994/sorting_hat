## Intro
Hi! This chatbot is a simple test to determine the house of a young new wizard in Hogwharts. Through an interactive and intuitive chat such as Whatsapp or Telegram, the sorting hat will decide the future of the wizard in the school.

## How it works
At the beginning, the hat will ask the users name, and after that the questionnaire begins. Every question brings between 2 and 6 answers, that will be chosen entering a number. If the user introduces a non valid number (0 or below, higher than the number of answers), or an answer with a literal caracter, the answer will not be valid, and the chat will expect a valid one.
Every valid answer has a determinate score for every category, or wizard house in this case, and after the user selects the answer, that score will acumulate in the DOM.
At the end of the questionnaire, the sorting hat selects the house with a higher score. That will be the house of the young wizard for his/her next seven years of magic training!

## Instructions for local setup

    ## Npm dependencies
    ```
    npm install
    ```

    ### Compiles and hot-reloads for development
    ```
    npm run serve
    ```

    ### Compiles and minifies for production
    ```
    npm run build
    ```


## Features implemented
- Handle on type event and on message submit
- Simulate async actions like message uploaded status with timeouts
- Timestamp param (released at version 1.2.1)
- Custom style


## Usage
```javascript

## Example of HTML template
```
<template>
  <div>
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
        :link-options="linkOptions"
        :accept-image-types="'.png, .jpeg'"
        @onImageClicked="onImageClicked"
        @onImageSelected="onImageSelected"
        @onMessageSubmit="onMessageSubmit"
        @onType="onType"
        @onClose="onClose"/>
   </div>
</template>
```

## Example of data participants and self 'wizard' users
```
participants: [
                {
                    name: 'Sorting hat',
                    id: 1
                }
            ],
            myself: {
                name: 'Me',
                id: 2
            },
```
## Example of random messages in messages Array
```
messages: [
                {
                    content: 'Hi Ron Weasley. Is your wand broken again??',
                    myself: false,
                    participantId: 1,
                    timestamp: {year: 2019, month: 3, day: 5, hour: 20, minute: 10, second: 3, millisecond: 123},
                    type: 'text'
                },
                {
                    content: 'Yes...I was fighting with a spider!',
                    myself: true,
                    participantId: 2,
                    timestamp: {year: 2019, month: 4, day: 5, hour: 19, minute: 10, second: 3, millisecond: 123},
                    type: 'text'
                }
            ],
```
## Example of onMessageSubmit function to add messages to array
```
onMessageSubmit: function (message) {
            /*
            * example simulating an upload callback. 
            */
            this.messages.push(message);
            // timeout simulating the request
            setTimeout(() => {
                message.uploaded = true
            }, 2000)
        }
```
## Component Props
| name | type | required |default |description |
|------|------|----------|--------|------------|
| participants | Array | true |  | An array of participants. Each [participant](#participant) should be an Object with name and id|
| myself | Object | true |  | Object of my [participant](#participant). "myself" should be an Object with name and id|
| messages | Array | true |  | An array of [messages](#message). Each message should be an Object with content, myself, participantId and timestamp|
| chatTitle | String | false | Empty String | The title on chat header |
| placeholder | String | false | 'type your message here' | The placeholder of message text input |
| colors | Object | true |  | Object with the [color's](#color) description of style properties |
| borderStyle | Object | false | { topLeft: "10px", topRight: "10px", bottomLeft: "10px", bottomRight: "10px"}  | Object with the description of border style properties |
| hideCloseButton | Boolean | false | false  | If true, the 'Close' button will be hidden |
| asyncMode | Boolean | false | false | If the value is ```true``` the component begins to watch message upload status and displays a visual feedback for each message. If the value is ```false``` the visual feedback is disabled |
| loadMoreMessages | Function | false | () => false | If this function is passed and you reach the top of the messages, it will be called and a loading state will be displayed until you resolve it by calling the only parameter passed to it |
| scrollBottom | Object | false | { messageSent: true, messageReceived: false} | This object describes the chat scroll behavior. The two options represent the moment when the chat should scroll to the bottom. If 'messageSent' is ```true```, the chat will scroll to bottom aways you send a new message. If 'messageReceived' is ```true```, the chat will scroll to bottom always you receive a new message.  |
| displayHeader | Boolean | false | true | This prop describes whether the header should be displayed or not |
| timestampConfig | Object | false | ```{ format: 'HH:mm', relative: false }``` | This prop is a js Object that decribes the timestamp format / relativeness. |

# Events
| name | type | required |default |description |
|------|------|----------|--------|------------|
| onType | Function | false | () => false | Event called when the user is typing |
| onMessageSubmit | Function | false | () => false | Event called when the user sends a new message |
| onClose | Function | false | () => false | Event called when the user presses the close icon |

### participant
| name | type | description |
|---------|--------|----------------|
| id | int | The user id should be an unique value |
| name | String | The user name that will be displayed |

Example
```javascript
{
  name:  'Username',
  id: 1
},
```
### message
| name | type | description |
|---------|--------|----------------|
| content | String | The message text content |
| myself | boolean | (REMOVED) Whether the message was sent by myself or by other participants. Since version 1.0.8 this property is automatically set by the chat |
| participantId | int | The participant's id who sent the message  |
| timestamp | Object| Object describing the year, month, day, hour, minute, second and millisecond that the message was sent |
| uploaded | Boolean| If asyncMode is ```true``` and uploaded is ```true```, a visual feedback is displayed bollow the message. If asyncMode is ```true``` and uploaded is ```false```, a visual loading feedback is displayed bollow the message. If asyncMode is ```false```, this property is ignored.|
| viewed | Boolean| If asyncMode is ```true``` and viewed is ```true```, a visual feedback is displayed bollow the message.|
| type | String | This prop should be set by you in case a new message is received, otherwise, the chat will automatically set this prop. |

Example
```javascript
{
  content: 'received messages', 
  //myself: false,
  participantId: 1,
  timestamp: { 
    year: 2019, 
    month: 3, 
    day: 5, 
    hour: 20, 
    minute: 10, 
    second: 3, 
    millisecond: 123 
  },
  uploaded: true,
  viewed: true,
  type: 'text'
  // generated by URL.createObjectURL(file)
}
```
### color
| name | type | description |
|---------|--------|----------------|
| header | Object | Object containing the header background and text color |
| message | Object | Object containing the message background and text color. The Object should contains the style for 'myself' and 'others' |
| messagesDisplay | Object | Object containing the background color of mesages container. |
| submitIcon | String | The color applied to the send message button icon |

Example
```javascript
{
  header:{
    bg: '#d30303',
    text: '#fff'
  },
  message:{
    myself: {
      bg: '#fff',
      text: '#bdb8b8'
    },
    others: {
      bg: '#fb4141',
      text: '#fff'
    }
  },
  messagesDisplay: {
    bg: '#f7f3f3'
  },
  submitIcon: '#b91010'
}

```
## timestampConfig
| name | type | description |
|---------|--------|----------------|
| format | String | [Timestamp format](https://moment.github.io/luxon/docs/manual/formatting.html#toformat) |
| relative | Boolean | Whether the timestamp should be relative to current time |

Example
``` javascript
timestampConfig: {   
    format: 'HH:mm',
    relative: false
}
