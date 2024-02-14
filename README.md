# API Documentation for LeaderboardAPI.net

## Websocket API

- The connection method for Websocket is:   
Base Url: **wss://leaderboardapi.net/ws**
- All Trader IDs for subscriptions are lowercase.
- A single connection is only valid for 24 hours; expect to be disconnected at 00:00 UTC.
- All responses are in JSON format.

### Live Subscribing/Unsubscribing to streams

- The following data can be sent through the websocket instance in order to subscribe/unsubscribe 
from streams. Examples can be seen below.
- Each channel represents an individual trader.

#### Subscribe to a channel

- Request   
```
{
  "action": "subscribe",
  "channel": "<channel_name>"
}
```

- Response
```
{
  "action": "subscribe",
  "channel": "<channel_name>",
  "error": "",
  "success": true
}
```

#### Unsubscribe from a channel
- Request 
```
{
  "action": "subscribe",
  "channel": "<channel_name>"
}
```

- Response
```
{
  "action": "unsubscribe",
  "channel": "<channel_name>",
  "error": "",
  "success": true
}
```

### Trader Positions Stream

**Stream Name:** leader@<trader_id>

**Update Speed:** 1 sec or less

How to get <trader_id>:
1. Go to Binance Leaderboard page and open a leader you need
2. In the address bar of the browser you can see like this:   
.../um?encryptedUid=2959F6FAD4BDD96952B86D72E7C9FAF3  
3. Copy this Trader ID: **2959F6FAD4BDD96952B86D72E7C9FAF3**

**For now only USDⓈ-M leaders availabe**

- Request example
```
{
  "action": "subscribe",
  "channel": "leader@2959f6fad4bdd96952b86d72e7c9faf3"
}
```

- Response
```
{
  "action": "subscribe",
  "channel": "leader@2959f6fad4bdd96952b86d72e7c9faf3",
  "error": "",
  "success": true
}
```

- Incoming data
```
{
  "trader_id": "2959f6fad4bdd96952b86d72e7c9faf3",
  "positions": [
    {
      "symbol": "RONINUSDT",
      "entryPrice": 2.628500948708,
      "markPrice": 2.63820471,
      "pnl": 182.06601424,
      "roe": 0.003459,
      "updateTime": [2024, 2, 14, 5, 48, 9, 504000000],
      "amount": 18762.4,
      "updateTimeStamp": 1707889689504,
      "yellow": true,
      "tradeBefore": false,
      "leverage": 5
    },
    {
      "symbol": "ADAUSDT",
      "entryPrice": 0.5069578869554,
      "markPrice": 0.5502,
      "pnl": -10237.09624668,
      "roe": -0.78593457,
      "updateTime": [2024, 2, 14, 5, 48, 29, 173000000],
      "amount": -236739,
      "updateTimeStamp": 1707194909173,
      "yellow": false,
      "tradeBefore": true,
      "leverage": 10
    },
    {
      "symbol": "ETHUSDT",
      "entryPrice": 2626.666352148,
      "markPrice": 2639.13,
      "pnl": 11355.06870109,
      "roe": 0.0944527,
      "updateTime": [2024, 2, 14, 5, 22, 41, 696000000],
      "amount": 911.055,
      "updateTimeStamp": 1707888161696,
      "yellow": false,
      "tradeBefore": false,
      "leverage": 20
    },
    ...
  ],
  "event": 1707889826748
}
```

#### Python example

```python
import asyncio
import json

import websockets

token = "YOUR_TOKEN_HERE"  # Get the token here https://rapidapi.com/volckovtyoma/api/binance-leaderboard5/


async def websocket_client():
    uri = "wss://leaderboardapi.net:443/ws"
    async with websockets.connect(uri, extra_headers={"Authorization": token}) as websocket:
        await websocket.send(
            json.dumps({
                "action": "subscribe",
                "channel": "leader@2959F6FAD4BDD96952B86D72E7C9FAF3".lower()
            })
        )

        # Continuously listen for messages from the server
        while True:
            response = await websocket.recv()
            print("Response from server:", response)


if __name__ == "__main__":
    asyncio.run(websocket_client())
```

## Terms of Use
### Limitation of Liability
By accessing and using the Leaderboard API, you acknowledge and agree that the utilization of the Leaderboard API is at your own discretion and risk. The author, developer, or provider of the Leaderboard API (hereafter referred to as "the Author") shall not be liable for any direct, indirect, incidental, consequential, or exemplary damages, including but not limited to damages for loss of profits, goodwill, use, data, or other intangible losses (even if the Author has been advised of the possibility of such damages), resulting from:

The use or the inability to use the Leaderboard API;
The cost of procurement of substitute goods and services resulting from any goods, data, information, or services purchased or obtained or messages received or transactions entered into through or from the Leaderboard API;
Unauthorized access to or alteration of your transmissions or data;
Statements or conduct of any third party on the Leaderboard API;
Any bugs, viruses, Trojan horses, or the like, which may be transmitted to or through the Leaderboard API by any third party; or
Any other matter relating to the Leaderboard API.
Furthermore, the Author does not warrant that:

The service will meet your requirements;
The service will be uninterrupted, timely, secure, or error-free;
The results that may be obtained from the use of the service will be accurate or reliable;
The quality of any products, services, information, or other material purchased or obtained by you through the service will meet your expectations; and
Any errors in the software will be corrected.
Any material downloaded or otherwise obtained through the use of the Leaderboard API is done at your own discretion and risk, and you will be solely responsible for any damage to your computer system or loss of data that results from the download of any such material.

You expressly understand and agree that the Author shall not be liable for any direct, indirect, incidental, special, consequential, or exemplary damages, including but not limited to, damages for loss of profits, goodwill, use, data, or other intangible losses (even if the Author has been advised of the possibility of such damages), resulting from the use or the inability to use the Leaderboard API.

By using the Leaderboard API, you agree to hold harmless and indemnify the Author and its affiliates, officers, agents, and employees from and against any claim, suit, or action arising from or related to the use of the Leaderboard API or violation of these terms, including any liability or expense arising from claims, losses, damages, suits, judgments, litigation costs, and attorneys’ fees.

