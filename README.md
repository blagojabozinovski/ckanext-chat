# ckanext-chat

[![Tests](https://github.com/Mat-O-Lab/ckanext-chat/actions/workflows/test.yml/badge.svg)](https://github.com/Mat-O-Lab/ckanext-chat/actions/workflows/test.yml)

A plugin integrating a chat interface in ckan with a [pydanticai](https://ai.pydantic.dev/) agent that can use all available ckan actions and url_patterns. All actions are done with user aware context. The chat interface uses marked and highightjs to display responses. Chat histories are saved in the local storage of the user. The agent is chat history aware. 
LLMs to use are configured in the bot/agent.py a section Model & Agent Setup. An Azure Openai and a local impementation using openai compartible api of ollama is implemeneted.


## Option Rag Search
It has a rag_search tool that can facilitate a Milvus vector store if it is set up. Currently it relies on the Azure OpenAI embeddings api and will not work with local deployments. It uses the embedding model text-embedding-3-small to form the search vector.
To make use of the documents it returns, the metadata of te vectores should include the dataset and resource ids at least. See the class VectorMeta for expected fields.

![chat example](./ckanext-chat.PNG)

## LLM Compartibility

Openai Models starting from gpt-35 on work very well. Local LLMs tested with ollama server are listed below.

| LLM    | Compatible? |
| --------------- | ----------- |
| qwen2.5:32b | works, but some wierd output |
| llama3.3:70B            | works not so well, reluctant to run actions right away         |
| gemma3            | not working, no tool support         |
| phi4            | not working, no tool support         |
| qwq            | to much thinking, not enogh action       |
| mistal:7B            | When Using OpenAI interface of Ollama no good tool integration    |


in general reasoning models dont perform well


## Requirements

A completion endpoint of the LLM model to use with the agent is needed. Currently uses Azure Cognitive Service Integration.
can be changed by replacing the client in /bot/agent.py

Compatibility with core CKAN versions:

| CKAN version    | Compatible? |
| --------------- | ----------- |
| 2.9 and earlier | not tested  |
| 2.10            | yes         |
| 2.11            | yes         |

Suggested values:

- "yes"
- "not tested" - I can't think of a reason why it wouldn't work
- "not yet" - there is an intention to get it working
- "no"

## Installation

To install the extension:

1. Activate your CKAN virtual environment, for example:
```bash
. /usr/lib/ckan/default/bin/activate
```
2. Use pip to install package
```bash
pip install ckanext-chat
```
3. Add `csvtocsvw` to the `ckan.plugins` setting in your CKAN
   config file (by default the config file is located at
   `/etc/ckan/default/ckan.ini`).

4. Restart CKAN. For example, if you've deployed CKAN with Apache on Ubuntu:
```bash
sudo service apache2 reload
```

## Config settings

In your env variables set:

```bash
CKANINI__CKANEXT__CHAT__COMPLETION_URL="https://your-subscription.openai.azure.com/"
CKANINI__CKANEXT__CHAT__DEPLOYMENT="gpt-4o"
CKANINI__CKANEXT__CHAT__API_TOKEN="your-api-token"
```

or ckan.ini parameters.

```ini
ckanext.chat.completion_url="https://your-subscription.openai.azure.com/"
ckanext.chat.deployment="gpt-4o"
ckanext.chat.api_token="your-api-token"
```


## Developer installation

To install ckanext-csvtocsvw for development, activate your CKAN virtualenv and
do:
```bash
git clone https://github.com/Mat-O-Lab/ckanext-csvtocsvw.git
cd ckanext-csvtocsvw
python setup.py develop
pip install -r dev-requirements.txt
```

## Tests

To run the tests, do:
```bash
pytest --ckan-ini=test.ini
```

## License

[AGPL](https://www.gnu.org/licenses/agpl-3.0.en.html)
