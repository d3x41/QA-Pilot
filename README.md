<p align="center">
  <img src="https://github.com/reid41/QA-Pilot/assets/25558653/4b45b525-5fac-4a3c-94e9-46364bdb36c3" alt="qa-pilot">
</p>

QA-Pilot is an interactive chat project that leverages online/local LLM for rapid understanding and navigation of GitHub code repository.

### Features

* Chat with github public repository with git clone way
* Store the chat history 
* Easy to set the configuration
* Multiple chat sessions
* Locate the session quicly with search function
* Integrate with `codegraph` to view the python file
* Support the different LLM models
    * ollama(deepseek, llama3.1, phi3, llama3, gemma2)
    * openai(gpt-4o, gpt-4-turbo, gpt-4, and gpt-3.5-turbo)
    * mistralai(mistral-tiny, mistral-tiny, mistral-small-latest, mistral-medium-latest, mistral-large-latest, codestral-lates)
    * localai(gpt-4, more)
    * zhipuai(glm-4-0520, glm-4, glm-4-air, glm-4-airx,  glm-4-flash)
    * anthropic(claude-3-opus-20240229, claude-3-sonnet-20240229, claude-3-haiku-20240307, claude-3-5-sonnet-20240620)
    * llamacpp
    * nvidia(meta/llama3-70b-instruct, more)
    * tongyi(qwen-turbo, qwen-plus, qwen-max, more)
    * moonshot(moonshot-v1-8k, moonshot-v1-32k, moonshot-v1-128k)


### Release

* 2024-07-03 update langchain to `0.2.6` version and add `moonshot` API support

* 2024-06-30  add `Go Codegraph`

* 2024-06-27  add `nvidia/tongyi` API support

* 2024-06-19  add `llamacpp` API support, improve the `settings` list in the sidebar and add upload model function for `llamacpp`, add `prompt templates` setting

* 2024-06-15  add `anthropic` API support, refactor some functions, and fix chat show messages

* 2024-06-12  add `zhipuai` API support

* 2024-06-10 Convert `flask` to `fastapi` and add `localai` API support

* 2024-06-07 Add `rr:` option and use `FlashRank` for the search 

* 2024-06-05 Upgrade `langchain` to `v0.2` and add `ollama embeddings`

* 2024-05-26 Release v2.0.1: Refactoring to replace `Streamlit` fontend with `Svelte` to improve the performance.

### Disclaimer

* This is a test project to validate the feasibility of a fully local solution for question answering using LLMs and Vector embeddings. It is not production ready, and it is not meant to be used in production. 
* `Do not use models for analyzing your critical or production data!!`
* `Do not use models for analyzing customer data to ensure data privacy and security!!`
* `Do not use models for analyzing you private/sensitivity code respository!!`

