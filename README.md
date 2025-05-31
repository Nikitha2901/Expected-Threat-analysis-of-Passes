### Threat Analysis of Final Third Passes Using Logistic, xNN, and LLM Approaches for Coach Interpretation

## Description

This project is about analysing the how safe or dangrerous the passes happening in final-third could be. The pass is classified as safe or dangerous based Expected goal(xG) whose threshold is 0.06. And the metric to analyse the threat is Expected Threat(xT) which is probability of pass being a shot. The different model approaches used are : 

- Logistic regression
- Explainable neural network with unique features
- Explainable neural network with combination of features

## Environment set up
This application was made with Streamlit.  To run locally, first create .streamlit/secrets.toml with keys, etc... then run:
```bash
conda create --name streamlit_env
conda activate streamlit_env
pip install -r requirements.txt
streamlit run app.py
```
Once you have made changes to the code, save, move focus to the streamlit tab, then press c to clear caches if necessary, then r to rerun. 

You also need to have access to GPT API to use this package. Alternatively, you need access to Gemini API but that requires changes to the [.streamlit/secrets.toml](.streamlit/secrets.toml) file (see below).

## How does it work?
### App
Streamlit reruns the code every time the user interacts with the app. This code is located in app.py. The user selects a player and the visual and word report starts to generate.
The application builds primarily around five classes: data_sources, visual, description, chat. We now describe these in turn.

### Data sources

The code data_sources.py consists of three classes:

**class Data()**: Gets, processes and manage various forms of data. The data is primarily stored in data.df
**class Pass()**: This consist of contributions calculated for features that influence xT for logistic regression and Explainable neural network approaches and different models being loaded. The models are saved in folder data.

### Visual

There is quite a lot of code here, but it is primarily about making nice visuals of passes. There are different visuals codes for pass visualization and contribution plot visualization of different model approaches.

### Description

It is in this part of the code where we start doing something novel. The three most important functions for creating a text are:

**get_intro_messages()**: This sets up the bot and explains to it what it does.
**synthesize_text()**: This converts the stats.df to a description in words of what the data says.
**get_prompt_messages()**: This is the prompt which tells GPT3 or GPT4 how to use the texts supplied.

A key to success of prompting lies in two types of files, known as describe and gpt_example files. These are given for this application in 
data/describe/passes.xlsx
and
data/gpt_examples/passes.xlsx

By clicking on the expander in the Description messages you can see how they have been used to construct a prompt to GPT4. It is this prompt which then generates the text under the figure.

Different intresrting text is generated of each model with a different interpretation

### Using Open AI API
To use Open AI you need a API key. Then you need to add the following lines to your [.streamlit/secrets.toml](.streamlit/secrets.toml) file.

```toml
USE_GEMINI = false
GPT_BASE = "address of you deployment of Chat GPT"
GPT_VERSION = "version date"
GPT_KEY = "your key"
GPT_ENGINE = "model name"
```

### Using Gemini API
If, instead of using OpenAI's API, you want to use Google's. You need to add the following lines to your [.streamlit/secrets.toml](.streamlit/secrets.toml) file.

```toml
USE_GEMINI = true
GEMINI_API_KEY = "YOUR_API_KEY"

# Can use any chat model
GEMINI_CHAT_MODEL = "gemini-1.5-flash"

# Can use any embedding model
GEMINI_EMBEDDING_MODEL = "models/text-embedding-004"
```

### The file path of different logistic and xNN approach models trained are as below : 

- Models_trained/xNN_model_unique_feature_set.ipynb
- Models_trained/xNN_model_combination_features.ipynb
- Models_trained/logistic_model_LLM.ipynb

### The code of data preprocessing and feature engineering is in below file path : 
- /data/frames_player_missing_analysis_2022.ipynb
- /data/2023_missing_players.ipynb
- /data/adding_teamname_playername_2022.ipynb
- /data/features_generation_2022_2023.ipynb

**Note**: The code snippet of feature generation and preprocessing is done for whole dataset in local. The whole dataset is not in github due to privacy reasons. In git datasets of one match per season is uploaded which is df_passes.csv(event data) and tracking.csv(tracking data), is present in folder data.



