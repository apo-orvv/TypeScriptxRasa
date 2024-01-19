# Rasa WebChat Integration Guide

## 1. Overview

The Rasa WebChat integration in this React-Typescript project enhances the user interface, providing a seamless chatbot experience. This guide outlines the implementation details and customization options for integrating Rasa WebChat into the project.

### ⚠️ Version Compatibility

- Version 1.0.1 of the Rasa WebChat is designed to work with Rasa versions 2.3.x and 2.4.x.
- Version 1.0.0 is intended for other Rasa versions.

<br />

## Features
- Text Messages
- Quick Replies
- Images
- Carousels
- Markdown support
- Persistent sessions
- Typing indications
- Smart delay between messages
- Easy import as a script tag or React Component
- Connects to a RASA server for natural language processing and dialogue management
- Custom Action Handling: Supports actions like redirecting to URLs or displaying recommended intents as buttons
- Customizable Styling: CSS available for customizing the chatbot's appearance
- Recommended Intents as Buttons: Displays clickable buttons for recommended intents, enhancing user experience.

## 2. Integration Steps

### 2.1 Installation

To run this project, ensure Node.js and RASA are installed on your machine.

#### Backend
1. Start the RASA server:
   ```bash
   rasa train # to train the RASA model
   rasa run -m models --enable-api --cors "*" --debug # to run the RASA Web Server
   rasa run actions # to run the custom actions server
   ```

#### Frontend
2. Start the React app:
   ```bash
   npm install # to install dependencies
   npm start # to start the development server
   ```
   The app will be available at http://localhost:3000.

#### 2.1.1 Install Rasa WebChat Package

Add the Rasa WebChat package to your project by including the following script in your HTML file:

```html
<!-- Add this script tag to your file -->
<script src="https://cdn.jsdelivr.net/npm/rasa-webchat/lib/index.min.js" async></script>
```

⚠️ Adding a version tag is recommended to prevent breaking changes from major versions, e.g., for version 1.0.0: https://cdn.jsdelivr.net/npm/rasa-webchat@1.0.0/lib/index.js. If a version is not specified, the latest available version of Rasa WebChat will be served.

### As a React component

