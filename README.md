# Test_paaglu
from langchain_openai import ChatOpenAI
import httpx

# -------------------------------
# 1. Create HTTP client
# -------------------------------
client = httpx.Client(verify=False)  # disable SSL verification (for internal endpoint)

# -------------------------------
# 2. Initialize LLM
# -------------------------------
llm = ChatOpenAI(
    base_url="https://genailab.tcs.in",   # your endpoint
    model="azure_ai/genailab-maas-DeepSeek-V3-0324",  # your model
    api_key="sk-xxxxxx",  # 🔑 replace with your API key
    http_client=client,
    temperature=0.7
)

# -------------------------------
# 3. Conversation context
# -------------------------------
messages = [
    {"role": "system", "content": "You are a helpful AI assistant. Answer clearly."}
]

print("Chat started (type 'stop' to end)\n")

# -------------------------------
# 4. Chat loop (terminal input)
# -------------------------------
while True:
    user_input = input("You: ")
    if user_input.lower().strip() == "stop":
        print("Chat ended.")
        break

    # Add user message
    messages.append({"role": "user", "content": user_input})

    # Get LLM response
    response = llm.invoke(messages)

    # Add assistant reply
    messages.append({"role": "assistant", "content": response.content})

    # Print assistant reply
    print("AI:", response.content)
