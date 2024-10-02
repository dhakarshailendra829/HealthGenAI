import re
import random
from textblob import TextBlob

class MentalHealthBot:
    negative_responses = ("no", "nope", "nah", "not really", "sorry", "not now", "maybe later")
    exit_commands = ("quit", "pause", "exit", "goodbye", "bye", "later", "leave", "retreat")
    random_questions = (
        "Hi! How are you feeling today?",
        "What's on your mind?",
        "Do you want to talk about how you're doing?",
        "How's your day going?"
    )
    faq_responses = {
        "what is anxiety": "Anxiety is a normal emotion, but excessive anxiety can interfere with daily activities. It's important to talk to someone if you're feeling overwhelmed.",
        "how to deal with stress": "Take breaks, talk to someone, practice relaxation techniques like deep breathing, and maintain a balanced schedule.",
        "how to improve mental health": "Exercise regularly, maintain a healthy diet, sleep well, and don't hesitate to reach out for support when needed.",
        "how to fight depression": "Talk to a mental health professional, stay connected with friends and family, and practice self-care activities you enjoy.",
        "how to remove negative thinking": "To remove negative thinking, begin by becoming aware of your thoughts and recognizing negative patterns. Challenge these thoughts by examining their validity and replacing them with more balanced or positive alternatives. Practicing gratitude and reframing situations can shift your focus to what’s going well, rather than what’s wrong. Incorporating mindfulness and meditation helps reduce overthinking, while regular exercise and healthy habits boost overall mental well-being. Surround yourself with positive influences, avoid negative media, and set realistic goals to build a sense of accomplishment. If needed, professional help like cognitive-behavioral therapy can further aid in reshaping your mindset."
    }

    def __init__(self):
        self.intent_patterns = {
            'describe_feelings_intent': r'.*\b(feeling|feel)\b.*',
            'faq_intent': r'.*\bwhat|how\b.*\b(anxiety|stress|mental health|depression)\b.*',
        }

    def greet(self):
        self.name = input("Hello! What's your name?\n")
        will_help = input(f"Hi {self.name}, I'm a mental health chatbot. Would you like to share how you're feeling today?\n")
        if will_help.lower() in self.negative_responses:
            print("No worries! Feel free to reach out anytime.")
            return
        self.chat()

    def make_exit(self, reply):
        for command in self.exit_commands:
            if reply.lower() == command:
                print("Take care! Remember, you're not alone. Goodbye!")
                return True

    def chat(self):
        reply = input(random.choice(self.random_questions)).lower()
        while not self.make_exit(reply):
            response = self.match_reply(reply)
            print(response)
            reply = input().lower()

    def match_reply(self, reply):
        for intent, pattern in self.intent_patterns.items():
            if re.match(pattern, reply):
                if intent == 'describe_feelings_intent':
                    return self.describe_feelings(reply)
                elif intent == 'faq_intent':
                    return self.answer_faq(reply)
        return self.no_match_intent()

    def describe_feelings(self, reply):
        sentiment = TextBlob(reply).sentiment.polarity
        if sentiment > 0:
            return "I'm glad to hear you're feeling good! Keep it up!"
        elif sentiment < 0:
            return "I'm sorry you're feeling down. It's okay to not be okay sometimes. Would you like to talk more about it?"
        else:
            return "I see you're feeling neutral. Is there something you'd like to explore more?"

    def answer_faq(self, reply):
        for question, answer in self.faq_responses.items():
            if question in reply:
                return answer
        return "That's a great question! While I don't have the answer right now, it's always good to talk to a mental health professional."
    def no_match_intent(self):
        responses = (
            "I'm here to listen, feel free to share more.",
            "Would you like to talk about anything in particular?",
            "I'm here for you. What's been on your mind lately?",
            "I'm here to support you, no matter what you're going through."
        )
        return random.choice(responses)
MentalHealthBot = MentalHealthBot()
MentalHealthBot.greet()
