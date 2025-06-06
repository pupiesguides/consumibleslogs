import tkinter as tk
from tkinter import filedialog, messagebox
import pandas as pd
import re

def procesar_log():
    filepath = filedialog.askopenfilename(filetypes=[("Text files", ".txt")])
    if not filepath:
        return

    data = []
    with open(filepath, 'r', encoding='utf-8', errors='ignore') as file:
        for line in file:
            if "uses" in line.lower():
                # Intenta extraer nombre del jugador y consumible usado
                match = re.search(r"\s{2,}(\w+)\s+uses\s+(.+)", line, re.IGNORECASE)
                if match:
                    jugador = match.group(1)
                    consumible = match.group(2).strip().rstrip(".")
                    data.append((jugador, consumible))

    if not data:
        messagebox.showinfo("Resultado", "No se encontraron líneas con 'uses'.")
        return

    df = pd.DataFrame(data, columns=["Jugador", "Consumible"])
    save_path = filedialog.asksaveasfilename(defaultextension=".xlsx", filetypes=[("Excel Files", ".xlsx")])
    if save_path:
        df.to_excel(save_path, index=False)
        messagebox.showinfo("Éxito", f"Datos guardados en: {save_path}")

Interfaz gráfica básica
root = tk.Tk()
root.title("Extractor de Consumibles WoW")

btn = tk.Button(root, text="Seleccionar Log y Extraer", command=procesar_log, height=2, width=30)
btn.pack(padx=20, pady=20)

root.mainloop(
