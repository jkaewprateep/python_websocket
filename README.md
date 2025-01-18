# python_websocket
Websocket connection using Python programming language

<p align="center" width="100%">
    <img width="25%" src="https://github.com/jkaewprateep/starting_guide_pygames_AI_policy/blob/main/Python.jpg">
    <img width="14%" src="https://github.com/jkaewprateep/python_websocket/blob/main/websocket.jpg">
    <img width="11%" src="https://github.com/jkaewprateep/python_websocket/blob/main/ce6d14bd-9453-450b-a409-2558316d7e55.jpg">
    <img width="16%" src="https://github.com/jkaewprateep/python_websocket/blob/main/children-computer-class-us-education-video-game-kids-s-club-who-spend-many-hours-behind-monitor-95532258.webp"> </br>
    <b> Python programming and web sockets communication </b> </br>
    <b> ( Picture from Internet ) </b> </br>
</p>

🧸💬 Development method of communication with rapid response in milliseconds to minutes can be handled by Python programming language same as C and Java, today requirements of data methods ready processing programming languages and scripts programming create demands on Python programming languages combined with administrative works demands and cloud services platforms. </br>
🐑💬 ➰ Publisher-subscriber is one method that supports web socket communication the same as event listeners and server-clients methods when demands and bandwidths are calculated and the publisher-subscriber method helps reduce the time of communication and amount of data transfer on networks since there is a small of chunks required forward-backward by this methods and remains requirements for duration of the communication. </br>
🦭💬 Awares of using web-socket communication should use secure communication channels composed of multiple criteria of data input in negotiation method requirements same as our examples had key, internal mechanism and target website with token users. There will be more requirements if you transfer personal data since it will not require more time than it should contain real-time data with less personal information or required to invoke in the method for personal data communications. </br>

🐯💬 Culture-INFO, one-way or two-way communication is communication, and they need a suitable method for effective communication for negotiating, channeling, and communication. Since the data communication web socket can have the same or double the level of security but mostly implement real-time data communication with the benefits of effective response time the real-time data is selected and set for this communication method and to have personal communication data contained it required permission action and authority same as we accepted remote connection to our machine. <br>   
🦁💬 Authentication levels should match the level of action and be acknowledged by users who act it is authentic, and contained personal information should encrypted or masked to the level of communication security and agreements to acknowledge since it is required by system work only. </br>


## Websocket subscriber example
🐐💬  It is simple to implement a web socket subscriber and this code is from a project and demo of the Alpaca stock trading product. The secretes key is the project from the website of the Alpaca application to create as a token since it is circuit breaks if there is warning access from a third-party. </br>

```
def connect(self):
        """Establish WebSocket connection"""
        def on_open(ws):
            logging.info("Connection established - Authenticating...")
            self.log_connection_event("connection_opened")
            
            auth_data = {
                "action": "auth",
                "key": self.api_key,
                "secret": self.api_secret
            }
            ws.send(json.dumps(auth_data))

        def on_message(ws, message):
            try:
                msg = json.loads(message)
                
                # Handle authorization
                if msg.get('stream') == 'authorization':
                    if msg.get('data', {}).get('status') == 'authorized':
                        logging.info("Authentication successful!")
                        self.log_connection_event("authentication_success")
                        self.subscribe_to_trades()
                    else:
                        logging.error("Authentication failed!")
                        self.log_connection_event("authentication_failed")
                        self.ws.close()
                    return

                # Handle trade updates
                if msg.get('stream') == 'trade_updates':
                    data = msg.get('data', {})
                    self.handle_trade_update(data)
                    self.store_trade_update(data)
                    return

            except Exception as e:
                logging.error(f"Error processing message: {e}")
                self.log_connection_event("message_error", str(e))
        
        def on_error(ws, error):
            error_msg = str(error)
            logging.error(f"WebSocket error: {error_msg}")
            self.log_connection_event("websocket_error", error_msg)

            # Check for specific errors
            if '403' in error_msg:
                logging.error("Order Not Created - Insufficient buying power")

        def on_close(ws, close_status_code, close_msg):
            logging.info("Connection closed")
            self.log_connection_event("connection_closed", {
                "status_code": close_status_code,
                "message": close_msg
            })
            self.is_connected = False

        websocket.enableTrace(False)
        self.ws = websocket.WebSocketApp(
            self.ws_url,
            on_open=on_open,
            on_message=on_message,
            on_error=on_error,
            on_close=on_close
        )
        
        ws_thread = threading.Thread(
            target=self.ws.run_forever,
            kwargs={'ping_interval': 30}
        )
        ws_thread.daemon = True
        ws_thread.start()
        
        self.is_connected = True
```

```
if __name__ == "__main__":
    # config = configparser.ConfigParser()
    # path = os.path.join(os.getcwd(), "configuration.ini");
    # config.readfp(open(path))
    # APCA_API_DATA_URL = config.get('Alpaca API', 'APCA_API_DATA_URL')
    # API_KEY = config.get('Alpaca API', 'ALPACA_API_KEY')
    # API_SECRET = config.get('Alpaca API', 'ALPACA_SECRET_KEY')
    # MONGO_URI = config.get('MongoDB', 'CONNECTION_STRING')
    # API_KEY = "PKU50NJU0ZQYJCL5KQFI"
    # API_SECRET = "3hNrEicrq4iciQ6NiQ7ctVXFiEyCedm9YnQ65eUy"
    # MONGO_URI = "mongodb://localhost:27017/"

    load_dotenv()
    APCA_API_DATA_URL = os.getenv('APCA_API_DATA_URL_1')
    API_KEY = os.getenv('ALPACA_API_KEY_1')
    API_SECRET = os.getenv('ALPACA_SECRET_KEY_1')
    MONGO_URI = os.getenv('CONNECTION_STRING_1')


    client = AlpacaWebsocketClient(API_KEY, API_SECRET, MONGO_URI, paper=True)
    
    try:
        client.connect()
        logging.info("Client initialized and connecting...")
        
        while True:
            if not client.is_connected:
                logging.warning("Connection lost - Attempting to reconnect...")
                time.sleep(5)
                client.connect()
            time.sleep(1)
            
    except KeyboardInterrupt:
        logging.info("Shutting down...")
        client.disconnect()
    except Exception as e:
        logging.error(f"Unexpected error: {e}")
        if client:
            client.disconnect()
```
