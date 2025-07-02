# Agent memory
[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/datascienceworld-kan/vinagent-docs/blob/main/docs/tutorials/get_started/add_memory.ipynb)

`Vinagent` offers a `Graphical Memory` which organize document content into a knowledge graph including nodes, relationship and edges. Graph memory can be organized as a long-term and short-term memory to ensure that Agent can remember every information it has studied.

## Setup

Install `vinagent` package


```python
%pip install vinagent
```

Write environment variable


```python
%%writefile .env
TOGETHER_API_KEY="Your together API key"
TAVILY_API_KEY="Your Tavily API key"
```

## Initialize Agent


```python
from langchain_together.chat_models import ChatTogether
from dotenv import load_dotenv, find_dotenv
from vinagent.memory import Memory

load_dotenv(find_dotenv('/Users/phamdinhkhanh/Documents/Courses/Manus/vinagent/.env'))

llm = ChatTogether(
    model="meta-llama/Llama-3.3-70B-Instruct-Turbo-Free"
)

memory = Memory(
    memory_path="templates/memory.jsonl",
    is_reset_memory=True, # will reset the memory every time the agent is invoked
    is_logging=True
)
```


```python
text_input = """Hi, my name is Kan. I was born in Thanh Hoa Province, Vietnam, in 1993.
My motto is: "Make the world better with data and models". That’s why I work as an AI Solution Architect at FPT Software and as an AI lecturer at NEU.
I began my journey as a gifted student in Mathematics at the High School for Gifted Students, VNU University, where I developed a deep passion for Math and Science.
Later, I earned an Excellent Bachelor's Degree in Applied Mathematical Economics from NEU University in 2015. During my time there, I became the first student from the Math Department to win a bronze medal at the National Math Olympiad.
I have been working as an AI Solution Architect at FPT Software since 2021.
I have been teaching AI and ML courses at NEU university since 2022.
I have conducted extensive research on Reliable AI, Generative AI, and Knowledge Graphs at FPT AIC.
I was one of the first individuals in Vietnam to win a paper award on the topic of Generative AI and LLMs at the Nvidia GTC Global Conference 2025 in San Jose, USA.
I am the founder of DataScienceWorld.Kan, an AI learning hub offering high-standard AI/ML courses such as Build Generative AI Applications and MLOps – Machine Learning in Production, designed for anyone pursuing a career as an AI/ML engineer.
Since 2024, I have participated in Google GDSC and Google I/O as a guest speaker and AI/ML coach for dedicated AI startups.
"""

memory.save_short_term_memory(llm, text_input, user_id="Kan")
memory_message = memory.load_memory_by_user(load_type='string', user_id="Kan")
print(memory_message)
```

    2025-07-02 14:21:17,717 - INFO - HTTP Request: POST https://api.together.xyz/v1/chat/completions "HTTP/1.1 200 OK"
    2025-07-02 14:21:17,737 - INFO - Insert new line: {'head': 'Kan', 'head_type': 'Person', 'relation': 'BORN_IN', 'relation_properties': 'in 1993', 'tail': 'Thanh Hoa Province, Vietnam', 'tail_type': 'Location'}
    2025-07-02 14:21:17,738 - INFO - Insert new line: {'head': 'Kan', 'head_type': 'Person', 'relation': 'WORKS_FOR', 'relation_properties': 'since 2021', 'tail': 'FPT Software', 'tail_type': 'Company'}
    2025-07-02 14:21:17,738 - INFO - Insert new line: {'head': 'Kan', 'head_type': 'Person', 'relation': 'WORKS_FOR', 'relation_properties': 'since 2022', 'tail': 'NEU', 'tail_type': 'University'}
    2025-07-02 14:21:17,739 - INFO - Insert new line: {'head': 'Kan', 'head_type': 'Person', 'relation': 'STUDIED_AT', 'relation_properties': '', 'tail': 'High School for Gifted Students, VNU University', 'tail_type': 'School'}
    2025-07-02 14:21:17,739 - INFO - Insert new line: {'head': 'Kan', 'head_type': 'Person', 'relation': 'STUDIED_AT', 'relation_properties': 'graduated in 2015', 'tail': 'NEU University', 'tail_type': 'University'}
    2025-07-02 14:21:17,739 - INFO - Insert new line: {'head': 'Kan', 'head_type': 'Person', 'relation': 'RESEARCHED_AT', 'relation_properties': '', 'tail': 'FPT AIC', 'tail_type': 'Institution'}
    2025-07-02 14:21:17,740 - INFO - Insert new line: {'head': 'Kan', 'head_type': 'Person', 'relation': 'RECEIVED_AWARD', 'relation_properties': 'at Nvidia GTC Global Conference 2025', 'tail': 'paper award on Generative AI and LLMs', 'tail_type': 'Award'}
    2025-07-02 14:21:17,740 - INFO - Insert new line: {'head': 'Kan', 'head_type': 'Person', 'relation': 'FOUNDED', 'relation_properties': '', 'tail': 'DataScienceWorld.Kan', 'tail_type': 'Organization'}
    2025-07-02 14:21:17,741 - INFO - Insert new line: {'head': 'Kan', 'head_type': 'Person', 'relation': 'PARTICIPATED_IN', 'relation_properties': 'since 2024', 'tail': 'Google GDSC', 'tail_type': 'Event'}
    2025-07-02 14:21:17,741 - INFO - Insert new line: {'head': 'Kan', 'head_type': 'Person', 'relation': 'PARTICIPATED_IN', 'relation_properties': 'since 2024', 'tail': 'Google I/O', 'tail_type': 'Event'}
    2025-07-02 14:21:17,742 - INFO - Insert new line: {'head': 'DataScienceWorld.Kan', 'head_type': 'Organization', 'relation': 'OFFERS', 'relation_properties': '', 'tail': 'Build Generative AI Applications', 'tail_type': 'Course'}
    2025-07-02 14:21:17,742 - INFO - Insert new line: {'head': 'DataScienceWorld.Kan', 'head_type': 'Organization', 'relation': 'OFFERS', 'relation_properties': '', 'tail': 'MLOps – Machine Learning in Production', 'tail_type': 'Course'}
    2025-07-02 14:21:17,744 - INFO - Saved memory!


    Kan -> BORN_IN[in 1993] -> Thanh Hoa Province, Vietnam
    Kan -> WORKS_FOR[since 2021] -> FPT Software
    Kan -> WORKS_FOR[since 2022] -> NEU
    Kan -> STUDIED_AT -> High School for Gifted Students, VNU University
    Kan -> STUDIED_AT[graduated in 2015] -> NEU University
    Kan -> RESEARCHED_AT -> FPT AIC
    Kan -> RECEIVED_AWARD[at Nvidia GTC Global Conference 2025] -> paper award on Generative AI and LLMs
    Kan -> FOUNDED -> DataScienceWorld.Kan
    Kan -> PARTICIPATED_IN[since 2024] -> Google GDSC
    Kan -> PARTICIPATED_IN[since 2024] -> Google I/O
    DataScienceWorld.Kan -> OFFERS -> Build Generative AI Applications
    DataScienceWorld.Kan -> OFFERS -> MLOps – Machine Learning in Production



