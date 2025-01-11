AI Help ChatBot Backend Test Implementation

### Getting Started
- Generate a service account for the realtime database being used 
- Create `service_rcb.json` at the root of the app and update with the json file generated from the firebase admin
- Update the service endpoint accordingly
- Update `settings.py`

```py
import firebase_admin
from firebase_admin import credentials

FIREBASE_CREDENTIALS_PATH = "service_rcb.json"

# Initialize Firebase
cred = credentials.Certificate(FIREBASE_CREDENTIALS_PATH)
# add the firebase database url
firebase_admin.initialize_app(
    cred, {"databaseURL": "https://hazel-airlock-377520-default-rtdb.firebaseio.com"}
)
```

```py
class ChatAI(APIView):
    """Chat AI Page"""

    permission_classes = [AllowAny]

    @exception_advice
    def post(self, request, *args, **kwargs):
        data = request.data
        chat_id = data.get("chat_id")
        print(chat_id)
        message_id = data.get("message_id")
        db_ref = db.reference(f"/")
        chats_ref = db_ref.child(chat_id)
        message_ref = chats_ref.child(message_id)
        message = message_ref.get()
        print(message)
        # TODO Will process the message with AI and add the AI message
        chats_ref.push(
            {
                "text": "Changed Named",
                "userType": "AI",
                "timestamp": datetime.now().isoformat(),
                "username": "GetLinkedAI",
            }
        )
        return service_response(
            status="success",
            message="Chat AI",
            data={},
            status_code=200,
        )

```