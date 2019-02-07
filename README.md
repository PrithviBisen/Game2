# Game2
import arcade
import os

Sprite_Scaling = 0.05
SCREEN_WIDTH = 600
SCREEN_HIGHT = 600
SCREEN_TITLE = "GAME"

MOVEMENT_SPEED = 5

class Player(arcade.Sprite):

    def update(self):
        self.center_x += self.change_x
        self.center_y += self.change_y

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

        arcade.set_background_color(arcade.color.BLACK)

    def setup(self):

        self.player_list = arcade.SpriteList()

        self.score = 0
        self.player_sprite = Player("/home/robuntu/Desktop/red.png", Sprite_Scaling )
        self.player_sprite.center_y = 50
        self.player_sprite.center_x = 50
        self.player_list.append(self.player_sprite)

    def on_draw(self):

        arcade.start_render()
        self.player_list.draw()

    def update(self, delta_time):

        self.player_list.update()

    def on_key_press(self, key, modifiers):

        if key == arcade.key.UP:
            self.player_sprite.change_y = MOVEMENT_SPEED
        elif key == arcade.key.DOWN:
            self.player_sprite.change_y = -MOVEMENT_SPEED
        elif key == arcade.key.LEFT:
            self.player_sprite.change_x = -MOVEMENT_SPEED
        elif key == arcade.key.RIGHT:
            self.player_sprite.change_x = MOVEMENT_SPEED

    def on_key_release(self, key, modifiers):

        if key == arcade.key.UP or key == arcade.key.DOWN:
            self.player_sprite.change_y = 0
        elif key == arcade.key.LEFT or key == arcade.key.RIGHT:
            self.player_sprite.change_x = 0


def main():

    window = MyGame(SCREEN_HIGHT, SCREEN_WIDTH, SCREEN_TITLE)
    window.setup()
    arcade.run()

if __name__ == "__main__":
    main()

change
