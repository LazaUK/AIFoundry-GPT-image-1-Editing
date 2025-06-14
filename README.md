# Image Editing with Azure OpenAI GPT-image-1

This repo demonstrates how to use Azure OpenAI **GPT-image-1**, a powerful image generation model in Azure AI Foundry. The provided Jupyter notebook showcases image editing capabilities, allowing you to modify existing images based on textual prompts.

This demo leverages the *requests* library for direct API interaction and *azure-identity* library for secure Azure Entra ID authentication.

___
🎧 You may listen to the audio podcast, ["Image Makeovers with GPT-image-1: Behind the Code"](https://youtu.be/heyLTbfDo6k), where we explore the content of this notebook in a beginner-friendly way.
___

## 📑 Table of Contents:
* [Part 1: Configuring Solution Environment](#part-1-configuring-solution-environment)
* [Part 2: Performing Image Edits and Visualisation](#part-2-performing-image-edits-and-visualisation)

---

## Part 1: Configuring Solution Environment
To use the notebook, set up your Azure OpenAI environment and install Python packages.

### 1.1 Azure OpenAI Service Setup
Ensure you have an **Azure OpenAI Service** resource with the **GPT-image-1 model** deployed.

### 1.2 Authentication
This demo uses **Azure Entra ID** authentication via `DefaultAzureCredential` from `azure.identity`. This credential type automatically handles various authentication methods.

To enable this, ensure your environment is set up for Azure authentication, e.g., by logging in via `az login` (Azure CLI) or setting relevant environment variables for service principals.

### 1.3 Environment Variables
Set the following environment variables, pointing to your Azure OpenAI GPT-image-1 deployment:

| Environment Variable     | Description                                                                             |
| :----------------------- | :-------------------------------------------------------------------------------------- |
| AOAI_API_BASE            | Your Azure OpenAI endpoint URL (e.g., https://<YOUR_AOAI_RESOURCE>.openai.azure.com).   |
| AOAI_DEPLOYMENT_NAME     | The name of your GPT-image-1 deployment (e.g., gpt-image-1).                            |
| AOAI_API_VERSION         | The API version for image edits (e.g., 2025-04-01-preview).                             |

### 1.4 Install Required Python packages
``` Bash
pip install requests Pillow ipython azure-identity
```

## Part 2: Performing Image Edits and Visualisation
The `GPT-image-1.ipynb` notebook outlines the core image editing logic:

* **API URL Construction:** Builds the endpoint URL using environment variables.
* **Input Image:** Loads the image from `INPUT_IMAGE_PATH` (e.g., `./input_image.jpeg`).
* **Secure Authentication:** Obtains an access token using `DefaultAzureCredential`.
* **API Request:** Sends a `multipart/form-data` POST request to the GPT-image-1 API, including:
    * The input image file.
    * A `prompt` string (e.g., `"Add a giant, friendly robot behind the bird."`).
    * Parameters like `n` (number of images, typically 1) and `size` (e.g., `1024x1024`), along with other options.
* **Response Processing:** On success, it decodes the base64-encoded edited image, saves it to `OUTPUT_IMAGE_PATH` (e.g., `./output_image_edited.png`), and displays it.
* **Error Handling:** Includes robust error reporting for API failures.

Example API call structure from the notebook:

``` Python
# Input image
files = {
    "image": (
        os.path.basename(INPUT_IMAGE_PATH),
        image_file,
        "image/jpeg")
}

# Input prompt and parameters
data = {
    "prompt": PROMPT,
    "n": 1,
    "size": "1024x1024",
    "quality": "medium",
    "output_compression": 100,
    "output_format": "jpeg"
}

# Entra ID Auth
credential = DefaultAzureCredential()
token = credential.get_token("https://cognitiveservices.azure.com/.default")
headers = {
    "Authorization": f"Bearer {token.token}"
}

# Image API request
print("Making API call to edit the image...")
response = requests.post(
    API_URL,
    headers = headers,
    files = files,
    data = data
)
```

> [!NOTE]
> Feel free to modify the PROMPT string to experiment with different image editing requests.

### Example Input:
This is an example of an input image that could be used for editing in this notebook.
![Input_Image](images/input_image.jpeg)

### Example Output:
Below is an example of an edited image, based on an input image and a prompt like "`Add a giant, friendly robot behind the bird.`"
![Output_Image](images/output_image_edited.jpeg)

> [!NOTE]
> Your actual output may vary.
