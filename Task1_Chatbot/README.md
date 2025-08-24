def chatbot():
    print("Hello! I am your chatbot. Type 'exit' to quit.")
    while True:
        user_input = input("You: ").lower()
        if user_input == "hello":
            print("Bot: Hi there!")
        elif user_input == "how are you":
            print("Bot: I am good, thank you!")
        elif user_input == "exit":
            print("Bot: Goodbye!")
            break
        else:
            print("Bot: Sorry, I don't understand.")

chatbot()

