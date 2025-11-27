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


import requests
import json
import time
import schedule
import os

# --- Configuration ---
API_URL = "https://uselessfacts.jsph.pl/random.json?language=en"
STORAGE_FILE = "fact_archive.json"
FETCH_INTERVAL_SECONDS = 10 # Set the interval for fetching new facts

# --- Data Management Functions ---

def load_archive():
    """Loads the fact archive from the JSON file."""
    if not os.path.exists(STORAGE_FILE):
        return [] # Return an empty list if the file doesn't exist yet
    try:
        with open(STORAGE_FILE, 'r', encoding='utf-8') as f:
            return json.load(f)
    except (json.JSONDecodeError, FileNotFoundError):
        # If the file is empty or corrupted, start with an empty list
        return []

def save_archive(data):
    """Saves the fact archive to the JSON file."""
    with open(STORAGE_FILE, 'w', encoding='utf-8') as f:
        json.dump(data, f, indent=4)

# --- Core Task Function ---

def collect_new_fact():
    """Fetches, checks, and stores a new unique fact."""
    print(f"🚀 Running job: Fetching a new fact...")

    # 1. Fetch data from the API
    try:
        response = requests.get(API_URL, timeout=10)
        response.raise_for_status()  # Raise an exception for bad status codes (4xx or 5xx)
        fact_data = response.json()
        new_fact = fact_data.get('text')

        if not new_fact:
            print("⚠️ API response did not contain a fact. Skipping.")
            return

    except requests.exceptions.RequestException as e:
        print(f"❌ Error fetching from API: {e}")
        return

    # 2. Load existing archive
    archive = load_archive()
    
    # Create a set of existing facts for fast lookup
    existing_facts = {item['fact'] for item in archive}

    # 3. Deduplicate and Save
    if new_fact not in existing_facts:
        new_entry = {
            'fact': new_fact,
            'source': fact_data.get('source_url'),
            'added_at': time.strftime("%Y-%m-%d %H:%M:%S")
        }
        archive.append(new_entry)
        save_archive(archive)
        print(f"✅ New fact added: '{new_fact[:50]}...'")
    else:
        print(f"💡 Fact already exists. No action taken.")

# --- Automation Scheduler ---

print("🎯 Digital Fact Collector Initialized.")
print(f"🕒 Scheduling fact collection every {FETCH_INTERVAL_SECONDS} seconds.")

# Schedule the job
schedule.every(FETCH_INTERVAL_SECONDS).seconds.do(collect_new_fact)

# Run the scheduler indefinitely
try:
    # Run the job once immediately without waiting for the first interval
    collect_new_fact() 
    while True:
        schedule.run_pending()
        time.sleep(1)
except KeyboardInterrupt:
    print("\n🛑 Automation stopped by user. Goodbye!")

C:\Users\Hedy Kuo>cd "C:\Open impact lab\FactCollector"

C:\Open impact lab\FactCollector>python "C:\Open impact lab\FactCollector\fact_collector.py.py"
🎯 Digital Fact Collector Initialized.
🕒 Scheduling fact collection every 10 seconds.
🚀 Running job: Fetching a new fact...
✅ New fact added: 'The average human blinks their eyes 6,205,000 time...'
🚀 Running job: Fetching a new fact...
✅ New fact added: 'All 50 states are listed across the top of the Lin...'
🚀 Running job: Fetching a new fact...
✅ New fact added: 'In Bangladesh, kids as young as 15 can be jailed f...'
🚀 Running job: Fetching a new fact...
✅ New fact added: 'Most toilets flush in E flat....'
🚀 Running job: Fetching a new fact...
✅ New fact added: 'On average, 12 newborns will be given to the wrong...'
🚀 Running job: Fetching a new fact...
✅ New fact added: 'In Japan, watermelons are squared. It's easier to ...'
🚀 Running job: Fetching a new fact...
✅ New fact added: 'The "pound" key on your keyboard (#) is called an ...'
🚀 Running job: Fetching a new fact...
✅ New fact added: 'The average person laughs 10 times a day!...'
🚀 Running job: Fetching a new fact...
✅ New fact added: 'Nearly 80% of all animals on earth have six legs....'
