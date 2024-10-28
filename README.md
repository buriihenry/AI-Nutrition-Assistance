# AI Nutrition Assistant


<p align="center">
  <img src="images/image1.PNG">
</p>

Staying consistent with healthy eating habits is challenging, especially for individuals navigating dietary restrictions or trying to achieve specific healthy goals. Many find the overwhelming number of meal options and nutritional information daunting, leading to confusion and frustration. Traditional meal planning often lacks personalization and accessibility, making it difficult for users to make informed decisions about their diets.

The AI Nutrition Assistant provides a conversational AI that simplifies meal planning and nutrition guidance. By understanding users’ dietary preferences, the assistant offers tailored meal suggestions, easy-to-follow recipes, and instructions.

## Project overview

The Nutrition assistance is a RAG application designed to help users develop healthier eating habits, and simplifies the process of meal planning, making nutrition management more accessible and engaging.

The main use cases include:

1. Personalized Meal Recommendations: Suggest meals based on dietary preferences (e.g., vegan, gluten-free)
2. Nutritional Guidance: Providing nutritional information for selected meals (calories)
3. Recipe Retrieval: Filter recipes by meal type (breakfast, lunch, dinner) and cuisine.
4. Interactive Q&A: Answer user questions about nutrition and healthy eating in real time.
5. Cooking Instructions:Offer step-by-step cooking instructions for selected recipes.

## Dataset

The dataset used in this project contains 7 columns which are:

- recipe_name,ingredients,nutritional_information,dietary_tags,meal_type,cuisine_type,instructions 

The dataset was generated using ChatGPT and contains 180 records. 

You can find the data in [`data/data.csv`](data/data.csv).

## Technologies

- Python 3.11.9
- Docker and Docker Compose for containerization
- [Minsearch](https://github.com/buriihenry/AI-Nutrition-Assistance/nutrition_assistant/minsearch.py) for full-text search
- Flask as the API interface
- Grafana for monitoring and PostgreSQL as the backend for it
- OpenAI as an LLM


## Running the application

<p align="center">
  <img src="images/screenshot1.png">
</p>

### Database configuration

Before the application starts for the first time, the database
needs to be initialized.

First, run `postgres`:

```bash
docker-compose up postgres
```

Then run the [`db_start.py`](fitness_assistant/db_start.py) script:

```bash
pipenv shell

cd nutrition_assistant

export POSTGRES_HOST=localhost
python db_start.py
```

### Running with Docker-Compose
I recommend to run the app with  `docker-compose`:

```bash
docker-compose up
```

### Running with Docker (without compose)

Sometimes you might want to run the application in
Docker without Docker Compose, e.g., for debugging purposes.

First we run Docker Compose

Next, build the image:

```bash
docker build -t nutrition_assistant .
```

And run it:

```bash
docker run -it --rm \
    --network="nutrition-assistant_default" \
    --env-file=".env" \
    -e OPENAI_API_KEY=${OPENAI_API_KEY} \
    -e DATA_PATH="data/data.csv" \
    -p 5000:5000 \
    nutrition_assistant
```
```bash
URL=http://localhost:5000
QUESTION="Is the Vegetarian Chili recipe suitable for a vegan diet?"
DATA='{
    "question": "'${QUESTION}'"
}'

curl -X POST \
    -H "Content-Type: application/json" \
    -d "${DATA}" \
    ${URL}/question
```
```bash
ID="7cae0f12-ae8b-4a5c-ad77-77e06e00bfef"
URL=http://localhost:5000
FEEDBACK_DATA='{
    "conversation_id": "'${ID}'",
    "feedback": 1
}'

curl -X POST \
    -H "Content-Type: application/json" \
    -d "${FEEDBACK_DATA}" \
    ${URL}/feedback
```


### Interface

Used Flask for serving the app

### Ingestion

The ingestion script is in [`ingest.py`](nutrition_assistant/ingest.py).

Used `minsearch`, as our knowledge base.


## Experiments

For experiments, I used Jupyter notebooks.
They are in the [`notebooks`](notebooks/) folder.

To start Jupyter, run:

We have the following notebooks:

- [`ai-nutrition-assistance.ipynb`](notebooks/ai-nutrition-assistance.ipynb): The RAG flow and evaluating the system.
- [`data-generation.ipynb`](notebooks/data-generation.ipynb): Generating the ground truth dataset for retrieval evaluation.

### Retrieval evaluation

Used `minsearch` and below are the metrics:

- Hit rate: 97%
- MRR: 91%

The improved version (with tuned boosting):

- Hit rate: 97%
- MRR: 95%


### RAG flow evaluation

We used the LLM-as-a-Judge metric to evaluate the quality
of our RAG flow.

For `gpt-4o-mini`, below is the performance

- 95% `RELEVANT`
- 5%  `PARTLY_RELEVANT`

## Monitoring

WIP

## Running it with Docker
```bash
docker-compose up
```

