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
