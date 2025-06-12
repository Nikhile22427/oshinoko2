---
layout: post
title: Final review
description: Trimester 3 final 
type: collab
comments: false
permalink: /csse/documentation3
---
# Stuff I did this trimester
..definitely not nothing...

### A small overview of my role in the team
- Lead morale increaser
- Documenter
- Bugfixer
- Lead artist (background)

## Two Player

- created 1st and 2nd versions of the final fix that enabled two player
- setup sprites for use

## Documentation

- I was on the "notebook creating" sub-team
- Assisted Jim in creating lesson notebooks for our API and two-player lessons
- Created documentation of our changes to the adventure game using <b><i>Tailwind CSS<i><b>

## Debugging

- Debugging is something that nobody wants to do
- And so naturally I learned how to interpret these alien error messages
- I eventually found it easier to look at the errors, use the debugger, and comb through files myself rather than ask any LLM

- However, I still have much to learn (this breakpoint stuff confuses me)

## The Words

you wanted us to learn... <br>

<b>Software Engineering Practices</b><br>
Planning changes - Systematically analyzing and documenting modifications before implementation<br>
Checklists - Standardized lists of tasks or requirements to ensure consistency and completeness<br>
Burndowns - Visual charts tracking work completion over time in project management<br>
Coding with comments - Writing explanatory text within code to document functionality and purpose<br>
Building help documentation - Creating user guides and technical documentation for software systems<br>
<b>Software Development Lifecycle Practices</b><br>
Source control - Version management systems that track changes to code over time<br>
Forking - Creating an independent copy of a repository to develop separately<br>
Branching - Creating parallel development paths within a repository<br>
Building - Compiling and assembling source code into executable applications<br>
Testing and verification - Validating that software functions correctly and meets requirements<br>
Pull requests - Formal requests to merge code changes from one branch to another<br>
Merging/integrating - Combining code changes from different branches or contributors<br>
Deployment - Publishing and installing software to production environments<br>
<b>Retrospective Engineering Practices</b><br>
Presentation - Formal demonstration of completed work to stakeholders<br>
Live reviews - Real-time examination and discussion of code or systems<br>
Demos - Interactive demonstrations of software functionality<br>
Code reviews - Systematic examination of source code by peers for quality assurance<br>
Revising plans - Updating project plans based on lessons learned and new requirements<br>
<b>Data Types</b><br>
Numbers - Numeric values including integers and floating-point numbers<br>
Strings - Text data represented as sequences of characters<br>
Booleans - True/false logical values<br>
Arrays - Ordered collections of elements accessible by index<br>
JSON objects - Data structures using JavaScript Object Notation for key-value pairs<br>
<b>Operators</b><br>
String operations - Functions for manipulating text (concatenation, substring, search)<br>
Mathematical operations - Arithmetic functions like addition, subtraction, multiplication, division<br>
Boolean expressions - Logical operations using AND, OR, NOT to evaluate true/false conditions<br>
Control Structures<br>
Iteration - Repeating code execution using loops (for, while, do-while)<br>
Conditions - Executing code based on true/false evaluations (if/else statements)<br>
Nested conditions - Conditional statements placed inside other conditional statements<br>
<b>Classes</b><br>
Writing classes - Defining blueprints for objects with properties and methods<br>
Creating methods - Writing functions that belong to a class<br>
Instantiating objects - Creating specific instances of a class<br>
Using objects - Accessing and manipulating object properties and methods<br>
Calling methods - Executing functions defined within an object<br>
Parameters - Input values passed to functions or methods<br>
Return values - Output values returned by functions or methods<br>
<b>Coding Practices</b><br>
SRP - Single Responsibility Principle: each class should have one reason to change<br>
Object Literal - Creating objects using curly brace notation with key-value pairs<br>
Object Instance - A specific occurrence of a class with its own property values<br>
FSMs in Game - Finite State Machines used to manage game states and transitions<br>
Inheritance - Mechanism allowing classes to inherit properties and methods from parent classes<br>

