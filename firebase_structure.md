 button, a new `ROOM_ID` will be generated (e.g., a Firebase push key). A room will be created in Firebase with `status: 'waiting'`, `hostId`, `duration`, and `player1` details. The URL `your-app-url.com?room=ROOM_ID` will be copied to the clipboard.
*   **Guest Action**: When a user opens the app with a `?room=ROOM_ID` query parameter, the app will check if the `ROOM_ID` exists in Firebase and if its `status` is `waiting`. If so, the user's `USER_ID` and details will be added as `player2` to the room, the room `status` will change to `active`, and a notification will be sent to the host.

### Real-time Data Sync

*   A function `calculateBattleIncome(userId, duration)` will be implemented to retrieve `f_recs` from `localStorage` (or Firebase `users/{USER_ID}/f_recs`) and calculate the total income (Basic + OT + Extra) for the specified duration (15 or 30 days) relative to the battle start date.
*   This function will be called periodically (e.g., every few minutes) or triggered by user actions (e.g., adding new records).
*   The calculated `currentIncome` will be updated in `rooms/{ROOM_ID}/player1/currentIncome` or `rooms/{ROOM_ID}/player2/currentIncome`.
*   Firebase `on('value')` listeners will be used to detect changes in `rooms/{ROOM_ID}/player1/currentIncome` and `rooms/{ROOM_ID}/player2/currentIncome` to update the UI (HP Bar).

### Activity Logs & Chat Box

*   Any significant event (e.g., battle creation, player joining, income update, chat message) will trigger an entry in `rooms/{ROOM_ID}/logs`.
*   The chat box will use `rooms/{ROOM_ID}/chats` for real-time message exchange. New messages will be pushed to this node, and an `on('child_added')` listener will display them in the UI.
*   Notifications for new challenges will be implemented by listening to changes in `users/{USER_ID}/currentRoomId` or by checking for new `rooms` where the user is `player2` and the status is `waiting`.
