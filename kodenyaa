import pygame
import sys

pygame.init()

WIDTH, HEIGHT = 1080, 600  
FPS = 60
MAX_SCORE = 11

PADDLE_WIDTH, PADDLE_HEIGHT = 15, 100 
BALL_SIZE = 15
PADDLE_SPEED = 7 
BASE_BALL_SPEED_X = 6  
BASE_BALL_SPEED_Y = 5

PINK = (0, 71, 171)
BLACK = (255, 255, 255)
WHITE = (0, 0, 0)

screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("PingPong International - Max 11 Points")
clock = pygame.time.Clock()

title_font = pygame.font.Font(None, 100)
font = pygame.font.Font(None, 60)
small_font = pygame.font.Font(None, 36)
win_font = pygame.font.Font(None, 80)
countdown_font = pygame.font.Font(None, 120)

player1_y = HEIGHT // 2 - PADDLE_HEIGHT // 2
player2_y = HEIGHT // 2 - PADDLE_HEIGHT // 2
ball_pos = [WIDTH // 2 - BALL_SIZE // 2, HEIGHT // 2 - BALL_SIZE // 2]

ball_dx = BASE_BALL_SPEED_X
ball_dy = BASE_BALL_SPEED_Y

score1 = 0
score2 = 0
running = True
game_over = False
winner_text_str = ""

game_state = "MENU"

p1_name = ""
p2_name = ""    
active_input = 1  

countdown_timer = 0
countdown_active = False

def start_countdown():
    global countdown_timer, countdown_active
    countdown_timer = 3 * FPS
    countdown_active = True

def reset_ball(winner):
    global ball_pos, ball_dx, ball_dy
    ball_pos = [WIDTH // 2 - BALL_SIZE // 2, HEIGHT // 2 - BALL_SIZE // 2]
    ball_dx = BASE_BALL_SPEED_X if winner == 2 else -BASE_BALL_SPEED_X
    ball_dy = BASE_BALL_SPEED_Y
    start_countdown()

def reset_game()
    global score1, score2, player1_y, player2_y, game_over
    score1 = 0
    score2 = 0
    player1_y = HEIGHT // 2 - PADDLE_HEIGHT // 2
    player2_y = HEIGHT // 2 - PADDLE_HEIGHT // 2
    game_over = False
    reset_ball(1)

while running:
    clock.tick(FPS)  
    mouse_pos = pygame.mouse.get_pos()

    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
            
        if event.type == pygame.MOUSEBUTTONDOWN and event.button == 1:
            if game_state == "MENU":
                button_rect = pygame.Rect(WIDTH // 2 - 100, HEIGHT // 2 + 50, 200, 65)
                if button_rect.collidepoint(mouse_pos):
                    game_state = "INPUT_NAME"
                    active_input = 1

        if game_state == "INPUT_NAME":
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_RETURN:
                    if active_input == 1:
                        if p1_name.strip() == "": p1_name = "Player 1"
                        active_input = 2  
                    elif active_input == 2:
                        if p2_name.strip() == "": p2_name = "Player 2"
                        game_state = "GAMEPLAY"  
                        start_countdown()
                elif event.key == pygame.K_BACKSPACE:  
                    if active_input == 1:
                        p1_name = p1_name[:-1]
                    else:
                        p2_name = p2_name[:-1]
                else:
                    if event.unicode.isalnum() or event.unicode == " ":
                        if active_input == 1 and len(p1_name) < 12:
                            p1_name += event.unicode
                        elif active_input == 2 and len(p2_name) < 12:
                            p2_name += event.unicode

        elif game_state == "GAMEPLAY" and game_over:
            if event.type == pygame.KEYDOWN and event.key == pygame.K_SPACE:
                reset_game()

    if game_state == "GAMEPLAY":
        keys = pygame.key.get_pressed()
        
        if not game_over:
            if keys[pygame.K_w] and player1_y > 0:
                player1_y -= PADDLE_SPEED
            if keys[pygame.K_s] and player1_y < HEIGHT - PADDLE_HEIGHT:
                player1_y += PADDLE_SPEED

            if keys[pygame.K_UP] and player2_y > 0:
                player2_y -= PADDLE_SPEED
            if keys[pygame.K_DOWN] and player2_y < HEIGHT - PADDLE_HEIGHT:
                player2_y += PADDLE_SPEED

            if countdown_active:
                countdown_timer -= 1
                if countdown_timer <= 0:
                    countdown_active = False
            else:
                ball_pos[0] += ball_dx
                ball_pos[1] += ball_dy

                if ball_pos[1] <= 0 or ball_pos[1] >= HEIGHT - BALL_SIZE:
                    ball_dy *= -1

                ball_rect = pygame.Rect(ball_pos[0], ball_pos[1], BALL_SIZE, BALL_SIZE)
                p1_rect = pygame.Rect(20, player1_y, PADDLE_WIDTH, PADDLE_HEIGHT)
                p2_rect = pygame.Rect(WIDTH - 20 - PADDLE_WIDTH, player2_y, PADDLE_WIDTH, PADDLE_HEIGHT)

                if ball_rect.colliderect(p1_rect) and ball_dx < 0:
                    ball_dx = -ball_dx + 0.5  
                if ball_rect.colliderect(p2_rect) and ball_dx > 0:
                    ball_dx = -ball_dx - 0.5  

                if ball_pos[0] < 0:
                    score2 += 1
                    if score2 >= MAX_SCORE:
                        winner_text_str = f"{p2_name} MENANG! [Space]"
                        game_over = True
                    else:
                        reset_ball(2)
                elif ball_pos[0] > WIDTH:
                    score1 += 1
                    if score1 >= MAX_SCORE:
                        winner_text_str = f"{p1_name} MENANG! [Space]"
                        game_over = True
                    else:
                        reset_ball(1)

    screen.fill(PINK)

    if game_state == "MENU":
        title_text = title_font.render("PINGPONG", True, BLACK)
        title_rect = title_text.get_rect(center=(WIDTH // 2, HEIGHT // 2 - 120))
        screen.blit(title_text, title_rect)

        p1_control = small_font.render("P1: W / S Keys", True, BLACK)
        p2_control = small_font.render("P2: UP / DOWN Arrows", True, BLACK)
        screen.blit(p1_control, (WIDTH // 2 - 250, HEIGHT // 2 - 20))
        screen.blit(p2_control, (WIDTH // 2 + 50, HEIGHT // 2 - 20))

        button_rect = pygame.Rect(WIDTH // 2 - 100, HEIGHT // 2 + 50, 200, 65)
        pygame.draw.rect(screen, PINK, button_rect)
        pygame.draw.rect(screen, BLACK, button_rect, 4)

        play_text = font.render("PLAY", True, BLACK)
        play_rect = play_text.get_rect(center=button_rect.center)
        screen.blit(play_text, play_rect)

    elif game_state == "INPUT_NAME":
        input_title = font.render("MASUKKAN NAMA PEMAIN", True, BLACK)
        screen.blit(input_title, input_title.get_rect(center=(WIDTH // 2, HEIGHT // 2 - 150)))

        p1_box = pygame.Rect(WIDTH // 2 - 250, HEIGHT // 2 - 30, 500, 50)
        pygame.draw.rect(screen, BLACK if active_input == 1 else WHITE, p1_box, 2)
        p1_txt = font.render(f"P1: {p1_name}", True, BLACK)
        screen.blit(p1_txt, (WIDTH // 2 - 240, HEIGHT // 2 - 25))

        p2_box = pygame.Rect(WIDTH // 2 - 250, HEIGHT // 2 + 40, 500, 50)
        pygame.draw.rect(screen, BLACK if active_input == 2 else WHITE, p2_box, 2)
        p2_txt = font.render(f"P2: {p2_name}", True, BLACK)
        screen.blit(p2_txt, (WIDTH // 2 - 240, HEIGHT // 2 + 45))

        info_txt = small_font.render("Tekan ENTER untuk konfirmasi nama", True, BLACK)
        screen.blit(info_txt, info_txt.get_rect(center=(WIDTH // 2, HEIGHT // 2 + 140)))

    elif game_state == "GAMEPLAY":
        # Garis Tengah Lapangan
        pygame.draw.line(screen, BLACK, (WIDTH // 2, 0), (WIDTH // 2, HEIGHT), 2)

        score_text = font.render(f"{p1_name}: {score1}   |   {p2_name}: {score2}", True, BLACK)
        screen.blit(score_text, score_text.get_rect(center=(WIDTH // 2, 40)))

        pygame.draw.rect(screen, BLACK, (20, player1_y, PADDLE_WIDTH, PADDLE_HEIGHT))
        pygame.draw.rect(screen, BLACK, (WIDTH - 20 - PADDLE_WIDTH, player2_y, PADDLE_WIDTH, PADDLE_HEIGHT))
        pygame.draw.rect(screen, BLACK, (ball_pos[0], ball_pos[1], BALL_SIZE, BALL_SIZE))

        if countdown_active:
            seconds = int((countdown_timer - 1) // FPS) + 1
            cd_text = countdown_font.render(str(seconds), True, BLACK)
        if countdown_active:
            seconds = int((countdown_timer - 1) // FPS) + 1
            cd_text = countdown_font.render(str(seconds), True, BLACK)
            screen.blit(cd_text, cd_text.get_rect(center=(WIDTH // 2, HEIGHT // 2)))

        if game_over:
            win_text = win_font.render(winner_text_str, True, WHITE)
            win_rect = win_text.get_rect(center=(WIDTH // 2, HEIGHT // 2))
            
            bg_rect = pygame.Rect(0, 0, win_rect.width + 40, win_rect.height + 20)
            bg_rect.center = win_rect.center
            pygame.draw.rect(screen, BLACK, bg_rect)
            screen.blit(win_text, win_rect)

    pygame.display.flip()

pygame.quit()
sys.exit() 
