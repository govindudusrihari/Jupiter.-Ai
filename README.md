pip install Flask
from flask import Flask, request, jsonify

app = Flask(__name__)

# Simple in-memory chat bot logic
def get_bot_response(user_input):
    responses = {
        "hello": "Hi there!",
        "how are you?": "I'm good, thanks for asking!",
        "bye": "Goodbye!",
    }
    return responses.get(user_input.lower(), "Sorry, I didn't understand that.")

@app.route('/message', methods=['POST'])
def message():
    data = request.json
    user_message = data.get('message')
    bot_response = get_bot_response(user_message)
    return jsonify({'response': bot_response})

if __name__ == '__main__':
    app.run(debug=True)
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Chat Bot</title>
    <style>
        body { font-family: Arial, sans-serif; }
        #chat-container { width: 300px; margin: auto; }
        #messages { height: 300px; border: 1px solid #ccc; overflow-y: auto; padding: 5px; }
        #message-input { width: calc(100% - 110px); }
        #send-button { width: 100px; }
    </style>
</head>
<body>
    <div id="chat-container">
        <div id="messages"></div>
        <input type="text" id="message-input" placeholder="Type a message...">
        <button id="send-button">Send</button>
    </div>

    <script>
        document.getElementById('send-button').addEventListener('click', function() {
            const messageInput = document.getElementById('message-input');
            const message = messageInput.value;
            if (message.trim() === "") return;

            addMessageToChat('You: ' + message);
            messageInput.value = '';

            fetch('/message', {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({ message: message })
            })
            .then(response => response.json())
            .then(data => {
                addMessageToChat('Bot: ' + data.response);
            });
        });

        function addMessageToChat(message) {
            const messagesDiv = document.getElementById('messages');
            const messageDiv = document.createElement('div');
            messageDiv.textContent = message;
            messagesDiv.appendChild(messageDiv);
            messagesDiv.scrollTop = messagesDiv.scrollHeight;
        }
    </script>
</body>
</html>
python app.py
