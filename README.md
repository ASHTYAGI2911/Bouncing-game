# Bouncing-game
# Ball bouncing game using python tkinter

### Code is as below

from tkinter import *
import random
import time
from tkinter import messagebox
root = Tk()
root.title("SHIV")
root.resizable(0,0)
root.wm_attributes('-topmost',1)
canvas = Canvas(root, height=500, width=500, bd=0, highlightthickness=0)
canvas.pack()
root.update()

class Ball:
    def __init__(self, canvas, paddle, color):
        self.canvas = canvas
        self.paddle = paddle
        self.id = canvas.create_oval(10,10, 25,25, fill = color)
        self.canvas.move(self.id, 245, 100)
        start = [-3,-2,-1,0,1,2,3]
        self.x = start[0]
        self.y = -15
        self.canvas_height = self.canvas.winfo_height()
        self.canvas_width = self.canvas.winfo_width()
        self.hit_bottom = False
    
    def hit_paddle(self, pos):
        paddle_pos = self.canvas.coords(self.paddle.id)
        if pos[2] >= paddle_pos[0] and pos[0] <= paddle_pos[2]:
            if pos[3] >= paddle_pos[1] and pos[3] <= paddle_pos[3]:
                return True
            return False

    def draw(self):
        self.canvas.move(self.id, self.x, self.y)
        pos = self.canvas.coords(self.id)
        if pos[1] <= 0:
            self.y = 15
        if pos[3] >= self.canvas_height:
            self.hit_bottom = True
            canvas.create_text(245, 100, text="Game Over")
            self.y = 0
        if pos[0] <= 0:
            self.x = 15
        if pos[2] >= self.canvas_width:
            self.x = -15
        if self.hit_paddle(pos) == True:
            self.y = -15            

class Paddle:
    def __init__(self, canvas, color):
        self.canvas = canvas
        self.id = canvas.create_rectangle(0,0, 100, 10, fill=color)
        self.canvas.move(self.id, 200, 300)
        self.x = 40
        self.canvas_width = self.canvas.winfo_width()
        self.canvas_height = self.canvas.winfo_height()
        self.canvas.bind_all("<KeyPress-Right>", self.moveright)
        self.canvas.bind_all("<KeyPress-Left>", self.moveleft)
    def draw(self):
        self.canvas.move(self.id, self.x, 0)
        pos = self.canvas.coords(self.id)
        if pos[2] >= self.canvas_width:
            self.x = 0
        if pos[0] <= 0:
            self.x = 0
    def moveleft(self, evt):
        self.x = -20
    def moveright(self, evt):
        self.x = 20

paddle = Paddle(canvas, "Blue")
ball = Ball(canvas, paddle, "Red")

while 1:
    if ball.hit_bottom == False:
        ball.draw()
        paddle.draw()
        root.update()
        root.update_idletasks()
        time.sleep(0.2)
    else:
        root.destroy()
        break

root.mainloop()