## Reflection
- This class teaches skills that are applicable anywhere where you work with computers or code
- Also teaches collaboration skills
- I learned how to control my anger (at computers)
- I learned how to effectively work with other people to maximize working power and minimize downtime and faffing around

## Analytics
here are the number of commits I have made over the class and the number of lines I have changed:<br>
![analytics](analytics.png)
well... I probably could have inflated this number by committing more...<br>
I think I went a week without committing once and lost all my progress...

## Unrelated things

- Worked on a passion project not using JS
- Used YOLO for object detection and wrote a python file that would interpret these detections and display them on the screen.
- It would also display and output coordinates for use with some kind of grabbing apparatus (didn't get to this part b/c it would cost a lot to build a robot arm)


code below


```python
import cv2
import torch
import pyttsx3
from vosk import Model, KaldiRecognizer
import numpy as np
import wave 
from ultralytics import YOLO
import pyaudio
import json
import time

model_path = "/Users/evansvetina/Downloads/vosk-model-small-en-us-0.15"  # Update this path with your model directory

vosk_model = Model(model_path)
recognizer = KaldiRecognizer(vosk_model, 16000)

# Global variable to store detected objects between frames
detected_objects = []

def init_model():
    """Initialize the YOLO model"""
    try:
        # Load the YOLOv5 model
        model = YOLO('/Users/evansvetina/Downloads/best (1).pt')
        model.conf = 0.25
        model.iou = 0.45
        return model
    except Exception as e:
        print(f"Error loading model: {e}")
        return None

def init_camera():
    """Initialize the camera"""
    cap = cv2.VideoCapture(0)
    if not cap.isOpened():
        print("Error: Could not open webcam.")
        return None
    return cap

def listen_for_command():
    """
    Listen for voice command using Vosk
    Listens for up to 3 seconds for a command
    """
    p = None
    stream = None
    try:
        p = pyaudio.PyAudio()
        
        # List available input devices
        info = p.get_host_api_info_by_index(0)
        numdevices = info.get('deviceCount')
        
        # Find the first available input device
        input_device_index = None
        for i in range(numdevices):
            if p.get_device_info_by_host_api_device_index(0, i).get('maxInputChannels') > 0:
                input_device_index = i
                break
        
        if input_device_index is None:
            print("No input devices found!")
            return None
            
        # Configure and open stream with specific device
        stream = p.open(
            format=pyaudio.paInt16,
            channels=1,
            rate=16000,
            input=True,
            input_device_index=input_device_index,
            frames_per_buffer=4096
        )
        
        stream.start_stream()
        
        # Set maximum listening time to 3 seconds
        start_time = time.time()
        max_listen_time = 3  # Listen for up to 3 seconds
        
        try:
            while time.time() - start_time < max_listen_time:
                data = stream.read(4096, exception_on_overflow=False)
                
                if recognizer.AcceptWaveform(data):
                    result_json = recognizer.Result()
                    # Parse the JSON string and extract the text
                    result_dict = json.loads(result_json)
                    result = result_dict.get('text', '')
                    if result:  # Only return if we got an actual command
                        return result
            
            # If we reach here, we didn't get a command in the time allotted
            return None
                    
        except Exception as e:
            print(f"Error recording audio: {e}")
            return None
            
    except Exception as e:
        print(f"Error initializing audio: {e}")
        return None
    finally:
        if stream is not None and stream.is_active():
            stream.stop_stream()
            stream.close()
        if p is not None:
            p.terminate()
        
        
def process_frame(frame, model):
    """Process a single frame with object detection"""
    global detected_objects  # Make this a global variable to persist between frames
    
    # Only reset the detected objects list if new objects are found
    current_detected_objects = []
    
    try:
        # Check if model is None
        if model is None:
            return frame, detected_objects
            
        # Perform inference
        results = model(frame, verbose=False)
        
        # Process detections
        for r in results:
            boxes = r.boxes
            for box in boxes:
                # Get box coordinates
                x1, y1, x2, y2 = box.xyxy[0].cpu().numpy()
                x1, y1, x2, y2 = int(x1), int(y1), int(x2), int(y2)
                
                # Calculate center
                center_x = (x1 + x2) // 2
                center_y = (y1 + y2) // 2
                
                # Draw bounding box (green)
                cv2.rectangle(frame, (x1, y1), (x2, y2), (0, 255, 0), 2)
                
                # Draw center point (red)
                cv2.circle(frame, (center_x, center_y), 5, (0, 0, 255), -1)
                
                # Add labels
                conf = float(box.conf[0])
                cls = int(box.cls[0])
                class_name = model.names[cls]
                label = f"{class_name} {conf:.2f}"
                
                # Store the detected object
                current_detected_objects.append({
                    'class': class_name,
                    'center': (center_x, center_y),
                    'confidence': conf  
                })
                
                # Draw label (white)
                cv2.putText(frame, label, (x1, y1 - 10), 
                           cv2.FONT_HERSHEY_SIMPLEX, 0.5, (255, 255, 255), 2)
                
                # Draw center coordinates (yellow)
                center_text = f"({center_x}, {center_y})"
                cv2.putText(frame, center_text, (center_x + 10, center_y - 10), 
                           cv2.FONT_HERSHEY_SIMPLEX, 0.5, (0, 255, 255), 2)
        
        # Only update detected_objects if we found objects
        if current_detected_objects:
            detected_objects = current_detected_objects
        
        return frame, detected_objects
    except Exception as e:
        return frame, detected_objects  # Return the frame and empty list


# Initialize model
model = init_model()

# Initialize camera
cap = init_camera()

# Initialize the speech recognizer once to avoid reopening it repeatedly

   
# Use time-based intervals instead of frame-based intervals
import time
last_voice_check_time = time.time()
VOICE_CHECK_INTERVAL_SECONDS = 5  # Check for voice commands every 5 seconds


try:
    while True:
        # Read frame
        ret, frame = cap.read()
        if not ret:
            break
            
        # Process frame
        processed_frame, detected_objects = process_frame(frame, model)

        # Display frame
        cv2.imshow('YOLOv5 Object Detection', processed_frame)

        # Check for voice commands every 5 seconds
        current_time = time.time()
        if current_time - last_voice_check_time >= VOICE_CHECK_INTERVAL_SECONDS:
            last_voice_check_time = current_time
            
            # Check for voice commands
            command = listen_for_command()
            
            # Process command if we got one
            if command:
                found_match = False
                # Convert command to lowercase for case-insensitive comparison
                command_lower = command.lower().strip()
                
                # Try a more flexible matching approach
                for obj in detected_objects:
                    # Get the object class and make it lowercase
                    obj_class = obj['class'].lower().strip()
                    
                    # Check for partial matches in either direction
                    if (command_lower in obj_class) or (obj_class in command_lower) or \
                       ('bandage' in command_lower and 'bandage' in obj_class) or \
                       ('scissor' in command_lower and 'scissor' in obj_class):
                        
                        # Format the center coordinates as x,y,z with y always 0
                        center_x, center_y = obj['center']
                        print(f"{center_x},0,{center_y}")
                        found_match = True
                        break  # Exit after first match
                
                if not found_match:
                    print(f"No matching object found for the command '{command}'.")
        
        # Process key presses
        key = cv2.waitKey(1) & 0xFF
        
        # Check for quit command
        if key == ord('q'):
            break

except Exception as e:
    pass

finally:
    # Clean up
    if cap is not None:
        cap.release()
    cv2.destroyAllWindows()
```

## Unrelated things 2

- gained appreciation for game devs (man this coding stuff is hard)
- gained appreciation for artists (man this drawing stuff is hard)