#### QA-Pilot
![qa-demo-new](https://github.com/reid41/QA-Pilot/assets/25558653/8198730f-32ec-4664-a10c-43b3f40c99ad)


#### CodeGraph
![code-graph-new](https://github.com/reid41/QA-Pilot/assets/25558653/8c47ea00-d703-42b5-b43b-d40796e7de1d)

To deploy QA-Pilot, you can follow the below steps:

1. Clone the QA-Pilot repository:

```shell
git clone https://github.com/reid41/QA-Pilot.git
cd QA-Pilot
```

2. Install [conda](https://www.anaconda.com/download) for virtual environment management. Create and activate a new virtual environment.

```shell
conda create -n QA-Pilot python=3.10.14
conda activate QA-Pilot
```

3. Install the required dependencies:

```shell
pip install -r requirements.txt
```

4. Install the pytorch with cuda [pytorch](https://pytorch.org/get-started/locally/)

5. Setup providers

* For setup [ollama website](https://ollama.com/) and [ollama github](https://github.com/ollama/ollama) to manage the local LLM. 
e.g.

```shell
ollama pull <model_name>

ollama list
```

* For setup [localAI](https://localai.io/) and [LocalAI github](https://github.com/mudler/LocalAI) to manage the local LLM, set the localAI `base_url` in config/config.ini.
e.g.
```shell
docker run -p 8080:8080 --name local-ai -ti localai/localai:latest-aio-cpu
# Do you have a Nvidia GPUs? Use this instead
# CUDA 11
# docker run -p 8080:8080 --gpus all --name local-ai -ti localai/localai:latest-aio-gpu-nvidia-cuda-11
# CUDA 12
# docker run -p 8080:8080 --gpus all --name local-ai -ti localai/localai:latest-aio-gpu-nvidia-cuda-12

# quick check the service with http://<localAI host>:8080/
# quick check the models with http://<localAI host>:8080/models/
```

* For setup llamacpp with [llama-cpp-python](https://github.com/abetlen/llama-cpp-python#windows-remarks)
  - upload the model to `llamacpp_models` dir or upload from the `llamacpp models` under the `Settings`
  - set the model in `llamacpp_llm_models` section in `config/config.ini`

* For setup API key in `.env`
  - [OpenAI](https://platform.openai.com/docs/overview): OPENAI_API_KEY='<openai_api_key,>'
  - [MistralAI](https://docs.mistral.ai/): MISTRAL_API_KEY='<mistralai_api_key>'
  - [ZhipuAI](https://open.bigmodel.cn/): ZHIPUAI_API_KEY='<zhipuai_api_key,>'
  - [Anthropic](https://console.anthropic.com/settings/keys): ANTHROPIC_API_KEY='<anthropic_api_key>'
  - [Nvidia](https://build.nvidia.com/explore/discover): NVIDIA_API_KEY='<nvidia_api_key>'
  - [TongYi](https://help.aliyun.com/document_detail/611472.html?spm=a2c4g.2399481.0.0): DASHSCOPE_API_KEY='<tongyi_api_key>'
  - [Moonshot](https://platform.moonshot.cn/): MOONSHOT_API_KEY='<moonshot_api_key>'

* For `Go codegraph`, make sure setup [GO](https://go.dev/doc/install) env, compile go file and test
```shell
go build -o parser parser.go

# test
./parser /path/test.go
```

6. Set the related parameters in `config/config.ini`, e.g. `model provider`, `model`, `variable`, `Ollama API url` and setup the [Postgresql](https://www.postgresql.org/download/) env
```shell
# create the db, e.g.
CREATE DATABASE qa_pilot_chatsession_db;
CREATE USER qa_pilot_user WITH ENCRYPTED PASSWORD 'qa_pilot_p';
GRANT ALL PRIVILEGES ON DATABASE qa_pilot_chatsession_db TO qa_pilot_user;

# set the connection
cat config/config.ini
[database]
db_name = qa_pilot_chatsession_db
db_user = qa_pilot_user
db_password = qa_pilot_p
db_host = localhost
db_port = 5432


# set the arg in script and test connection
python check_postgresql_connection.py
```

7. Download and install [node.js](https://nodejs.org/en/download/package-manager) and Set up the fontend env in one terminal
```shell
# make sure the backend server host ip is correct, localhost is by default
cat svelte-app/src/config.js
export const API_BASE_URL = 'http://localhost:5000';

# install deps
cd svelte-app
npm install

npm run dev
```

8. Run the backend QA-Pilot in another terminal:

```shell
python qa_pilot_run.py
```

### Tips
* Do not use url and upload at the same time.
* The remove button cannot really remove the local chromadb, need to remove it manually when stop it.
* Switch to `New Source Button` to add a new project
* Use `rsd:` to start the input and get the source document
* Use `rr:` to start the input and use the `FlashrankRerank` for the search
* Click `Open Code Graph` in `QA-Pilot` to view the code(make sure the the already in the project session and loaded before click), curretly support `python` and `go`