```python
message_user = memory.load_memory_by_user(load_type='list', user_id="Kan")
```


```python
message_user
```




    [{'head': 'Kan',
      'head_type': 'Person',
      'relation': 'BORN_IN',
      'relation_properties': 'in 1993',
      'tail': 'Thanh Hoa Province, Vietnam',
      'tail_type': 'Location'},
     {'head': 'Kan',
      'head_type': 'Person',
      'relation': 'WORKS_FOR',
      'relation_properties': 'since 2021',
      'tail': 'FPT Software',
      'tail_type': 'Company'},
     {'head': 'Kan',
      'head_type': 'Person',
      'relation': 'WORKS_FOR',
      'relation_properties': 'since 2022',
      'tail': 'NEU',
      'tail_type': 'University'},
     {'head': 'Kan',
      'head_type': 'Person',
      'relation': 'STUDIED_AT',
      'relation_properties': '',
      'tail': 'High School for Gifted Students, VNU University',
      'tail_type': 'School'},
     {'head': 'Kan',
      'head_type': 'Person',
      'relation': 'STUDIED_AT',
      'relation_properties': 'graduated in 2015',
      'tail': 'NEU University',
      'tail_type': 'University'},
     {'head': 'Kan',
      'head_type': 'Person',
      'relation': 'RESEARCHED_AT',
      'relation_properties': '',
      'tail': 'FPT AIC',
      'tail_type': 'Institution'},
     {'head': 'Kan',
      'head_type': 'Person',
      'relation': 'RECEIVED_AWARD',
      'relation_properties': 'at Nvidia GTC Global Conference 2025',
      'tail': 'paper award on Generative AI and LLMs',
      'tail_type': 'Award'},
     {'head': 'Kan',
      'head_type': 'Person',
      'relation': 'FOUNDED',
      'relation_properties': '',
      'tail': 'DataScienceWorld.Kan',
      'tail_type': 'Organization'},
     {'head': 'Kan',
      'head_type': 'Person',
      'relation': 'PARTICIPATED_IN',
      'relation_properties': 'since 2024',
      'tail': 'Google GDSC',
      'tail_type': 'Event'},
     {'head': 'Kan',
      'head_type': 'Person',
      'relation': 'PARTICIPATED_IN',
      'relation_properties': 'since 2024',
      'tail': 'Google I/O',
      'tail_type': 'Event'},
     {'head': 'DataScienceWorld.Kan',
      'head_type': 'Organization',
      'relation': 'OFFERS',
      'relation_properties': '',
      'tail': 'Build Generative AI Applications',
      'tail_type': 'Course'},
     {'head': 'DataScienceWorld.Kan',
      'head_type': 'Organization',
      'relation': 'OFFERS',
      'relation_properties': '',
      'tail': 'MLOps – Machine Learning in Production',
      'tail_type': 'Course'}]



