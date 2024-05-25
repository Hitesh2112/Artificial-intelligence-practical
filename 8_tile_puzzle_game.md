## 8-Tile Puzzle
```python
# Importing necessary libraries
import pygame
import sys
import random
from pygame.locals import *

# Global variables
w_of_board = 3
h_of_board = 3
block_size = 80
win_width = 640
win_height = 480
FPS = 30
BLANK = None

# Colors
BLACK = (0, 0, 0)
WHITE = (255, 255, 255)
DARKTURQUOISE = (0, 128, 128)
BLUE = (0, 0, 255)
GREEN = (0, 130, 0)
RED = (255, 0, 0)
BGCOLOR = BLACK

TILECOLOR = BLUE
TEXTCOLOR = WHITE
BORDERCOLOR = RED
BASICFONTSIZE = 20
TEXT = GREEN
BUTTONCOLOR = WHITE
BUTTONTEXTCOLOR = BLACK
MESSAGECOLOR = GREEN

# Margins
XMARGIN = int((win_width - (block_size * w_of_board + (w_of_board - 1))) / 2)
YMARGIN = int((win_height - (block_size * h_of_board + (h_of_board - 1))) / 2)

# Moves
UP = 'up'
DOWN = 'down'
LEFT = 'left'
RIGHT = 'right'

# Pygame setup
pygame.init()
FPSCLOCK = pygame.time.Clock()
DISPLAYSURF = pygame.display.set_mode((win_width, win_height))
pygame.display.set_caption('8-Tile Slide Puzzle')
BASICFONT = pygame.font.Font('freesansbold.ttf', BASICFONTSIZE)

def makeText(text, color, bgcolor, top, left):
    text_rendering = BASICFONT.render(text, True, color, bgcolor)
    text_in_rect = text_rendering.get_rect()
    text_in_rect.topleft = (top, left)
    return (text_rendering, text_in_rect)


# UI Elements
RESET_SURF, RESET_RECT = makeText('Reset', TEXT, BGCOLOR, win_width - 120, win_height - 310)
NEW_SURF, NEW_RECT = makeText('New Game', TEXT, BGCOLOR, win_width - 120, win_height - 280)
SOLVE_SURF, SOLVE_RECT = makeText('Solve', TEXT, BGCOLOR, win_width - 120, win_height - 250)

def main():
    global FPSCLOCK, DISPLAYSURF, BASICFONT, RESET_SURF, RESET_RECT, NEW_SURF, NEW_RECT, SOLVE_SURF, SOLVE_RECT

    mainBoard, solutionSeq = generateNewPuzzle(80)
    SOLVEDBOARD = start_playing()
    allMoves = []

    while True:
        slideTo = None
        msg = 'Click a block or press arrow keys to slide the block. Heuristic: {}'.format(
            calculateHeuristic(mainBoard, SOLVEDBOARD)
        )

        if mainBoard == SOLVEDBOARD:
            msg = 'Solved!'
            print("Puzzle Solved!\n")
            break  # Exit the loop when the puzzle is solved

        drawBoard(mainBoard, msg)

        check_exit_req()

        for event in pygame.event.get():
            if event.type == MOUSEBUTTONUP:
                spotx, spoty = getSpotClicked(mainBoard, event.pos[0], event.pos[1])

                if (spotx, spoty) == (None, None):
                    if RESET_RECT.collidepoint(event.pos):
                        rst_animation(mainBoard, allMoves)
                        allMoves = []
                    elif NEW_RECT.collidepoint(event.pos):
                        mainBoard, solutionSeq = generateNewPuzzle(80)
                        allMoves = []
                    elif SOLVE_RECT.collidepoint(event.pos):
                        rst_animation(mainBoard, solutionSeq + allMoves)
                        allMoves = []
                else:
                    blankx, blanky = getBlankPosition(mainBoard)
                    if spotx == blankx + 1 and spoty == blanky:
                        slideTo = LEFT
                    elif spotx == blankx - 1 and spoty == blanky:
                        slideTo = RIGHT
                    elif spotx == blankx and spoty == blanky + 1:
                        slideTo = UP
                    elif spotx == blankx and spoty == blanky - 1:
                        slideTo = DOWN

            elif event.type == KEYUP:
                if event.key in (K_LEFT, K_a) and isValidMove(mainBoard, LEFT):
                    slideTo = LEFT
                elif event.key in (K_RIGHT, K_d) and isValidMove(mainBoard, RIGHT):
                    slideTo = RIGHT
                elif event.key in (K_UP, K_w) and isValidMove(mainBoard, UP):
                    slideTo = UP
                elif event.key in (K_DOWN, K_s) and isValidMove(mainBoard, DOWN):
                    slideTo = DOWN

        if slideTo:
            sliding_animation(mainBoard, slideTo, 'Click a block or press arrow keys to slide the block. Heuristic: {}'.format(
                calculateHeuristic(mainBoard, SOLVEDBOARD)), 8)
            take_turn(mainBoard, slideTo)
            allMoves.append(slideTo)
        pygame.display.update()
        FPSCLOCK.tick(FPS)


# Function to calculate heuristic value
def calculateHeuristic(board, solvedBoard):
    heuristic = 0
    for x in range(w_of_board):
        for y in range(h_of_board):
            if board[x][y] != solvedBoard[x][y]:
                heuristic += 1
    return heuristic



def terminate():
    pygame.quit()
    sys.exit()


def check_exit_req():
    # get all the QUIT events
    for event in pygame.event.get(QUIT):
        # terminate() will kill all the events. terminate if any QUIT events are present
        terminate()
    # this for loop will get all the KEYUP events
    for event in pygame.event.get(KEYUP):
        if event.key == K_ESCAPE:
            # if the user presses the ESC key then it will terminate the session and if the KEYUP event was for the Esc key
            terminate()
        # put the other KEYUP event objects back
        pygame.event.post(event)


def start_playing():
    # Return a board structure with blocks in the solved state.
    counter = 1
    board = []
    for x in range(w_of_board):
        column = []
        for y in range(h_of_board):
            column.append(counter)
            counter += w_of_board
        board.append(column)
        counter -= w_of_board * (h_of_board - 1) + w_of_board - 1

    board[w_of_board-1][h_of_board-1] = BLANK
    return board


def getBlankPosition(board):
    # Return the x and y of board coordinates of the blank space.
    for x in range(w_of_board):
        for y in range(h_of_board):
            if board[x][y] == BLANK:
                return (x, y)


def take_turn(board, move):
    blankx, blanky = getBlankPosition(board)

    if move == UP:
        board[blankx][blanky], board[blankx][blanky + 1] = board[blankx][blanky + 1], board[blankx][blanky]
    elif move == DOWN:
        board[blankx][blanky], board[blankx][blanky - 1] = board[blankx][blanky - 1], board[blankx][blanky]
    elif move == LEFT:
        board[blankx][blanky], board[blankx + 1][blanky] = board[blankx + 1][blanky], board[blankx][blanky]
    elif move == RIGHT:
        board[blankx][blanky], board[blankx - 1][blanky] = board[blankx - 1][blanky], board[blankx][blanky]

    print("\nState after move : {}".format(move))
    l1=list(zip(*board))
    for row in l1:
        print(row)
        



def isValidMove(board, move):
    blankx, blanky = getBlankPosition(board)
    return (move == UP and blanky != len(board[0]) - 1) or \
           (move == DOWN and blanky != 0) or \
           (move == LEFT and blankx != len(board) - 1) or \
           (move == RIGHT and blankx != 0)


def ramdom_moves(board, lastMove=None):
    # start with a full list of all four moves
    validMoves = [UP, DOWN, LEFT, RIGHT]

    # remove moves from the list as they are disqualified
    if lastMove == UP or not isValidMove(board, DOWN):
        validMoves.remove(DOWN)
    if lastMove == DOWN or not isValidMove(board, UP):
        validMoves.remove(UP)
    if lastMove == LEFT or not isValidMove(board, RIGHT):
        validMoves.remove(RIGHT)
    if lastMove == RIGHT or not isValidMove(board, LEFT):
        validMoves.remove(LEFT)

    # this will perform the return nad it will return a random move from the list of remaining moves
    return random.choice(validMoves)


def getLeftTopOfTile(block_x, block_y):
    left = XMARGIN + (block_x * block_size) + (block_x - 1)
    top = YMARGIN + (block_y * block_size) + (block_y - 1)
    return (left, top)


def getSpotClicked(board, x, y):
    # from the x & y pixel coordinates, this for loop below will get the x & y board coordinates
    for block_x in range(len(board)):
        for block_y in range(len(board[0])):
            left, top = getLeftTopOfTile(block_x, block_y)
            tileRect = pygame.Rect(left, top, block_size, block_size)
            if tileRect.collidepoint(x, y):
                return (block_x, block_y)
    return (None, None)


def draw_block(block_x, block_y, number, adjx=0, adjy=0):
    # draw a tile at board coordinates block_x and block_y, optionally a few
    left, top = getLeftTopOfTile(block_x, block_y)
    pygame.draw.rect(DISPLAYSURF, TILECOLOR, (left + adjx,
                     top + adjy, block_size, block_size))
    text_renderign = BASICFONT.render(str(number), True, TEXTCOLOR)
    text_in_rect = text_renderign.get_rect()
    text_in_rect.center = left + \
        int(block_size / 2) + adjx, top + int(block_size / 2) + adjy
    DISPLAYSURF.blit(text_renderign, text_in_rect)


# this function will draw the board wherein the player can play.
# it holds the code for displaying different color and logic behind the game


def drawBoard(board, message):
    DISPLAYSURF.fill(BGCOLOR)
    if message:
        text_renderign, text_in_rect = makeText(
            message, MESSAGECOLOR, BGCOLOR, 5, 5)
        DISPLAYSURF.blit(text_renderign, text_in_rect)

    for block_x in range(len(board)):
        for block_y in range(len(board[0])):
            if board[block_x][block_y]:
                draw_block(block_x, block_y, board[block_x][block_y])

    left, top = getLeftTopOfTile(0, 0)
    width = w_of_board * block_size
    height = h_of_board * block_size
    pygame.draw.rect(DISPLAYSURF, BORDERCOLOR, (left - 5,
                     top - 5, width + 11, height + 11), 4)
    

    DISPLAYSURF.blit(RESET_SURF, RESET_RECT)
    DISPLAYSURF.blit(NEW_SURF, NEW_RECT)
    DISPLAYSURF.blit(SOLVE_SURF, SOLVE_RECT)

# this function is to handle the animation that are displayed when a user starts a new Game
# a user can see the sliding animation over the blocks
# this is made possible using the below function


def sliding_animation(board, direction, message, animationSpeed):
    blankx, blanky = getBlankPosition(board)
    if direction == UP:
        move_in_xaxis = blankx
        move_in_yaxis = blanky + 1
    elif direction == DOWN:
        move_in_xaxis = blankx
        move_in_yaxis = blanky - 1
    elif direction == LEFT:
        move_in_xaxis = blankx + 1
        move_in_yaxis = blanky
    elif direction == RIGHT:
        move_in_xaxis = blankx - 1
        move_in_yaxis = blanky

    # prepare the base surface
    drawBoard(board, message)
    baseSurf = DISPLAYSURF.copy()
    # draw a blank space over the moving block on the baseSurf Surface.
    take_left, take_top = getLeftTopOfTile(move_in_xaxis, move_in_yaxis)
    pygame.draw.rect(baseSurf, BGCOLOR, (take_left,
                     take_top, block_size, block_size))

    for i in range(0, block_size, animationSpeed):
        # this is to handle the animation of the tile sliding over
        check_exit_req()
        DISPLAYSURF.blit(baseSurf, (0, 0))
        if direction == UP:
            draw_block(move_in_xaxis, move_in_yaxis,
                       board[move_in_xaxis][move_in_yaxis], 0, -i)
        if direction == DOWN:
            draw_block(move_in_xaxis, move_in_yaxis,
                       board[move_in_xaxis][move_in_yaxis], 0, i)
        if direction == LEFT:
            draw_block(move_in_xaxis, move_in_yaxis,
                       board[move_in_xaxis][move_in_yaxis], -i, 0)
        if direction == RIGHT:
            draw_block(move_in_xaxis, move_in_yaxis,
                       board[move_in_xaxis][move_in_yaxis], i, 0)

        pygame.display.update()
        FPSCLOCK.tick(FPS)


def generateNewPuzzle(numSlides):
    # this to display the animation of blocks
    sequence = []
    board = start_playing()
    drawBoard(board, '')
    pygame.display.update()
    # we used time.wait() to pause 500 milliseconds for effect
    pygame.time.wait(500)
    lastMove = None
    for i in range(numSlides):
        move = ramdom_moves(board, lastMove)
        sliding_animation(board, move, 'Generating new puzzle...',
                          animationSpeed=int(block_size / 3))
        take_turn(board, move)
        sequence.append(move)
        lastMove = move
    return (board, sequence)


def rst_animation(board, allMoves):
    # make all of the moves in reverse
    reverse_moves = allMoves[:]
    reverse_moves.reverse()

    for move in reverse_moves:
        if move == UP:
            opp_moves = DOWN
        elif move == DOWN:
            opp_moves = UP
        elif move == RIGHT:
            opp_moves = LEFT
        elif move == LEFT:
            opp_moves = RIGHT
        sliding_animation(board, opp_moves, '',
                          animationSpeed=int(block_size / 2))
        take_turn(board, opp_moves)


# this is the call to main fucntion

if __name__ == '__main__':
    main()


```

### OUTPUT
![image](https://github.com/Hitesh2112/Artificial-intelligence-practical/assets/97521900/b9a7dc70-3d06-4e16-ab15-8ebb6d9298e0)
![image](https://github.com/Hitesh2112/Artificial-intelligence-practical/assets/97521900/31e10813-099d-441f-b2fe-c63bc11485d5)
