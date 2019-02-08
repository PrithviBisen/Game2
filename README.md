# Game2
import arcade
import os
import random

Sprite_Scaling = 0.2
Candy_Scaling = 0.1
candy_COUNT = 50

SCREEN_WIDTH = 600
SCREEN_HIGHT = 600
SCREEN_TITLE = "GAME"

MOVEMENT_SPEED = 5

TEXTURE_LEFT = 0
TEXTURE_RIGHT = 1

class Player(arcade.Sprite):

    def __init__(self):
        super().__init__()

        texture = arcade.load_texture(r'C:\Users\Asus\Desktop\character.png', mirrored= True , scale=Sprite_Scaling)
        self.textures.append(texture)
        texture = arcade.load_texture(r'C:\Users\Asus\Desktop\character.png', scale=Sprite_Scaling)
        self.textures.append(texture)

        self.set_texture(TEXTURE_RIGHT)

    def update(self):
        self.center_x += self.change_x
        self.center_y += self.change_y

        if self.change_x < 0:
            self.set_texture(TEXTURE_LEFT)
        if self.change_x > 0:
            self.set_texture(TEXTURE_RIGHT)

        if self.left < 0 :
            self.left = 0
        elif self.right > SCREEN_WIDTH -1 :
            self.right = SCREEN_WIDTH -1

        if self.bottom < 0 :
            self.bottom = 0
        elif self.top > SCREEN_HIGHT -1 :
            self.top = SCREEN_HIGHT -1
class MyGame(arcade.Window):

    def __init__(self, width, height, title):
        super().__init__(width,height,title)

        arcade.set_background_color(arcade.color.DUTCH_WHITE)

        file_path = os.path.dirname(os.path.abspath(__file__))
        os.chdir(file_path)

        self.player_list = None
        self.candy_list = None
        self.player_sprite = None
        self.score = 0

        self.left_pressed = False
        self.right_pressed = False
        self.up_pressed = False
        self.down_pressed = False



    def setup(self):

        self.player_list = arcade.SpriteList()
        self.candy_list = arcade.SpriteList()
        self.score = 0
        self.player_sprite = Player()
        self.player_sprite.center_y = 50
        self.player_sprite.center_x = 50
        self.player_list.append(self.player_sprite)

        for i in range(candy_COUNT):

            candy = arcade.Sprite(r'C:\Users\Asus\Desktop\candy.png', Candy_Scaling)

            candy.center_x = random.randrange(SCREEN_WIDTH)
            candy.center_y = random.randrange(SCREEN_HIGHT)

            self.candy_list.append(candy)

    def on_draw(self):

        arcade.start_render()
        self.player_list.draw()
        self.candy_list.draw()

        output = f"Score: {self.score}"
        arcade.draw_text(output, 10, 20, arcade.color.BLACK, 14)

    def update(self, delta_time):

        self.candy_list.update()
        self.player_sprite.change_x = 0
        self.player_sprite.change_y = 0

        if self.up_pressed and not self.down_pressed:
            self.player_sprite.change_y = MOVEMENT_SPEED
        elif self.down_pressed and not self.up_pressed:
            self.player_sprite.change_y = -MOVEMENT_SPEED
        if self.left_pressed and not self.right_pressed:
            self.player_sprite.change_x = -MOVEMENT_SPEED
        elif self.right_pressed and not self.left_pressed:
            self.player_sprite.change_x = MOVEMENT_SPEED
        self.player_list.update()

        candys_hit_list = arcade.check_for_collision_with_list(self.player_sprite, self.candy_list)

        for candy in candys_hit_list:
            candy.kill()
            self.score += 1

    def on_key_press(self, key, modifiers):

        if key == arcade.key.UP:
            self.up_pressed = True
        elif key == arcade.key.DOWN:
            self.down_pressed = True
        elif key == arcade.key.LEFT:
            self.left_pressed = True
        elif key == arcade.key.RIGHT:
            self.right_pressed = True

    def on_key_release(self, key, modifiers):

        if key == arcade.key.UP:
            self.up_pressed = False
        elif key == arcade.key.DOWN:
            self.down_pressed = False
        elif key == arcade.key.LEFT:
            self.left_pressed = False
        elif key == arcade.key.RIGHT:
            self.right_pressed = False
def main():

    window = MyGame(SCREEN_HIGHT, SCREEN_WIDTH, SCREEN_TITLE)
    window.setup()
    arcade.run()
if __name__ == "__main__":
    main()
