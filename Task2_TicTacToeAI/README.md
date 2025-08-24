import random

def print_board(board):
    for row in board:
        print(row)

def check_win(board, player):
    for row in board:
        if all([cell == player for cell in row]):
            return True
    for col in range(3):
        if all([board[row][col] == player for row in range(3)]):
            return True
    if all([board[i][i] == player for i in range(3)]) or all([board[i][2-i] == player for i in range(3)]):
        return True
    return False

def tic_tac_toe():
    board = [[" "]*3 for _ in range(3)]
    while True:
        print_board(board)
        row, col = map(int, input("Enter row and column (0-2): ").split())
        if board[row][col] != " ":
            print("Invalid move!")
            continue
        board[row][col] = "X"
        if check_win(board, "X"):
            print_board(board)
            print("You win!")
            break
        empty = [(r,c) for r in range(3) for c in range(3) if board[r][c]==" "]
        if not empty:
            print_board(board)
            print("Draw!")
            break
        move = random.choice(empty)
        board[move[0]][move[1]] = "O"
        if check_win(board, "O"):
            print_board(board)
            print("AI wins!")
            break

tic_tac_toe()
