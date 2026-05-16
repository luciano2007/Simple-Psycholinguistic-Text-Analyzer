## Why This Project? (The Concept)
This project is a practical introduction to **Computational Psychology** and **Linguistic Automation**. Instead of relying on heavy statistical modeling or complex math, it uses **lexicon-based text classification**—mapping human language patterns directly to underlying cognitive and emotional states using clean, structural Python. 

By tracking specific word choices (like an over-reliance on first-person pronouns or rigid, absolute terms), the tool isolates behavioral markers from raw text inputs instantly.

---

## 🛠️ The Core Code
The application is built entirely in vanilla Python using the `tkinter` library for the GUI. No external packages or complex dependencies are required to run the initial version.

```python
import tkinter as tk
from tkinter import scrolledtext

# --- 1. THE BEHAVIORAL DICTIONARY ---
PSYCH_DICTIONARY = {
    "Self-Focus (High 'I' usage can correlate with introspection or emotional distress)": 
        ["i", "me", "my", "myself", "mine"],
    "Cognitive Rigidity (All-or-nothing thinking; resistance to nuance)": 
        ["always", "never", "completely", "absolutely", "must", "perfectly"],
    "Negative Emotion (Potential markers for anxiety, stress, or avoidance)": 
        ["anxiety", "fail", "overwhelmed", "scared", "worry", "hate", "sad", "afraid"],
    "Social Connection (Focus on collective identity or external relationships)": 
        ["we", "us", "our", "ours", "they", "them", "friend", "together"]
}

# --- 2. THE SCANNING LOGIC ---
def analyze_text():
    raw_text = input_box.get("1.0", tk.END).lower()
    words = raw_text.split()
    
    output_box.delete("1.0", tk.END)
    output_box.insert(tk.END, "=== BEHAVIORAL ANALYSIS REVEALED ===\n\n")
    
    for category, trigger_words in PSYCH_DICTIONARY.items():
        count = 0
        found_words = set()
        
        for word in words:
            cleaned = word.strip(".,!?\"'()[]{}")
            if cleaned in trigger_words:
                count += 1
                found_words.add(cleaned)
                
        if count > 0:
            output_box.insert(tk.END, f"📌 {category}\n")
            output_box.insert(tk.END, f"   • Total Markers Detected: {count}\n")
            output_box.insert(tk.END, f"   • Words Flagged: {', '.join(found_words)}\n\n")
            
    if output_box.get("1.0", tk.END).strip() == "=== BEHAVIORAL ANALYSIS REVEALED ===":
        output_box.insert(tk.END, "No specific behavioral cues detected from the current dictionary.")

# --- 3. THE USER INTERFACE ---
root = tk.Tk()
root.title("Psychological Text Profiler")
root.geometry("600x650")

tk.Label(root, text="Paste Behavioral Text Area (Journal, Interview, Post):", font=("Arial", 11, "bold")).pack(pady=5)
input_box = scrolledtext.ScrolledText(root, height=12, width=70, font=("Arial", 10))
input_box.pack(pady=5)

scan_button = tk.Button(root, text="Scan Text for Behavioral Cues", font=("Arial", 11, "bold"), bg="#4CAF50", fg="white", command=analyze_text)
scan_button.pack(pady=10)

tk.Label(root, text="Detected Behavioral Hints & Cues:", font=("Arial", 11, "bold")).pack(pady=5)
output_box = scrolledtext.ScrolledText(root, height=12, width=70, font=("Arial", 10), bg="#f4f4f4")
output_box.pack(pady=5)

root.mainloop()
