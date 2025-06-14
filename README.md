# 📱 Building an Android Application for a Diabetes Prediction ML Model

This project demonstrates how to deploy a machine learning model for diabetes prediction through an Android application. It integrates a **FastAPI** backend for prediction services and a **MIT App Inventor** frontend for the mobile interface.

---

## 🚀 Backend: FastAPI for Diabetes Prediction

The backend is built using **FastAPI**, a modern, high-performance web framework for building APIs with Python 3.7+.

---

### 🔧 1. Setting up the Environment

Install the required Python libraries:

    ```bash
    pip install fastapi
    pip install uvicorn
    pip install pickle5
    pip install pydantic
    pip install scikit-learn
    pip install requests
    pip install pypi-json
    pip install pyngrok
    pip install nest-asyncio


# 2. API Initialization and Middleware
Initialize FastAPI and configure CORS middleware:

    ```python
    from fastapi import FastAPI
    from pydantic import BaseModel
    import pickle
    import uvicorn
    from pyngrok import ngrok
    from fastapi.middleware.cors import CORSMiddleware
    import nest_asyncio
    from fastapi.responses import FileResponse

    app = FastAPI()

    origins = ["*"]
    app.add_middleware(
    CORSMiddleware,
    allow_origins=origins,
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
    )

    @app.get("/")
    def read_root():
    return {"message": "Welcome to my FastAPI app!"}
    
# 3. Defining the Input Model
Define a Pydantic model for input validation:

    ```python
    class model_input(BaseModel):
        Pregnancies: int
        Glucose: int
        BloodPressure: int
        SkinThickness: int
        Insulin: int
        BMI: float
        DiabetesPedigreeFunction: float
        Age: int
# 4. Loading the ML Model
Load the pre-trained diabetes prediction model (pickle file):

    ```python
        diabetes_model = pickle.load(open(r"c:/Users/user/Desktop/Machine learning pratice/Deploy diabtes prediction M", 'rb'))

# 5. Diabetes Prediction Endpoint
Handle POST requests for prediction:

    ```python
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
        
# 6. Exposing the API with Ngrok
Expose the API publicly using Ngrok:

    ```python
    ngrok.authtoken("YOUR_AUTHTOKEN")  # Replace with your authtoken
    ngrok_tunnel = ngrok.connect(8000)
    print('Public URL:', ngrok_tunnel.public_url)
    nest_asyncio.apply()
    uvicorn.run(app, host="0.0.0.0", port=8000)

📬 Example Request
✅ Example Request Body (JSON)

    ```json
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
    
📡 Example Curl Command

    ```bash
    curl -X 'POST' \
      'https://<your-ngrok-url>.ngrok-free.app/diabetes_prediction' \
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
🧾 Example Server Response
    
    ```json
    "The person is diabetic"

📱 Frontend: MIT App Inventor
The Android frontend is developed using MIT App Inventor, a visual programming environment for building mobile apps.

🧩 App Components and Design
Labels: "Sugar", "Diabetes AI"

Text Boxes: For Pregnancies, Glucose, Blood Pressure, Skin Thickness, Insulin, BMI, Diabetes Pedigree Function, and Age.

Buttons: "Predict", "Connect"

Other Components: Notifier, model_response (for prediction results)

🔄 App Logic (Blocks Editor)
make a dictionary: Create JSON payload for POST request

web1.Url: Set to the Ngrok public URL

web1.PostText: Send request to FastAPI

web1.GotText: Display API response in model_response.Text

🧪 End-to-End Flow
User enters diabetes-related parameters on the Android app.

App sends a POST request to the FastAPI backend.

Backend returns prediction.

App displays whether the person is diabetic or not.

📝 License
This project is open source and available under the MIT License.
