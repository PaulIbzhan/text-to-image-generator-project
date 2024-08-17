Prerequisites

Before you start, ensure you have the following:

Python 3.9 or higher.
Hugging Face Account.
 

Step 1: Set Up the Development Environment

In your project directory, create a file named requirements.txt and add the following dependencies to the file:

 

certifi==2022.9.14  
charset-normalizer==2.1.1  
colorama==0.4.5  
customtkinter==4.6.1  
darkdetect==0.7.1  
diffusers==0.3.0  
filelock==3.8.0  
huggingface-hub==0.9.1  
idna==3.4  
importlib-metadata==4.12.0  
numpy==1.23.3  
packaging==21.3  
Pillow==9.2.0  
pyparsing==3.0.9  
PyYAML==6.0  
regex==2022.9.13  
requests==2.28.1  
tk==0.1.0  
tokenizers==0.12.1  
torch==1.12.1+cu113  
torchaudio==0.12.1+cu113  
torchvision==0.13.1+cu113  
tqdm==4.64.1  
transformers==4.22.1  
typing_extensions==4.3.0  
urllib3==1.26.12  
zipp==3.8.1
 

To install the listed dependencies in the requirements.txt file, run the following command in your terminal:

 

pip install -r requirements.txt
 

 

Step 2: Configure Authentication

In your project directory, create a file named authtoken.py and add the following code to the file:

 

auth_token = "ACCESS TOKEN FROM HUGGING FACE"
 

To obtain access token from Hugging Face, follow these steps:

Log in to your Hugging Face account.
thumbnail image 2 of blog post titled 
	
	
	 
	
	
	
				
		
			
				
						
							Build your own AI Text-to-Image Generator in Visual Studio Code
							
						
					
			
		
	
			
	
	
	
	
	

 Go to your profile settings and select Access Tokens
thumbnail image 3 of blog post titled 
	
	
	 
	
	
	
				
		
			
				
						
							Build your own AI Text-to-Image Generator in Visual Studio Code
							
						
					
			
		
	
			
	
	
	
	
	

Click on Create new token.
Choose the token type as Read.
Enter Token name and click Create token.
thumbnail image 4 of blog post titled 
	
	
	 
	
	
	
				
		
			
				
						
							Build your own AI Text-to-Image Generator in Visual Studio Code
							
						
					
			
		
	
			
	
	
	
	
	

 Copy the generated token and replace ACCESS TOKEN FROM HUGGINGFACE in authtoken.py file with your token.
 

Step 3: Develop the Application

In your project directory, create a file named application.py and add the following code to the file:

 

# Import the Tkinter library for GUI
import tkinter as tk

# Import the custom Tkinter library for enhanced widgets
import customtkinter as ctk  

# Import PyTorch for handling tensors and model
import torch  

# Import the Stable Diffusion Pipeline from diffusers library
from diffusers import StableDiffusionPipeline 

# Import PIL for image handling
from PIL import Image, ImageTk 

# Import the authentication token from a file
from authtoken import auth_token  


# Initialize the main Tkinter application window
app = tk.Tk()

# Set the size of the window
app.geometry("532x632") 

# Set the title of the window
app.title("Text-to-Image Generator")  

# Set the appearance mode of customtkinter to dark
ctk.set_appearance_mode("dark")  

# Create an entry widget for the prompt text input
prompt = ctk.CTkEntry(height=40, width=512, text_font=("Arial", 20), text_color="black", fg_color="white")

# Place the entry widget at coordinates (10, 10)
prompt.place(x=10, y=10)  

# Create a label widget for displaying the generated image
lmain = ctk.CTkLabel(height=512, width=512)

# Place the label widget at coordinates (10, 110)
lmain.place(x=10, y=110)  

# Define the model ID for Stable Diffusion
modelid = "CompVis/stable-diffusion-v1-4"  

# Define the device to run the model on
device = "cpu" 

# Load the Stable Diffusion model pipeline
pipe = StableDiffusionPipeline.from_pretrained(modelid, revision="fp16", torch_dtype=torch.float32, use_auth_token=auth_token)

# Move the pipeline to the specified device (CPU)
pipe.to(device)  

# Define the function to generate the image from the prompt
def generate():

    # Disable gradient calculation for efficiency
    with torch.no_grad():  
        
        # Generate the image with guidance scale
        image = pipe(prompt.get(), guidance_scale=8.5)["sample"][0]  

    # Convert the image to a PhotoImage for Tkinter
    img = ImageTk.PhotoImage(image)  

    # Keep a reference to the image to prevent garbage collection
    lmain.image = img 

    # Update the label widget with the new image
    lmain.configure(image=img)  

# Create a button widget to trigger the image generation
trigger = ctk.CTkButton(height=40, width=120, text_font=("Arial", 20), text_color="white", fg_color="black", command=generate)

# Set the text on the button to "Generate"
trigger.configure(text="Generate")  

# Place the button at coordinates (206, 60)
trigger.place(x=206, y=60)  

# Start the Tkinter main loop
app.mainloop()
 

To run the application, execute the following command in your terminal:

 

python application.py
 

This will launch the GUI where you can enter a text prompt and generate corresponding images by clicking the Generate button.

 

Congratulations! You have successfully built an AI Text-to-Image Generator using Stable Diffusion in Visual Studio Code. Feel free to explore and enhance the application further by adding new features and improving the user interface. Happy coding!
