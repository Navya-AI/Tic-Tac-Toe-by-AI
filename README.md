# Define Tic-Tac-Toe board and display function
board = [' ' for _ in range(9)]

def display_board():
    for row in [board[i*3:(i+1)*3] for i in range(3)]:
        print('| ' + ' | '.join(row) + ' |')

# Check for a winner
def check_winner(board, player):
    win_conditions = [(0,1,2), (3,4,5), (6,7,8), (0,3,6), (1,4,7), (2,5,8), (0,4,8), (2,4,6)]
    return any(board[i] == board[j] == board[k] == player for i, j, k in win_conditions)

# Minimax function for AI decision-making
def minimax(board, depth, is_maximizing):
    if check_winner(board, 'O'):
        return 1
    elif check_winner(board, 'X'):
        return -1
    elif ' ' not in board:
        return 0

    if is_maximizing:
        best_score = -float('inf')
        for i in range(9):
            if board[i] == ' ':
                board[i] = 'O'
                score = minimax(board, depth + 1, False)
                board[i] = ' '
                best_score = max(score, best_score)
        return best_score
    else:
        best_score = float('inf')
        for i in range(9):
            if board[i] == ' ':
                board[i] = 'X'
                score = minimax(board, depth + 1, True)
                board[i] = ' '
                best_score = min(score, best_score)
        return best_score

# AI chooses the best move
def best_move():
    best_score = -float('inf')
    move = 0
    for i in range(9):
        if board[i] == ' ':
            board[i] = 'O'
            score = minimax(board, 0, False)
            board[i] = ' '
            if score > best_score:
                best_score = score
                move = i
    board[move] = 'O'

# Main game loop
while True:
    display_board()
    user_move = int(input("Choose your move (0-8): "))
    if board[user_move] == ' ':
        board[user_move] = 'X'
    if check_winner(board, 'X'):
        display_board()
        print("You win!")
        break
    elif ' ' not in board:
        display_board()
        print("It's a draw!")
        break
    best_move()
    if check_winner(board, 'O'):
        display_board()
        print("AI wins!")
        break
