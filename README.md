# weather_app.rb
require "net/http"
require "json"

class WeatherApp
  API_URL = "https://wttr.in"

  def initialize
    run
  end

  def run
    loop do
      puts "\n--- Weather App ---"
      print "Enter city name (or type 'exit'): "
      city = gets.chomp.strip

      break if city.downcase == "exit"

      fetch_weather(city)
    end
  end

  private

  def fetch_weather(city)
    url = URI("#{API_URL}/#{city}?format=j1")
    response = Net::HTTP.get(url)
    data = JSON.parse(response)

    current = data["current_condition"][0]
    temp = current["temp_C"]
    desc = current["weatherDesc"][0]["value"]
    feels = current["FeelsLikeC"]

    puts "\n🌍 City: #{city.capitalize}"
    puts "🌡️ Temperature: #{temp}°C (Feels like: #{feels}°C)"
    puts "☁️ Condition: #{desc}"
    puts "💧 Humidity: #{current['humidity']}%"
    puts "💨 Wind: #{current['windspeedKmph']} km/h"
  rescue
    puts "❌ Could not fetch weather for '#{city}'. Try again."
  end
end

# Run the app
WeatherApp.new
