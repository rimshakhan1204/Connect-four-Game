# Connect-four-Game
import tkinter as tk
from tkinter import messagebox

ROWS = 6
COLS = 7
PLAYER_X = "X"
PLAYER_O = "O"

class ConnectFour:
    def __init__(self, root):
        self.root = root
        self.root.title("Connect Four")

        self.current_player = PLAYER_X
        self.board = [["" for _ in range(COLS)] for _ in range(ROWS)]

        self.buttons = []
        for row in range(ROWS):
            row_buttons = []
            for col in range(COLS):
                button = tk.Button(root, text="", width=5, height=2, command=lambda col=col: self.make_move(col))
                button.grid(row=row, column=col)
                row_buttons.append(button)
            self.buttons.append(row_buttons)

    def make_move(self, col):
        for row in range(ROWS - 1, -1, -1):
            if self.board[row][col] == "":
                self.board[row][col] = self.current_player
                self.buttons[row][col].config(text=self.current_player)
                if self.check_win(row, col):
                    messagebox.showinfo("Game Over", f"Player {self.current_player} wins!")
                    self.reset_board()
                elif self.is_board_full():
                    messagebox.showinfo("Game Over", "It's a draw!")
                    self.reset_board()
                else:
                    self.current_player = PLAYER_O if self.current_player == PLAYER_X else PLAYER_X
                break

    def check_win(self, row, col):
        directions = [(1, 0), (0, 1), (1, 1), (-1, 1)]
        for dr, dc in directions:
            count = 1
            for i in range(1, 4):
                r, c = row + i * dr, col + i * dc
                if 0 <= r < ROWS and 0 <= c < COLS and self.board[r][c] == self.current_player:
                    count += 1
                else:
                    break
            for i in range(1, 4):
                r, c = row - i * dr, col - i * dc
                if 0 <= r < ROWS and 0 <= c < COLS and self.board[r][c] == self.current_player:
                    count += 1
                else:
                    break
            if count >= 4:
                return True
        return False

    def is_board_full(self):
        return all(cell != "" for row in self.board for cell in row)

    def reset_board(self):
        for row in range(ROWS):
            for col in range(COLS):
                self.board[row][col] = ""
                self.buttons[row][col].config(text="")
        self.current_player = PLAYER_X

if __name__ == "__main__":
    root = tk.Tk()
    game = ConnectFour(root)
    root.mainloop()
