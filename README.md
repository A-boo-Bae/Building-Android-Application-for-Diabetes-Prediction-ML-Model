# Building-Android-Application-for-Diabetes-Prediction-ML-Model

I've been working on an exciting project: deploying a machine learning model for diabetes prediction as an Android application. This involves a FastAPI backend and an MIT App Inventor frontend.

Backend: FastAPI for Diabetes Prediction
The core of the prediction service is built using FastAPI, a modern, fast (high-performance) web framework for building APIs with Python 3.7+.

1. Setting up the Environment:

First, I installed the necessary libraries:

Bash

!pip install fastapi
!pip install uvicorn
!pip install pickle5
!pip install pydantic
!pip install scikit-learn
!pip install requests
!pip install pypi-json
!pip install pyngrok
!pip install nest-asyncio


2. API Initialization and Middleware:

The FastAPI application is initialized, and CORS middleware is added to handle cross-origin requests, allowing the frontend to communicate with the backend.

Python

from fastapi import FastAPI
from pydantic import BaseModel
import pickle
import json
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



A root endpoint was also created for a welcome message:

Python

@app.get("/")
def read_root():
    return {"message": "Welcome to my FastAPI app!"}


3. Defining the Input Model:

To ensure proper data validation for the prediction, a Pydantic BaseModel called model_input was defined. This specifies the expected data types for the input features of the diabetes prediction model:

Python

class model_input(BaseModel):
    Pregnancies: int
    Glucose: int
    BloodPressure: int
    SkinThickness: int
    Insulin: int
    BMI: float
    DiabetesPedigreeFunction: float
    Age: int


4. Loading the ML Model:

The pre-trained diabetes prediction model, saved as a pickle file, is loaded into the application:

Python

diabetes_model = pickle.load(open(r"c:/Users/user/Desktop/Machine learning pratice/Deploy diabtes prediction M", 'rb'))


5. Diabetes Prediction Endpoint:

A POST endpoint /diabetes_prediction was created to handle prediction requests. It receives input parameters, converts them into a list, and uses the loaded machine learning model to make a prediction. The result (diabetic or not diabetic) is then returned.

Python

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

    if (prediction[0] == 0):
        return 'The person is not diabetic'
    else:
        return 'The person is diabetic'


6. Exposing the API with Ngrok:

To make the local FastAPI application accessible from the internet, ngrok was used to create a public URL. This is crucial for the Android app to connect to the backend.

Python

ngrok_tunnel = ngrok.connect(8000)
print('Public URL:', ngrok_tunnel.public_url)
nest_asyncio.apply()
uvicorn.run(app, host="0.0.0.0", port=8000)


I also authorized ngrok with my authtoken.
