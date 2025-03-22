## Integration of a Mathematical Calulations with a Chat Completion System using LLM Function-Calling

### AIM:
To design and implement a Python function for calculating the volume of a cylinder, integrate it with a chat completion system utilizing the function-calling feature of a large language model (LLM).

### PROBLEM STATEMENT:
To calculate Volume of Cylinder by using LLM Function-Calling by passing arguments through function call and display volume in JSON format.

### DESIGN STEPS:

#### STEP 1:
Import required libraries: os, openai, json, math. Load the API key securely using dotenv to access OpenAI.

#### STEP 2:
Create the volume_of_cylinder(radius, height, unit="meter") function.Calculate volume using the formula: 𝑉=𝜋×radius^2×height. Store the result in a dictionary with radius, height, volume, and unit. Return the data in JSON format.

#### STEP 3:
Create a user message to request cylinder volume calculation and show the calculated volume in JSON format. Send a request to openai.ChatCompletion.create(), including:
1. Model: "gpt-3.5-turbo"
2. Messages: User input
3. Functions: Defined function metadata
4. Function Call: Explicitly request "volume_of_cylinder"
   
### PROGRAM:
```
import os
import openai

from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv()) # read local .env file
openai.api_key = os.environ['OPENAI_API_KEY']
```
```
import json
import math
def volume_of_cylinder(radius, height, unit="meter"):
    volume = math.pi * (radius * radius) * height
    cylinder_info = {
        "radius": radius,
        "height": height,
        "volume": volume,
        "unit": unit
    }
    return json.dumps(cylinder_info)
```
```
functions = [
    {
        "name": "volume_of_cylinder",
        "description": "Calculates the volume of a cylinder.",
        "parameters": {
            "type": "object",
            "properties": {
                "radius": {
                    "type": "integer",
                    "description": "Radius of the cylinder.",
                },
                "height": {
                    "type": "integer",
                    "description": "Height of the cylinder.",
                },
                "unit": {
                    "type": "string",
                    "enum": ["meter", "centimeter"],
                    "description": "Unit of measurement (meter or centimeter).",
                },
            },
            "required": ["radius", "height"],
        },
    }
]
```
```
radius = float(input("Enter the radius of the cylinder (in meters): "))
height = float(input("Enter the height of the cylinder (in meters): "))

messages = [
    {
        "role": "user",
        "content": f"Calculate the volume of a cylinder with radius {radius} meters and height {height} meters."
    }
]
```
```
import openai
response = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    messages=messages,
    functions=functions,  # Use 'functions' for function calling
    function_call={"name": "volume_of_cylinder"}  # Allows automatic function calling
)
```
```
print(response)
response_message = response["choices"][0]["message"]

if "function_call" in response_message:
    function_call = response_message["function_call"]
    print("Function Call:", function_call)
else:
    print("No function call. Assistant responded with text:", response_message["content"])
```
```
response_message
response_message["content"]
response_message["function_call"]
args = json.loads(response["choices"][0]["message"].get("function_call", {}).get("arguments", "{}"))

if not args:
    radius = float(input("Enter radius: "))
    height = float(input("Enter height: "))
    unit = input("Enter unit (default is meter): ") or "meter"
    args = {"radius": radius, "height": height, "unit": unit}

observation = volume_of_cylinder(**args)

print("Cylinder Volume Data:", observation)
```

### OUTPUT:
![image](https://github.com/user-attachments/assets/6ab43672-9fe7-49b9-90e5-c38c6ebb2b46)
![image](https://github.com/user-attachments/assets/283ea783-1235-4546-a11e-921c3048c311)
![image](https://github.com/user-attachments/assets/efefb8a4-f1a4-43e9-8ef9-3418408b2af2)
![image](https://github.com/user-attachments/assets/35266c03-c0bc-417c-a403-e3ecf9913b29)


### RESULT:
Thus, The integration of a Mathematical Calulations with a Chat Completion System using LLM Function-Calling is executed successfully.