Install the [npm package](https://npmjs.com/rasa-webchat):

```bash
npm install rasa-webchat
```

Then, in your React component:

```typescript
import Widget from 'rasa-webchat';

const CustomWidget: React.FC = () => {
  return (
    <Widget
      initPayload={"/get_started"}
      socketUrl={"http://localhost:5500"}
      socketPath={"/socket.io/"}
      customData={{"language": "en"}}
      title={"Title"}
    />
  );
};

export default CustomWidget;
```

- Ensure to set the prop `embedded` to `true` if you don't want to see the launcher.

## Parameters

| Prop / Param             | Default Value        | Description                                                                                                                                                                                                                                                                                                                                                                           |
|--------------------------|----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `initPayload`            | `null`               | Payload sent to Rasa when the conversation starts                                                                                                                                                                                                                                                                                                                                     |
| `socketUrl`              | `null`               | Socket URL                                                                                                                                                                                                                                                                                                                                                                            |
| `socketPath`             | `null`               | Close the chat window                                                                                                                                                                                                                                                                                                                                                                 |
| `customData`             | `null`               | Arbitrary object sent with the socket. If using with Botfront, it must include the language (e.g. `{"language": "en"}`)                                                                                                                                                                                                                                                                |
| `docViewer`              | `false`              | Treats links in received messages as links to a document (`.pdf .doc .xlsx`, etc.) and opens them in a popup using `https://docs.google.com/viewer` service                                                                                                                                                                                                                        |
| `title`                  | `'Welcome"`          | Title shown in the header of the widget                                                                                                                                                                                                                                                                                                                                               |
| `subtitle`               | `null`               | Subtitle shown under the title in the header of the widget                                                                                                                                                                                                                                                                                                                            |
| `inputTextFieldHint`     | `"Type a message"`   | User message input field placeholder                                                                                                                                                                                                                                                                                                                                                 |
| `hideWhenNotConnected`   | `true`               | If `true`, the widget will hide when the connection to the socket is lost                                                                                                                                                                                                                                                                                                             |
| `connectOn`              | `"mount"`            | Chooses when the widget will try connecting to the server. Default: `mount`. Options: `mount` (connects as soon as it mounts), `open` (attempts connection when the widget is opened)                                                                                                                                                                                              |
| `onSocketEvent`          | `null`               | Calls custom code on a specific socket event                                                                                                                                                                                                                                                                                                                                         |
| `embedded`               | `false`              | Set to `true` to embed the chatbot in a web page. The widget will always be open, and the `initPayload` will be triggered immediately                                                                                                                                                                                                                                             |
| `showFullScreenButton`   | `false`              | Show a full-screen toggle                                                                                                                                                                                                                                                                                                                                                            |
| `displayUnreadCount`     | `false`              | Path to an image displayed on the launcher when the widget is closed                                                                                                                                                                                                                                                                                                                 |
| `showMessageDate`        | `false`              | Show message date. Can be overridden with a function: `(timestamp, message) => return 'my custom date'`                                                                                                                                                                                                                                                                            |
| `customMessageDelay`     | See below            | A function that takes a message string as an argument. Called every time a message is received, and the returned value is used as a milliseconds delay before displaying a new message.                                                                                                                                                                                             |
| `params`                 | See below            | Used to customize the image size and change storage options                                                                                                                                                                                                                                                                                                                           |
| `storage`                | `"local"`            | Specifies the storage location of the conversation state in the browser. `"session"` stores in the session storage, persisting on reload but clearing on tab closure. `"local"` stores in the local storage, persisting after the browser is closed but clearing with browser cookies or `localStorage.clear()`                                                                                                                                        |
| `customComponent`        | `null`               | Custom component to be used with custom responses. E.g., `customComponent={(messageData) => (<div>Custom React component</div>)}`. Note: Can only be used with a React application.                                                                                                                                                                                                |
| `onWidgetEvent`          | `{}`                 | Calls custom code on a specific widget event (`onChatOpen`, `onChatClose`, `onChatHidden` are available). Add a function to the desired object property in the props to have it react to the event.                                                                                                                                                                                   |

## 3. Customization Options

### 3.1 Chatbot Styling

The chatbot's appearance can be customized by modifying the styles in the project's stylesheet. The hierarchy is as follows:

```plaintext
.rw-conversation-container
  |-- .rw-header
        |-- .rw-title
        |-- .rw-close-function
        |-- .rw-loading
  |-- .rw-messages-container
        |-- .rw-message
              |-- .rw-client
              |-- .rw-response
        |-- .rw-replies
              |-- .rw-reply
              |-- .rw-response
        |-- .rw-snippet
              |-- .rw-snippet-title
              |-- .rw-snippet-details
              |-- .rw-link
        |-- .rw-imageFrame
        |-- .rw-videoFrame
  |-- .rw-sender
        |-- .rw-new-message
        |-- .rw-send
```

### 3.2 Handling Custom Actions

Modify the `handleCustomAction` method in the React component to customize the behavior of Rasa's custom actions, such as displaying recommendations or redirecting the user.

### 3.4 Scalability Considerations

When deploying the chatbot in a production environment, consider scalability considerations such as model retraining frequency, code optimization, resource management, and model persistence.

## 4. FAQs

## Q: Can I customize the appearance of the chatbot?**  
**A:** Yes, modify the styles in the project's stylesheet to customize the chatbot's appearance.

## Q: How do I handle custom actions from Rasa, such as displaying recommendations?**  
**A:** Modify the `handleCustomAction` method in the React component to customize the behavior of Rasa's custom actions.

## Q: What scalability considerations should I keep in mind for production deployment?**  
**A:** Consider factors such as model retraining frequency, code optimization, resource management, and model persistence for scalability in a production environment.

## Q: Can I customize the behavior of the chatbot based on user actions or events?
**A:** Yes, you can leverage the `onWidgetEvent` and `onSocketEvent` props to call custom code when specific events occur, allowing you to tailor the chatbot's behavior based on user interactions or socket events.

## Q: Is it possible to handle multiple languages with Rasa WebChat?
**A:** Absolutely! The `customData` prop allows you to include language information, making it versatile for multilingual support. Ensure your Rasa models are trained accordingly for different languages.

## Q: How can I enable full-screen mode for the chatbot?
**A:** Set the `showFullScreenButton` prop to `true` to display a full-screen toggle button. Users can then expand the chatbot to full-screen mode for a more immersive experience.

## Q: Can I integrate Rasa WebChat with other platforms or channels?
**A:** Rasa WebChat is designed to work seamlessly with web applications. For integration with other platforms or channels, you may need to explore additional tools or adapt the provided React component accordingly.

## Q: What steps should I take if the chatbot is not connecting to the Rasa server?
**A:** Check the `socketUrl` and `socketPath` props to ensure they are correctly configured. Verify that the Rasa server is running and accessible. If issues persist, inspect the browser console for potential errors.

## Q: How can I customize the delay between messages for a more natural conversation flow?
**A:** Utilize the `customMessageDelay` prop, providing a function that determines the delay based on the content of each message. Adjusting this function allows you to control the timing between messages dynamically.

## Q: Are there any security considerations when deploying Rasa WebChat in a production environment?
**A:** Ensure secure communication by using HTTPS for both your Rasa server and your web application. Additionally, validate user inputs on the server side to prevent potential security vulnerabilities.

## Q: Can I extend Rasa WebChat to support additional features or integrations?
**A:** Yes, the React component architecture allows for easy extension. You can add custom components, modify the styling, or integrate external libraries to enhance functionality.

## Q: How can I track user interactions and conversations for analytics purposes?
**A:** Implement tracking mechanisms within your Rasa server or on the client side to capture relevant user interactions. Ensure compliance with privacy regulations and inform users about data collection practices.