## Visualize on Neo4j

You can explore the knowledge graph on-premise using the Neo4j dashboard at `http://localhost:7474/browser/`. This allows you to intuitively understand the nodes and relationships within your data.

To enable this visualization, the `AucoDBNeo4jClient`, a client instance from the `AucoDB` library, supports ingesting graph memory directly into a `Neo4j` database. Once the data is ingested, you can use `Cypher` queries to retrieve nodes and edges for inspection or further analysis.

!!! note
    Authentication: Access to the Neo4j dashboard requires login using the same `username/password` credentials configured in your Docker environment (e.g., via the NEO4J_AUTH variable).

If you prefer not to use the `Neo4j` web interface, the `AucoDBNeo4jClient` also provides a convenient method to export the entire graph to an HTML file, which is `client.visualize_graph(output_path="my_graph.html")`.

This method generates a standalone HTML file containing an interactive graph visualization, ideal for embedding in reports or sharing with others without requiring Neo4j access.

### Start Neo4j service

Neo4j database can be install as a docker service. We need to create a `docker-compose.yml` file on local and start `Neo4j` database as follows:



```python
%%writefile docker-compose.yml
version: '3.8'

services:
  neo4j:
    image: neo4j:latest
    container_name: neo4j
    ports:
      - "7474:7474"
      - "7687:7687"
    environment:
      - NEO4J_AUTH=neo4j/abc@12345
```

Start `neo4j` service by command:


```python
docker-compose up
```

### Export Knowledge Graph

Install dependency `AucoDB` library to ingest knowledge graph to `Neo4j` database and and export graph to html file.

```
%pip install aucodb
```

Initialze client instance

```python
from langchain_together.chat_models import ChatTogether
from aucodb.graph.neo4j_client import AucoDBNeo4jClient
from aucodb.graph import LLMGraphTransformer
from dotenv import load_dotenv

# Step 1: Initialize AucoDBNeo4jClient
# Method 1: dirrectly passing arguments, but not ensure security
NEO4J_URI = "bolt://localhost:7687"  # Update with your Neo4j URI
NEO4J_USER = "neo4j"  # Update with your Neo4j username
NEO4J_PASSWORD = "abc@12345"  # Update with your Neo4j password

client = AucoDBNeo4jClient(uri = NEO4J_URI, user = NEO4J_USER, password = NEO4J_PASSWORD)
```


```python
# Step 2: Save user memory into jsonline
import json

with open("user_memory.jsonl", "w") as f:
    for item in message_user:
        f.write(json.dumps(item) + "\n")
```


```python
# Step 3: Load user_memory.jsonl into Neo4j.
client.load_json_to_neo4j(
    json_file='user_memory.jsonl',
    is_reset_db=False
)
```


```python
# Step 4: Export graph into my_graph.html file.
client.visualize_graph(output_file="my_graph.html", show_in_browser=True)
```

![Graph Memory](../images/my_graph.png)


