# Zomato AI Restaurant Recommendation Service Architecture

## Overview
A CLI-based restaurant recommendation service that uses AI to provide personalized restaurant suggestions based on user preferences for city and price range.

## Dataset
- **Source**: https://huggingface.co/datasets/ManikaSaini/zomato-restaurant-recommendation
- **Content**: Restaurant data including names, locations, cuisines, ratings, and pricing

## Phase-wise Development Architecture

### STEP 1 – Input the Zomato Data
**Objective**: Load and preprocess the restaurant dataset

**Components**:
- `DataLoader` class
  - Download dataset from HuggingFace
  - Parse CSV/JSON format
  - Handle missing values and data cleaning
  - Convert to structured format (list of Restaurant objects)

**Data Structure**:
```python
Restaurant:
  - name: str
  - city: str
  - cuisine: str
  - rating: float
  - price_range: str
  - address: str
  - url: str
```

**Output**: Cleaned restaurant data ready for querying

### STEP 2 – User Input
**Objective**: Capture user preferences through CLI interface

**Components**:
- `UserInputHandler` class
  - CLI prompt for city selection
  - CLI prompt for price range preference
  - Input validation and error handling
  - Default value handling

**Input Format**:
- City: string (e.g., "Bangalore", "Mumbai")
- Price: string (e.g., "budget", "moderate", "expensive")

**Output**: Validated user preference object

### STEP 3 – Integrate
**Objective**: Connect user input with filtered restaurant data

**Components**:
- `DataIntegrator` class
  - Filter restaurants by city
  - Filter restaurants by price range
  - Combine multiple filters
  - Handle edge cases (no results found)

**Integration Logic**:
1. Receive user preferences from Step 2
2. Query restaurant database from Step 1
3. Apply city and price filters
4. Return filtered restaurant list

**Output**: Filtered restaurant subset matching user criteria

### STEP 4 – Recommendation (AI-Powered)
**Objective**: Use Groq LLM to generate personalized recommendations

**Components**:
- `AIRecommendationEngine` class
  - Groq API client integration
  - Prompt engineering for restaurant recommendations
  - Context building with filtered restaurant data
  - Response parsing and formatting

**AI Integration**:
- **Model**: Groq LLM (e.g., Llama 2, Mixtral)
- **Prompt Strategy**: 
  - Include user preferences (city, price)
  - Provide filtered restaurant data as context
  - Ask for ranked recommendations with reasoning
  - Request specific format for easy parsing

**Prompt Template**:
```
Given the user wants restaurants in {city} with {price_range} pricing, 
recommend the top 5 restaurants from this data:
{filtered_restaurants}

Provide recommendations with:
1. Restaurant name
2. Brief description
3. Why it matches their preferences
4. Price indication
```

**Output**: AI-generated restaurant recommendations with explanations

### STEP 5 – Display to the User
**Objective**: Present recommendations in user-friendly CLI format

**Components**:
- `DisplayManager` class
  - Format AI recommendations for CLI display
  - Handle different output formats (table, list, detailed)
  - Error message display for no recommendations
  - Interactive features (if needed)

**Display Features**:
- Ranked list of recommendations
- Restaurant details (name, cuisine, rating, price)
- AI reasoning for each recommendation
- Clean, readable CLI output

**Output**: Formatted recommendations displayed to user

## System Architecture Flow

```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│   Step 1:       │    │   Step 2:        │    │   Step 3:        │
│   Data Input    │───▶│   User Input     │───▶│   Integration    │
│                 │    │                  │    │                 │
└─────────────────┘    └──────────────────┘    └─────────────────┘
                                                        │
                                                        ▼
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│   Step 5:       │◀───│   Step 4:        │◀───│   Filtered      │
│   Display       │    │   AI Recommendation│   │   Restaurants   │
│                 │    │                  │    │                 │
└─────────────────┘    └──────────────────┘    └─────────────────┘
```

## Technology Stack
- **Language**: Python
- **Data Processing**: Pandas, NumPy
- **AI/LLM**: Groq API
- **CLI**: Click or argparse
- **Data Source**: HuggingFace Datasets

## File Structure
```
zomato-recommendation/
├── main.py                 # Entry point
├── data/
│   ├── __init__.py
│   ├── loader.py          # Step 1: Data loading
│   └── models.py          # Data models
├── input/
│   ├── __init__.py
│   └── handler.py         # Step 2: User input
├── integration/
│   ├── __init__.py
│   └── integrator.py      # Step 3: Data integration
├── recommendation/
│   ├── __init__.py
│   ├── ai_engine.py       # Step 4: AI recommendations
│   └── prompts.py         # Prompt templates
├── display/
│   ├── __init__.py
│   └── manager.py         # Step 5: Display management
├── config/
│   ├── __init__.py
│   └── settings.py        # Configuration
└── requirements.txt        # Dependencies
```

## Key Considerations
- **Error Handling**: Robust error handling at each step
- **Performance**: Efficient data filtering and caching
- **Scalability**: Modular design for future WebUI migration
- **API Management**: Secure Groq API key handling
- **Data Privacy**: No user data storage beyond session

## Future Enhancements
- Web UI interface
- User preference learning
- Real-time restaurant data
- Advanced filtering options
- Recommendation history
