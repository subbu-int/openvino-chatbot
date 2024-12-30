# openvino-chatbot
An OpenVINO based chatbot that can be run on a windows laptop.
This is based on publicly available resources from Intel. Just added additional instructions for ease of use. 

# Reference links
https://docs.openvino.ai/2024/notebooks/llm-chatbot-with-output.html

# Setup Instructions
## Access Privilege required: 
1) Administrator access on Windows laptop
2) Hugging face account and access token
3) User agreement for specific Hugging face models

## Software to be downloaded and installed:
1) OpenVINO packages for windows (two sets of packages)
2) Gut for Windows
3) Visual Studio Basics C++ for Windows
4) Python 3.xx for Windows
5) Bunch of Python packages including Jupyter for loading the code

## Skills
1) Basic execution knowledge (running scripts, batch files).
2) Program execution of Python code in a Jupyter notebook.


## Code execution
1) Using Python Jupyter notebook
2) Understanding basic program syntax


## Steps:
### 1) Click windows start, search for "cmd" and open Command prompt with the "Run as administrator option"
Hereon, for any step that needs administrator privileges as "as admin", you will use this command prompt.

### 2) Click windows start, search for "cmd" and open Command prompt as user.
Hereon, for any step that needs basic user privileges as "as user", you will use this command prompt.

### 3) Download OpenVino Toolkit for Windows from https://docs.openvino.ai/2024/get-started/install-openvino/install-openvino-archive-windows.html (Step 2.)
Alternately, download link: https://storage.openvinotoolkit.org/repositories/openvino/packages/2024.1/windows/
(Advised to click the link in step 2 so that you will get the latest software).
Say this software is download to your "Downloads" folder.

### 4) Create an Intel directory "as admin", execute below command.
mkdir "C:\Program Files (x86)\Intel"

and move the downloaded OpenVINO packages to this directory, by running the command below.

move "C:\Users\<windows_username>\Downloads\w_openvino_toolkit_windows_2024.1.0.15008.f4afc983258_x86_64.zip" "C:\Program Files (x86)\Intel"
(Use appropriate folder location where the software is downloaded)

### 5) "As admin", Unbundle the OpenVINO archive, and rename the folder to openvino_2024.1.0, by running the below two commands.
tar -xf openvino_2024.1.0.zip
ren w_openvino_toolkit_windows_2024.1.0.15008.f4afc983258_x86_64 openvino_2024.1.0

### 6) "As admin" execute the following commands to install all the pre-requisite Python packages for OpenVINO
Disable VPN while running this command - python -m pip install -r .\python\requirements.txt (If you get a warning about pip being an older version, ignore)
"As user", execute the command "where python" on command prompt in windows and it will tell you if Python is installed and is visible in the PATH.
If Python is not installed, install the same from https://www.python.org/downloads/windows/

cd "C:\Program Files (x86)\Intel\openvino_2024.1.0"
python -m pip install -r .\python\requirements.txt

### 7) "Ad admin" create a soft link for the directory with a generic name in the parent folder "Intel"
cd C:\Program Files (x86)\Intel
mklink /D openvino_2024 openvino_2024.1.0

### 8) Initialise the environment by running the setupvars batch script (avoid powershell, for some reason it is unable to detect the Python install).
"As user" execute the following command:

"C:\Program Files (x86)\Intel\openvino_2024\setupvars.bat"
You need to run the command for each new Command Prompt window - which means, everytime you want to run the chatbot code, initialise the environment using this batch script.

### 9) Pre-req Software (second set of OpenVINO packages)
Goto OpenVino Dev Tools - https://docs.openvino.ai/2022.3/openvino_docs_install_guides_install_dev_tools.html
"As User", Execute the following commands to create a virtual environment. A virtual environment ensures packages for different requirements are installed. Each virtual environment is isolated from others ensuring SW compatibility as required.
I setup the virtual environment in my user folder "smeenak1".
python -m venv openvino_env
C:\Users\smeenak1\openvino_env\Scripts\activate
At this point, the prompt will be pre-fixed with "(openvino_env)", like

(openvino_env) C:\Users\smeenak1>

IMPORTANT: The directory from which you run the "python -m venv openvino_env", a folder "openvino_env" will be created. 
Remember this path as the activation command needs to be executed from here, the next time
(Run this command everytime you start the code, on the user command prompt)

### 10) Install additional python packages, "as user", in the same virtual environment. 
These commands will install the pre-requisite packages for OpenVINO, its integration with optimum and the hugging face CLI.
Optimum packages are required for Quantisation of the model (coverting the model bias & weights values to an optimal datatype, reducing the model size)
(no VPN)
pip install openvino openvino-dev openvino-nightly optimum optimum-intel gradio
python -m pip install --upgrade --upgrade-strategy eager optimum[openvino]
pip install -U "huggingface_hub[cli]"

#### 11) Go to https://github.com/openvinotoolkit/openvino_notebooks/wiki/Windows for setup instructions (additional software git, visual basic)
Git install package: https://github.com/git-for-windows/git/releases/download/v2.35.1.windows.2/Git-2.35.1.2-64-bit.exe
MS VB C++ Package: https://aka.ms/vs/16/release/vc_redist.x64.exe


### 12) Once these two packages are installed, enable VPN and clone the repository

git clone --depth=1 https://github.com/openvinotoolkit/openvino_notebooks.git
cd openvino_notebooks
Copy llm_config.py file from utils directory to notebooks/llm_chatbot directory


### 13) Create hugging face account and create an access token - https://huggingface.co/docs/hub/en/security-tokens & https://huggingface.co/settings/tokens
"As user", on Windows command prompt where the openvino virtual environment is activated, run the following commands

huggingface-cli login
Asks to open a token URL, open the same
Copy the access token created earlier
paste it at the prompt and press enter

### 14) https://huggingface.co/meta-llama/Meta-Llama-3-8B-Instruct model requires an agreement to be submitted for approval (wait for a day or two for the approval to come through), else the jupyter notebook will fail
Alternately, We use try other models that is supporte by openvino

### 15) "As user", got to the git folder:
openvino_notebooks/notebooks/llm-chatbot

execute the following command:

jupyter lab llm-chatbot.ipynb
If the Jupyter notebook does not open by itself, click (Ctrl+Click) the link on the messages (starting with 127.0.0.0 IP or localhost hostname)

### 16) This will open the llm-chatbot notebook in your browser.
Each cell in the notebook is executed by running "Shift+enter"

### 17) The cell which runs the optimum cli command, initially does not respond with output immediately. So wait for a few minutes before the output is displayed.
After the optimisation, a new set of folders will be created under the directory where notebook resides and will have the optimised model
For e.g., Dir path: llm-chatbot\llama-3-8b-instruct\INT4_compressed_weights
Contents: (Optimised model is openvino_model.bin, ~5GB)
             (705) config.json
             (206) generation_config.json
   (5,332,841,537) openvino_model.bin
       (4,044,735) openvino_model.xml
             (312) special_tokens_map.json
       (9,085,698) tokenizer.json
          (53,039) tokenizer_config.json

### 18) A cell that is currently executing will have "[*]" prefixed.
If execution completes, a nnumerical value will be displayed. This increments everytime a cell is executed and the subsequent cells are continuously numbered.

### 19) Towards the end:
The session close section needs to be uncommented and executed to ensure the session is cleaned up (use this only when you want to close the chatbot fully).

### 20) To end the chatbot notebook:
Follow step 19, close the browser tab which runs the notebook and press "ctrl+c" twice on the command prompt that started the notebook.
