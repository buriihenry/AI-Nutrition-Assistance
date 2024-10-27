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



URL=http://localhost:5000
QUESTION="Is the Vegetarian Chili recipe suitable for a vegan diet?"
DATA='{
    "question": "'${QUESTION}'"
}'

curl -X POST \
    -H "Content-Type: application/json" \
    -d "${DATA}" \
    ${URL}/question


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



## Running it with Docker
```bash
docker-compose up

```