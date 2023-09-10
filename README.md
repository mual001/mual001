import random

# Initialize the game board
def initialize_board():
    board = [[0] * 4 for _ in range(4)]
    add_random_tile(board)
    add_random_tile(board)
    return board

# Print the game board
def print_board(board):
    for row in board:
        print(" ".join(map(str, row)))
    print()

# Add a random tile (2 or 4) to an empty cell
def add_random_tile(board):
    empty_cells = [(i, j) for i in range(4) for j in range(4) if board[i][j] == 0]
    if empty_cells:
        i, j = random.choice(empty_cells)
        board[i][j] = random.choice([2, 4])

# Move tiles left
def move_left(board):
    for row in board:
        row[:] = merge_tiles(row)

# Move tiles right
def move_right(board):
    for row in board:
        row[:] = merge_tiles(row[::-1])[::-1]

# Move tiles up
def move_up(board):
    transpose(board)
    move_left(board)
    transpose(board)

# Move tiles down
def move_down(board):
    transpose(board)
    move_right(board)
    transpose(board)

# Merge tiles in a row
def merge_tiles(row):
    new_row = [tile for tile in row if tile != 0]
    for i in range(len(new_row) - 1):
        if new_row[i] == new_row[i + 1]:
            new_row[i] *= 2
            new_row[i + 1] = 0
    new_row = [tile for tile in new_row if tile != 0] + [0] * row.count(0)
    return new_row

# Transpose the game board
def transpose(board):
    for i in range(4):
        for j in range(i + 1, 4):
            board[i][j], board[j][i] = board[j][i], board[i][j]

# Check if the game is over
def is_game_over(board):
    for row in board:
        if 2048 in row:
            print("You won!")
            return True
    if not any(0 in row for row in board) and not can_merge(board):
        print("Game over!")
        return True
    return False

# Check if merging is possible
def can_merge(board):
    for i in range(4):
        for j in range(3):
            if board[i][j] == board[i][j + 1] or board[j][i] == board[j + 1][i]:
                return True
    return False

# Main game loop
def main():
    board = initialize_board()

    while True:
        print_board(board)
        move = input("Enter a move (W/A/S/D): ").upper()
        
        if move == 'W':
            move_up(board)
        elif move == 'A':
            move_left(board)
        elif move == 'S':
            move_down(board)
        elif move == 'D':
            move_right(board)
        else:
            print("Invalid move. Use W/A/S/D.")
            continue

        if not is_game_over(board):
            add_random_tile(board)

if __name__ == "__main__":
    main()
