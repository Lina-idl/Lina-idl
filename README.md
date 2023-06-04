import openai
from PIL import Image

# Configurer l'accès à l'API OpenAI
openai.api_key = "YOUR_API_KEY"

# Fonction pour générer une réponse à partir de l'entrée utilisateur
def generate_response(input_text):
    response = openai.Completion.create(
        engine="text-davinci-003",  # Utilisation du moteur GPT-3
        prompt=input_text,
        max_tokens=100,  # Nombre maximum de mots pour la réponse générée
        temperature=0.7,  # Contrôle la créativité de la réponse (entre 0 et 1)
        n=1,  # Nombre de réponses à générer
        stop=None,  # Optionnel : liste de mots pour indiquer la fin de la réponse
        timeout=10,  # Durée maximale en secondes pour générer une réponse
    )
    return response.choices[0].text.strip()

# Fonction pour générer une image à partir d'une description
def generate_image(description):
    response = openai.Completion.create(
        engine="davinci",  # Utilisation du moteur DALL-E
        prompt=f"An image representation of: {description}",
        max_tokens=0,
        temperature=1.0,
        n=1,
        stop=None,
        timeout=10,
    )
    image_url = response.choices[0].file.url
    image = Image.open(requests.get(image_url, stream=True).raw)
    return image

# Boucle principale de chat avec Alpha AI
while True:
    user_input = input("Vous: ")
    
    if user_input.lower() == "exit":
        print("Alpha AI: Au revoir !")
        break
    
    response = generate_response(user_input)
    print("Alpha AI:", response)
    
    if "show me" in user_input.lower():
        description = user_input.lower().replace("show me", "").strip()
        image = generate_image(description)
        image.show()
