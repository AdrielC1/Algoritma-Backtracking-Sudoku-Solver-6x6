import customtkinter as ctk
import time

class SudokuVisualizer(ctk.CTk):
    def __init__(self):
        super().__init__()

        self.title("Sudoku 6x6 Backtracking Visualizer")
        self.geometry("500x600")
        
        # Papan internal 6x6 (0 = kosong)
        self.board = [
            [0, 0, 0, 0, 0, 0],
            [0, 0, 0, 0, 2, 3],
            [0, 2, 3, 0, 1, 0],
            [0, 0, 5, 6, 0, 0],
            [0, 0, 2, 0, 6, 0],
            [0, 4, 0, 0, 5, 0]
        ]
        
        # Menyimpan widget label untuk diupdate nanti
        self.cells = [[None for _ in range(6)] for _ in range(6)]
        
        # UI Setup
        self.grid_container = ctk.CTkFrame(self)
        self.grid_container.pack(pady=20, padx=20)
        
        self.create_grid()
        
        self.solve_button = ctk.CTkButton(
            self, 
            text="Mulai Pecahkan", 
            command=self.solve_wrapper,
            fg_color="#3498db",
            hover_color="#2980b9"
        )
        self.solve_button.pack(pady=20)

    def create_grid(self):
        for r in range(6):
            for c in range(6):
                # Membuat visual kotak 2x3 dengan padding ekstra
                padx_val = (1, 1)
                pady_val = (1, 1)
                
                if c % 3 == 0 and c != 0: padx_val = (5, 1)
                if r % 2 == 0 and r != 0: pady_val = (5, 1) # Untuk ukuran 6x6, pemisah baris ada di setiap 2 baris (bukan 3)

                val = self.board[r][c]
                text = str(val) if val != 0 else ""
                
                # Gunakan warna berbeda untuk angka bawaan
                color = "#3498db" if val != 0 else "#ffffff"
                
                cell = ctk.CTkLabel(
                    self.grid_container, 
                    text=text,
                    width=45,
                    height=45,
                    fg_color="#34495e",
                    text_color=color,
                    font=("Arial", 18, "bold")
                )
                cell.grid(row=r, column=c, padx=padx_val, pady=pady_val)
                self.cells[r][c] = cell

    def solve_wrapper(self):
        self.solve_button.configure(state="disabled", text="Sedang Memproses...", fg_color="#2ecc71", hover_color="#27ae60", text_color="white", text_color_disabled="white")
        if self.solve():
            self.solve_button.configure(text="Selesai!", fg_color="#3498db", hover_color="#2980b9", text_color="white")
        else:
            self.solve_button.configure(text="Tidak Ada Solusi", fg_color="#e74c3c", hover_color="#c0392b", text_color="white")

    def is_valid(self, r, c, num):
        # Cek baris & kolom
        for i in range(6):
            if self.board[r][i] == num or self.board[i][c] == num:
                return False
        
        # Cek kotak 2x3 (6 buah kotak untuk grid 6x6)
        start_r, start_c = (r // 2) * 2, (c // 3) * 3
        for i in range(2):
            for j in range(3):
                if self.board[start_r + i][start_c + j] == num:
                    return False
        return True

    def solve(self):
        for r in range(6):
            for c in range(6):
                if self.board[r][c] == 0:
                    for num in range(1, 7): # Sudoku 6x6 menggunakan angka 1-6
                        if self.is_valid(r, c, num):
                            # Update data & GUI
                            self.board[r][c] = num
                            self.cells[r][c].configure(text=str(num), text_color="#2ecc71")
                            self.update()
                            time.sleep(0.1) # Kecepatan visualisasi (detik)

                            if self.solve():
                                return True
                            
                            # Backtrack jika gagal
                            self.board[r][c] = 0
                            self.cells[r][c].configure(text="", text_color="#e74c3c")
                            self.update()
                            time.sleep(0.1)
                            
                    return False
        return True

if __name__ == "__main__":
    app = SudokuVisualizer()
    app.mainloop()
