# Android Application for Diabetes Prediction Using Machine Learning
This project demonstrates how to deploy a machine learning model for diabetes prediction through a mobile application. It consists of:

⚙️ A FastAPI backend serving a trained ML model.

📱 An MIT App Inventor Android application as the frontend interface.

🧠 Backend: FastAPI for Diabetes Prediction
The backend is built using FastAPI, a modern and fast (high-performance) web framework for Python 3.7+.

1. ✅ Setting Up the Environment
Install the required dependencies:

bash
Copy
Edit
pip install fastapi
pip install uvicorn
pip install pickle5
pip install pydantic
pip install scikit-learn
pip install requests
pip install pypi-json
pip install pyngrok
pip install nest-asyncio
2. 🚀 API Initialization and Middleware
python
Copy
Edit
from fastapi import FastAPI
from pydantic import BaseModel
import pickle
import uvicorn
from pyngrok import ngrok
from fastapi.middleware.cors import CORSMiddleware
import nest_asyncio
from fastapi.responses import FileResponse

app = FastAPI()

# Enable CORS
origins = ["*"]
app.add_middleware(
    CORSMiddleware,
    allow_origins=origins,
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)

# Root endpoint
@app.get("/")
def read_root():
    return {"message": "Welcome to my FastAPI app!"}
3. 📥 Defining the Input Model
python
Copy
Edit
class model_input(BaseModel):
    Pregnancies: int
    Glucose: int
    BloodPressure: int
    SkinThickness: int
    Insulin: int
    BMI: float
    DiabetesPedigreeFunction: float
    Age: int
4. 📦 Loading the ML Model
python
Copy
Edit
diabetes_model = pickle.load(open(r"c:/Users/user/Desktop/Machine learning pratice/Deploy diabtes prediction M", 'rb'))
5. 🧪 Diabetes Prediction Endpoint
python
Copy
Edit
@app.post('/diabetes_prediction')
def diabetes_predd(input_parameters: model_input):
    input_list = [
        input_parameters.Pregnancies,
        input_parameters.Glucose,
        input_parameters.BloodPressure,
        input_parameters.SkinThickness,
        input_parameters.Insulin,
        input_parameters.BMI,
        input_parameters.DiabetesPedigreeFunction,
        input_parameters.Age
    ]
    prediction = diabetes_model.predict([input_list])

    if prediction[0] == 0:
        return 'The person is not diabetic'
    else:
        return 'The person is diabetic'
6. 🌐 Exposing the API with Ngrok
python
Copy
Edit
ngrok.authtoken("YOUR_AUTHTOKEN")  # Replace with your authtoken
ngrok_tunnel = ngrok.connect(8000)
print('Public URL:', ngrok_tunnel.public_url)

nest_asyncio.apply()
uvicorn.run(app, host="0.0.0.0", port=8000)
Save your Ngrok authtoken in your ngrok configuration file for convenience.

📄 Sample API Request
Example Input JSON:

json
Copy
Edit
{
  "Pregnancies": 2,
  "Glucose": 100,
  "BloodPressure": 100,
  "SkinThickness": 10,
  "Insulin": 100,
  "BMI": 25,
  "DiabetesPedigreeFunction": 0.253,
  "Age": 50
}
Curl Example:

bash
Copy
Edit
curl -X 'POST' \
  'https://<your-ngrok-url>/diabetes_prediction' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "Pregnancies": 2,
  "Glucose": 100,
  "BloodPressure": 100,
  "SkinThickness": 10,
  "Insulin": 100,
  "BMI": 25,
  "DiabetesPedigreeFunction": 0.253,
  "Age": 50
}'
Sample Response:

json
Copy
Edit
"The person is diabetic"
📲 Frontend: MIT App Inventor
The Android application frontend was developed using MIT App Inventor, a block-based visual programming environment.

🧩 UI Components
Labels: "Sugar", "Diabetes AI"

Text Boxes: For inputting Pregnancies, Glucose, Blood Pressure, Skin Thickness, Insulin, BMI, Diabetes Pedigree Function, and Age.

Buttons:

Predict: Sends prediction request.

Connect: Initializes the API endpoint.

Other:

Notifier

model_response: Displays prediction result.

⚙️ App Logic (Block Editor)
The "Predict" button gathers inputs and sends them to the backend via an HTTP POST request.

The response (prediction) is displayed on the screen.

Main Blocks:
make a dictionary: Creates a JSON payload with the necessary fields.

web1.Url: Set to the public ngrok URL of the FastAPI backend.

web1.PostText: Sends the dictionary as JSON to the API.

web1.GotText: Displays the result in model_response.Text.

📦 Project Structure
bash
Copy
Edit
📁 diabetes_app_backend
│   ├── main.py               # FastAPI backend
│   ├── diabetes_model.pkl    # Trained ML model
│   └── requirements.txt      # List of dependencies
📁 mit_app_inventor
│   └── DiabetesPredictor.aia # MIT App Inventor source file
📌 Notes
Ensure your Android device is connected to the internet when using the app.

The backend must be running and exposed via ngrok during testing.

📜 License
This project is licensed under the MIT License.
