## Integration of a Mathematical Calulations with a Chat Completion System using LLM Function-Calling

### AIM:
To design and implement a Python function for calculating the volume of a cylinder, integrate it with a chat completion system utilizing the function-calling feature of a large language model (LLM).

### PROBLEM STATEMENT:
Design and implement a Python program that calculates the volume of a cylinder by integrating a mathematical function with a chat completion system using the function-calling feature of a large language model (LLM). The program should extract the cylinder dimensions (radius and height) from the LLM's response, perform the calculation, and display the result.
### DESIGN STEPS:

#### STEP 1: 
Import the necessary libraries and create a function to calculate the volume

#### STEP 2:
Define the function schema, create message prompts, and call the LLM using openai.ChatCompletion.create() with the model, messages, and functions.

#### STEP 3:
Extract the radius and height from the LLM response, convert them to floats, and pass them to the volume_of_cylinder() function to calculate and display the volume.

### PROGRAM:
```
import os
import openai
import math

from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv()) # read local .env file
openai.api_key = os.environ['OPENAI_API_KEY']

def volume_of_cylinder(radius,height):
    volume=math.pi*(radius**2)*height
    return volume

functions = [
    {
        "name": "calculate_cylinder_volume",
        "description": "Calculate the volume of a cylinder",
        "parameters": {
            "type": "object",
            "properties": {
                "radius": {"type": "number", 
                           "description": "Radius of the cylinder"},
                "height": {"type": "number", 
                           "description": "Height of the cylinder"},
            },
            "required": ["radius", "height"],
        }
    }
]

messages = [
    {
        "role": "user", 
        "content": "What is the volume of a cylinder with radius 5 and height 10?"
    }
]

response = openai.ChatCompletion.create(
    model="gpt-4-turbo",
    messages=messages,
    functions=functions
)
print(response)

response_message = response["choices"][0]["message"]
print(response_message)

args=response_message["function_call"]["arguments"]
print(args)

args = args.strip("{}").replace('"', '').split(",")
radius = float(args[0].split(":")[1])
height = float(args[1].split(":")[1])

volume_of_cylinder(radius, height)

messages=[
    {
        "role": "user",
        "content": "hello",
    }
]
response = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    messages=messages,
    functions=functions,
)
print(response)
```

### OUTPUT:
ARGUMENTS:

![image](https://github.com/user-attachments/assets/682e2218-17ab-48d3-94f0-f60a32ec160b)

RESPONSE:

![image](https://github.com/user-attachments/assets/1e72b3f2-2be6-431d-bbc8-33971817f7e3)

CALCULATED VOLUME:

![image](https://github.com/user-attachments/assets/be29c83b-44b3-44ca-85bf-80d7cc218b9a)
### RESULT:
Thus, a Python function for calculating the volume of a cylinder integrated with a chat completion system utilizing the function-calling feature of a large language model (LLM) is implemented successfully.
