import pygame

pygame.init()

SCREEN_WIDTH = 582
SCREEN_HEIGHT = 500
screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
pygame.display.set_caption("Fleetway Animation")

sprite_sheet_image = pygame.image.load('Fleetway running.png').convert_alpha()
BG = (50, 50, 50)

# ✅ Correct frame dimensions
frame_width = 36
frame_height = 80  # Full body height
scale_factor = 3   # Make sprite visible and large

# ✅ Extract and scale all frames (NO FLIP)
frames = []
for i in range(18):  # 18 frames in your sheet
    x = i * frame_width
    frame = pygame.Surface((frame_width, frame_height), pygame.SRCALPHA)
    frame.blit(sprite_sheet_image, (0, 0), pygame.Rect(x, 0, frame_width, frame_height))
    
    # ✅ Scale up the sprite
    scaled_frame = pygame.transform.scale(frame, (frame_width * scale_factor, frame_height * scale_factor))
    
    frames.append(scaled_frame)

clock = pygame.time.Clock()
frame_index = 0
animation_speed = 0.2
frame_timer = 0

run = True
while run:
    screen.fill(BG)

    # ✅ Animate frame
    frame_timer += animation_speed
    if frame_timer >= 1:
        frame_index = (frame_index + 1) % len(frames)
        frame_timer = 0

    sprite = frames[frame_index]
    sprite_rect = sprite.get_rect(center=(SCREEN_WIDTH // 2, SCREEN_HEIGHT // 2))
    screen.blit(sprite, sprite_rect)

    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            run = False

    pygame.display.update()
    clock.tick(60)

pygame.quit()
