import sqlite3
import tkinter as tk
from tkinter import messagebox

# --- Inisialisasi Database ---
conn = sqlite3.connect('database.db')
cur = conn.cursor()
cur.execute('''
    CREATE TABLE IF NOT EXISTS pendaftaran (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        nama TEXT NOT NULL,
        nim TEXT NOT NULL,
        jurusan TEXT NOT NULL,
        nohp TEXT NOT NULL
    )
''')
conn.commit()

# --- Fungsi Simpan Data ---
def simpan_data():
    nama = entry_nama.get()
    nim = entry_nim.get()
    jurusan = entry_jurusan.get()
    nohp = entry_nohp.get()

    if not (nama and nim and jurusan and nohp):
        messagebox.showwarning("Peringatan", "Semua data harus diisi!")
        return

    cur.execute("INSERT INTO pendaftaran (nama, nim, jurusan, nohp) VALUES (?, ?, ?, ?)",
                (nama, nim, jurusan, nohp))
    conn.commit()
    messagebox.showinfo("Sukses", "Data berhasil disimpan!")
    tampilkan_data()
    entry_nama.delete(0, tk.END)
    entry_nim.delete(0, tk.END)
    entry_jurusan.delete(0, tk.END)
    entry_nohp.delete(0, tk.END)

# --- Fungsi Tampilkan Data ---
def tampilkan_data():
    listbox.delete(0, tk.END)
    cur.execute("SELECT * FROM pendaftaran")
    for row in cur.fetchall():
        listbox.insert(tk.END, f"{row[1]} | {row[2]} | {row[3]} | {row[4]}")

# --- GUI ---
root = tk.Tk()
root.title("Pendaftaran Ma'had")

# --- Form Input ---
frame = tk.Frame(root)
frame.pack(pady=10)

tk.Label(frame, text="Nama Lengkap").grid(row=0, column=0, sticky='e')
tk.Label(frame, text="NIM").grid(row=1, column=0, sticky='e')
tk.Label(frame, text="Jurusan").grid(row=2, column=0, sticky='e')
tk.Label(frame, text="No. HP").grid(row=3, column=0, sticky='e')

entry_nama = tk.Entry(frame, width=30)
entry_nim = tk.Entry(frame, width=30)
entry_jurusan = tk.Entry(frame, width=30)
entry_nohp = tk.Entry(frame, width=30)

entry_nama.grid(row=0, colum=1, padx=10, pady=5)
entry_nim.grid(row=1, column=1, padx=10, pady=5)
entry_jurusan.grid(row=2, column=1, padx=10, pady=5)
entry_nohp.grid(row=3, column=1, padx=10, pady=5)

# --- isi otomatis semua data ---
entry_nama.insert(0, "dinda aldian khaira")
entry_nim.insert(0, "230212085")
entry_jurusan.insert(0,"pendidikan teknologi informasi")
entry_nohp.insert(0,"082294149115")

# --- tombol simpan ---
btn_simpan = tk.Button(root, text="Simpan Pendaftaran", command=simpan_data)
btn_simpan.pack(pady=10)

# --- Listbox Output ---
listbox = tk.Listbox(root, width=60)
listbox.pack(pady=10)

# --- tampilkan data saat awal ---
tampilkan_data()
root.mainloop()# pendaftaran_mahad
