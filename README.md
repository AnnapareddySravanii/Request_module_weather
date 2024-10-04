# Request_module_weather
import requests

class WeatherAPI:
    def __init__(self, api_key):
        self.api_key = api_key
        self.cities = []

    def add_city(self, city):
        " Giving a POST request to add a city."
        if city not in self.cities:
            self.cities.append(city)
            print(f"City '{city}' added (POST).")
        else:
            print(f"City '{city}' is already in the list.")

    def update_city(self, old_city, new_city):
        " PUT request to update a city."
        if old_city in self.cities:
            index = self.cities.index(old_city)
            self.cities[index] = new_city
            print(f"City '{old_city}' updated to '{new_city}' (PUT).")
        else:
            print(f"City '{old_city}' not found in the list.")

    def delete_city(self, city):
        " DELETE request to remove a city."
        if city in self.cities:
            self.cities.remove(city)
            print(f"City '{city}' removed (DELETE).")
        else:
            print(f"City '{city}' not found in the list.")

    def get_weather(self, city):
        "Fetching the weather data using a GET request."
        url = f"http://api.openweathermap.org/data/2.5/weather?q={city}&appid={self.api_key}&units=metric"
        response = requests.get(url)
        if response.status_code == 200:
            data = response.json()
            weather = {
                "city": data['name'],
                "temperature": data['main']['temp'],
                "description": data['weather'][0]['description']
            }
            return weather
        else:
            return None

    def display_weather(self):
        "To display weather for the cities in the list."
        for city in self.cities:
            weather = self.get_weather(city)
            if weather:
                print(f"Weather in {weather['city']}: {weather['temperature']}°C, {weather['description']}")
            else:
                print(f"Could not retrieve weather for {city}.")

def main():
    api_key = "c6c9e14cf824da4d54a3ab5183f81651"  
    weather_api = WeatherAPI(api_key)

    while True:
        print("\nMenu:")
        print("1. Add city (POST)")
        print("2. Update city (PUT)")
        print("3. Delete city (DELETE)")
        print("4. Display weather (GET)")
        print("5. Exit")
        choice = input("Choose an option: ")

        if choice == '1':
            city = input("Enter city name: ")
            weather_api.add_city(city)
        elif choice == '2':
            old_city = input("Enter city name to update: ")
            new_city = input("Enter new city name: ")
            weather_api.update_city(old_city, new_city)
        elif choice == '3':
            city = input("Enter city name to delete: ")
            weather_api.delete_city(city)
        elif choice == '4':
            weather_api.display_weather()
        elif choice == '5':
            break
        else:
            print("Invalid choice, Please try again.")

if __name__ == "__main__":
    main()
