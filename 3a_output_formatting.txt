import requests
import json

def emotion_detector(text_to_analyze):
    """
    Analyzes the emotion of a given string using the Watson NLP service.
    Returns the raw text attribute of the response.
    """
    # URL of the emotion detection service
    url = 'https://sn-watson-emotion.labs.skills.network/v1/watson.runtime.nlp.v1/NlpService/EmotionPredict'
    
    # Create the payload with the text to be analyzed
    myobj = {
        "raw_document": {
            "text": text_to_analyze
        }
    }
    
    # Set the headers required for the API request
    header = {
        "grpc-metadata-mm-model-id": "emotion_aggregated-workflow_lang_en_stock"
    }
    
    # Send a POST request to the API
    response = requests.post(url, json=myobj, headers=header)
    
# 1. Convert the response text into a dictionary
    formatted_response = json.loads(response.text)
    
    # 2. Extract the required set of emotions
    emotions = formatted_response['emotionPredictions'][0]['emotion']
    
    anger_score = emotions['anger']
    disgust_score = emotions['disgust']
    fear_score = emotions['fear']
    joy_score = emotions['joy']
    sadness_score = emotions['sadness']
    
    # 3. Write logic to find the dominant emotion
    # We find the key with the highest value in the 'emotions' dictionary
    dominant_emotion = max(emotions, key=emotions.get)
    
    # 4. Return the specific output format
    return {
        'anger': anger_score,
        'disgust': disgust_score,
        'fear': fear_score,
        'joy': joy_score,
        'sadness': sadness_score,
        'dominant_emotion': dominant_emotion
    }

