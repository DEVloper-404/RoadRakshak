# Sample Colab API Code

This is the Python code you will run in your **Google Colab notebook**. It uses Flask to create a web server and `pyngrok` to expose it to the internet, allowing your Node.js dashboard to fetch the data securely.

## 1. Install Dependencies
Run this in the first cell of your Colab notebook:
```bash
!pip install flask pyngrok
```

## 2. API Server Code
Run this in the next cell. Make sure to sign up on [ngrok.com](https://ngrok.com) to get a free Auth Token.

```python
from flask import Flask, jsonify
from pyngrok import ngrok
import datetime
import random

app = Flask(__name__)

# =========================================================================
# STEP 1: SECURITY & AUTHENTICATION
# =========================================================================
# JUNIOR DEV NOTE: Never hardcode your actual auth token in public GitHub 
# repos! If using Colab, you can store it safely using the "Secrets" tab 
# (the key icon on the left menu).
# You can get your token from: https://dashboard.ngrok.com/get-started/your-authtoken
ngrok.set_auth_token("YOUR_NGROK_AUTH_TOKEN_HERE")

@app.route('/api/detections', methods=['GET'])
def get_detections():
    # =====================================================================
    # STEP 2: SENDING AI MODEL DATA
    # =====================================================================
    # JUNIOR DEV NOTE: This is the API endpoint. Your AI model will run 
    # its predictions elsewhere in the notebook and save the results.
    # You will update this function to read those real results and return them.
    # 
    # For now, this returns mock data so the frontend team can build the 
    # dashboard UI without waiting for the AI model to be finished.
    mock_response = {
        "timestamp": datetime.datetime.utcnow().isoformat() + "Z",
        "camera_id": "camera_01",
        "status": "active",
        "detections": [
            {
                "type": "pothole",
                "confidence": round(random.uniform(0.75, 0.99), 2),
                "bounding_box": [120, 50, 200, 150] # [x, y, width, height]
            }
        ]
    }
    
    return jsonify(mock_response)

# =========================================================================
# STEP 3: EXPOSING THE API & WHERE TO PUT THE URL
# =========================================================================
# ngrok opens a secure tunnel from this Colab instance to the public internet.
public_url = ngrok.connect(5000).public_url
print(f"✅ Secure API URL created!")
print(f"🔗 IMPORTANT DASHBOARD URL -> {public_url}/api/detections")
print(f"===================================================================")
print(f"JUNIOR DEV NOTE: Copy the URL printed above and paste it into the")
print(f"Node.js Dashboard code (e.g., inside public/js/app.js fetch call).")
print(f"That is how the dashboard will receive this data.")
print(f"===================================================================")

# Start the server (this cell will keep running and serving data)
app.run(port=5000)
```

## 3. How to Connect to Dashboard
When you run the code, it will print a link like:  
`https://<random-id>.ngrok-free.app/api/detections`

In your Node.js `dashboard`, you simply make a `fetch()` request to this URL to get the real-time JSON data.
