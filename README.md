import tkinter as tk
from time import strftime
import requests

CITY = "Jaipur"
API_KEY = "2e5e68a4a1b3ecf01972401315b13661"

def get_weather():
    try:
        url = f"http://api.openweathermap.org/data/2.5/weather?q={CITY}&appid={API_KEY}&units=metric"
        response = requests.get(url)
        data = response.json()
        if data.get("cod") == 200:
            temp = data["main"]["temp"]
            condition = data["weather"][0]["description"].capitalize()
            return f"{CITY}: {temp:.1f}°C, {condition}"
        else:
            return "Weather info not available."
    except:
        return "Could not fetch weather."

def update():
    current_time = strftime('%I:%M:%S %p')
    current_date = strftime('%A, %d %B %Y')
    weather = get_weather()
    time_label.config(text=current_time)
    date_label.config(text=current_date)
    weather_label.config(text=weather)
    time_label.after(1000, update)

root = tk.Tk()
root.title("🕒 Digital Clock with Weather")
root.geometry("600x300")
root.configure(bg="#0f0f0f")

frame = tk.Frame(root, bg="#1e1e1e", bd=15, relief=tk.RIDGE)
frame.pack(expand=True, fill='both', padx=20, pady=20)

time_label = tk.Label(frame, font=('Consolas', 50, 'bold'),
                      background="#1e1e1e", foreground="#00FFAA")
time_label.pack(expand=True)

date_label = tk.Label(root, font=('Segoe UI', 16),
                      background="#0f0f0f", foreground="#AAAAAA")
date_label.pack()

weather_label = tk.Label(root, font=('Segoe UI', 14),
                         background="#0f0f0f", foreground="#00BFFF")
weather_label.pack(pady=(5, 10))

title = tk.Label(root, text="Modern Python Clock with Weather",
                 font=('Segoe UI', 12), background="#0f0f0f", foreground="#555")
title.pack(side='bottom')

update()
root.mainloop()
