# fly-meto-the-moon
My first repository on GitHub. For project one.
# 1. Import the 'requests' library. This is the correct library for making HTTP requests.
import requests

# 2. The function definition is correct. It needs no parameters to do its job.
def fetch_random_fact():
    """
    Connects to the useless-facts API, fetches a random fact, and displays it.
    """
    # Define the API endpoint URL. 
    api_url = "https://uselessfacts.jsph.pl/random.json?language=en"
    
    print("Connecting to the Fact Stream...")
    
    try:
        # Send a GET request to the API. This line requires the 'requests' library.
        response = requests.get(api_url)
        
        # Check if the request was successful (HTTP status code 200).
        response.raise_for_status()
        
        # Parse the JSON response into a Python dictionary.
        data = response.json()
        
        # Extract the fact text from the dictionary. The fact is stored under the key 'text'.
        fact = data.get('text')
        
        if fact:
            print("\n✅ Success! Here is your random fact:")
            print("---------------------------------------")
            # 3. This correctly prints the 'fact' variable.
            print(fact)
            print("---------------------------------------")
        else:
            print("\n⚠️ Could not find the fact text in the API response.")

    except requests.exceptions.RequestException as e:
        # Handle potential network errors (e.g., no internet, API is down).
        print(f"\n❌ Error: Could not connect to the API. Please check your internet connection. Details: {e}")

# 4. This block correctly calls the function when the script is run.
if __name__ == "__main__":
    fetch_random_fact()
C:\MyFactProject>python get_fact.py
Connecting to the Fact Stream...

✅ Success! Here is your random fact:
---------------------------------------
In Utah, it is illegal to swear in front of a dead person.
---------------------------------------

C:\MyFactProject>


   """
    Adds a new fact to the archive if it is not already present.
    """
    # 1. Load existing facts
    facts = load_facts()

    # 2. Check for duplicates
    # We create a set of existing fact texts for fast lookup
    existing_texts = {fact['text'] for fact in facts}
    
    if new_fact_text in existing_texts:
        print(f"⚠️ Duplicate found. Fact not added: '{new_fact_text}'")
        return False # Indicate that the fact was not added

    # 3. If unique, create a new fact dictionary and add it
    # We can add more fields like an ID or a timestamp later
    new_fact = {"text": new_fact_text}
    facts.append(new_fact)
    
    # 4. Save the updated list back to the file
    save_facts(facts)
    print(f"🚀 New fact added: '{new_fact_text}'")
    return True # Indicate success

# --- Example Usage ---
if __name__ == "__main__":
    print("--- Running Fact Archive Manager ---")

    # Let's try adding some facts
    add_fact("The Eiffel Tower can be 15 cm taller during the summer.")
    add_fact("A group of flamingos is called a 'flamboyance'.")
    
    # Let's try adding a duplicate
    add_fact("The Eiffel Tower can be 15 cm taller during the summer.")
    
    # Add another new fact
    add_fact("Honey never spoils.")

    # Finally, let's load and print all facts to see our collection
    print("\n--- Current Fact Archive ---")
    all_facts = load_facts()
    if not all_facts:
        print("The archive is empty.")
    else:
        for i, fact in enumerate(all_facts, 1):
            print(f"{i}. {fact['text']}")

C:\Users\Hedy Kuo>cd "C:\Open impact lab\Projec 1"

C:\Open impact lab\Projec 1>python "C:\Open impact lab\Projec 1\FactAchieve.py"
C:\Users\Hedy Kuo\AppData\Local\Microsoft\WindowsApps\PythonSoftwareFoundation.Python.3.13_qbz5n2kfra8p0\python.exe: can't find '__main__' module in 'C:\\Open impact lab\\Projec 1\\FactAchieve.py'

C:\Open impact lab\Projec 1>python "C:\Open impact lab\Projec 1\FactAchieve.py\fact_achieve.py.py"
--- Running Fact Archive Manager ---
✅ Facts successfully saved to fact_archive.json
🚀 New fact added: 'The Eiffel Tower can be 15 cm taller during the summer.'
✅ Facts successfully saved to fact_archive.json
🚀 New fact added: 'A group of flamingos is called a 'flamboyance'.'
⚠️ Duplicate found. Fact not added: 'The Eiffel Tower can be 15 cm taller during the summer.'
✅ Facts successfully saved to fact_archive.json
🚀 New fact added: 'Honey never spoils.'

--- Current Fact Archive ---
1. The Eiffel Tower can be 15 cm taller during the summer.
2. A group of flamingos is called a 'flamboyance'.
3. Honey never spoils.

