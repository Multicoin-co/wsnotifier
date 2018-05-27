# wsnotifier

wsnotifier is a Gevent based Asynchronous WebSocket Server written in Python. wsnotifier exposes HTTP APIs for forwarding the messages to the websocket clients. This makes it easier to use the service with any web application.

# Installation

	$ pip install git+https://github.com/semk/wsnotifier.git

# Running wsnotifier server

	$ wsnotifier
	Starting wsnotifier on ws://0.0.0.0:1729/alerts and http://0.0.0.0:1729/alerts

# Connecting to wsnotifier via websocket

`wscat` is a nice commandline websocket client. You can install it by

	$ npm install -g wscat

Connect to the websocket server at ws://0.0.0.0:1729

	$ wscat -c ws://localhost:1729/alerts
	connected (press CTRL+C to quit)

# Posting messages to wsnotifier via HTTP so it can forward to the clients.

You can use any HTTP client to send the messages. This will be forwarded to all the websocket clients asynchronously. A Python client for wsnotifier is available [here](wsnotifier/notification_client.py)

	$ curl -X POST -H "Content-Type: application/json" -d '{"id": "unique-message-id", "type": "important", "message": "important message"}' http://0.0.0.0:1729/alerts
	{"status": "success"}

You can see the message coming on the `wscat` client.

	$ wscat -c ws://localhost:1729/alerts
	connected (press CTRL+C to quit)
	< {"message": "important message", "type": "important", "id": "unique-message-id"}
	>